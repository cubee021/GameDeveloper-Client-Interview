## IsValid, == nullptr, check 차이

### IsValid

```cpp
FORCEINLINE bool IsValid(const UObject *Test)
{
	return Test && FInternalUObjectBaseUtilityIsValidFlagsChecker::CheckObjectValidBasedOnItsFlags(Test);
}
```

`IsValid`의 경우 단순 nullptr 체크 뿐 아니라 내부 플래그도 검사하는 작업을 거치게 됩니다.

```cpp
struct FInternalUObjectBaseUtilityIsValidFlagsChecker
{
	FORCEINLINE static bool CheckObjectValidBasedOnItsFlags(const UObject* Test)
	{
		// Here we don't really check if the flags match but if the end result is the same
		PRAGMA_DISABLE_DEPRECATION_WARNINGS
		checkSlow(GUObjectArray.IndexToObject(Test->InternalIndex)->HasAnyFlags(EInternalObjectFlags::PendingKill | EInternalObjectFlags::Garbage) == Test->HasAnyFlags(RF_InternalPendingKill | RF_InternalGarbage));
		PRAGMA_ENABLE_DEPRECATION_WARNINGS
		return !Test->HasAnyFlags(RF_InternalPendingKill | RF_InternalGarbage);
	}
};
```

내부 플래그는 Functor로 되어 있는데 `PendingKill`, `InternalGarbage`인지 체크하는 작업을 거치게 됩니다.



즉 **IsValid는 nullptr인지, 가비지인지, PendingKill인지 살펴보는 작업을 거칩니다.**
> PendingKill = '곧 파괴될 오브젝트'
> 
> 즉, 메모리에는 남아 있지만 곧이어 삭제될 오브젝트를 나타내는 플래그


### check, ensure 등의 어써트 (Assert)

이런 Assert 함수들은 에디터에서만 동작하는 함수로 안의 내용이 true, false 인지에 따라 동작이 달라지게 됩니다. 이들은 포인터의 operator bool() 연산에 기반해서 결과를 내므로 결론적으로는 nullptr와 비교하는 것과 동일한 결과를 내게 됩니다.

```cpp
/*
"실행 중지", "실행 중지하지 않고 오류 보고" : DO_CHECK define에 따라 컴파일
"디버그 빌드에서 실행 중지" : DO_GUARD_SLOW define에 따라 컴파일
*/

// check 는 false 값이면 "실행 중지"
check(Mesh != nullptr);
checkf(MyPtr, TEXT("MyPtr is nullptr")); // false 이면 로그 출력
checkcode(MyPtr, TEXT("MyPtr is nullptr")); // checkf 와 비슷
checkNoEntry(); // 절대 실행될 리 없는 경로 표시
checkNoReentry(); // 호출 완료 전까지 다시 호출되면 안되는 경우 체크
checkNoRecursion(); // checkNoReentry() 와 동일
unimplemented(); // 반드시 재정의해야 하는 가상 함수를 표시하기 위해 사용


// verify 는 DO_CHECK = 1 이 아니어도 실행. 0일 경우 "디버그 빌드에서 실행 중지"
verify(Mesh != nullptr); // 이하 다른 method 는 check와 유사


// ensure 은 콜스택 생성. "실행 중지하지 않고 오류 보고"
ensure(MyPtr != nullptr); // 이 지점까지의 콜스택 생성
ensureMsg(MyPtr, TEXT("log")); // 콜스택 생성 + 로그 출력
ensureMsgf(Success, TEXT("log")); // if 문의 조건문 안에 넣어 사용 가능
```


단 빌드를 하게 되면 해당 코드는 사라지게 됩니다.
