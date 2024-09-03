# Hard References와 Soft References
## Hard References
**Hard References**는 컴파일 타임에 설정된 Resource 또는 Instance에 대한 직접적인 포인터이다.

우리의 캐릭터가 로드되면서, 함께 로딩하는 TObjectPtr로 선언한 언리얼 오브젝트들도 메모리에 함께 로딩된다.

<br/>

## Soft References 
**Soft References**는 특정 상황일때만 보여질 오브젝트들은 메모리에 올리지 않고, 필요한 데이터만 로딩하도록 한다.

TSoftObjectPtr로 선언하고, 생성자에 에셋 주소 문자열을 지정해주면 된다.
```
// header
UPROPERTY()
TSoftObjectPtr<USkeletalMesh> ItemMesh;

//cpp
UMyItemData::UMyItemData()
{
    ItemMesh = FSoftObjectPath(TEXT("ABC"));
}
```
그리고 나머지가 필요하면 LoadSynchronous로 로드한다. (로드가 되는 첫 시점에서는 순간적인 Lag이 발생할 수 있다)
```cpp
void UMyItemData::ShowItem()
{
    if(ItemMesh.IsPending()) // 해당 에셋이 로드 되어있는지 확인
    {
        ItemMesh.LoadSynchronous();
        ItemMesh.Get();
    }
}
```


> 나머지는 TSoftObjectPtr의 예시 참고

<br/>

> 실제로 Project2에서 [MyMeleeItemData](https://github.com/cubee021/PlayAround_d/blob/6a66bd3d4e237ea7ade11b80969ebcb1617ad1be/Project2/Item/MyMeleeItemData.h#L28)의 SkeletalMesh를 TSoftObjectPtr로 선언한 것을 알 수 있다.
>
>> 그리고 로드는 [여기](https://github.com/cubee021/PlayAround_d/blob/6a66bd3d4e237ea7ade11b80969ebcb1617ad1be/Project2/Item/MyItemBox.cpp#L116)랑
[ 여기](https://github.com/cubee021/PlayAround_d/blob/main/Project2/Character/MyCharacterBase.cpp#L349)
