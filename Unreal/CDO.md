# CDO(Class Default Onject)
실행 초기의 런타임 과정에서는 언리얼 오브젝트마다 클래스 정보와 함께 언리얼 오브젝트의 인스턴스가 생성된다. 이 특별한 인스턴스는 언리얼 오브젝트의 기본 세팅을 지정하는 데 사용되며, CDO라고 한다.

오브젝트를 생성할 때(SpawnActor, NewObject 등등) 매번 초기화 시키지 않고, CDO를 미리 만들어 복제하는 빙식이 사용되고 있다.

> 참고로 에디터 상에서도 CDO의 존재를 엿볼 수 있는데 :
> 
> 1. 에디터 로딩 시 UCLASS로 선언된 클래스들의 CDO가 로딩된다. ```초기화..15%```가 이것!
> 2. 에디터 내에서 C++나 블루프린트에 미리보기같이 클래스가 시각화 되어있다.

<br/>

# ConstructorHelpers
객체를 생성할 때 쉽게 리소스를 찾고 로드할 수 있게 도와준다. 

게임 개발 과정에서 코드의 가독성과 우지보수성을 높이는 데 도움이 된다.

## FObjectFinder
특정 Mesh나 Texture를 찾는 경우에 사용된다.

## FClassFinder
특정 유형의 클래스를 찾는 데 사용된다.
```cpp
ex)
ConstructorHelpers::FClassFinder<APawn>(TEXT("c++ class or blueprint path"));
```
