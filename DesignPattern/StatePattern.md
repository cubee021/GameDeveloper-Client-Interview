# 스테이트 패턴 (State Pattern)

> 이 패턴을 알기 위해서는 FSM에 대한 지식이 있어야 합니다.

한번에 한 상태만 가질 수 있는 FSM을 제작할 떄 많이 사용하게 되는 패턴입니다. 중첩 switch/case 문으로 구현할 수 있지만 상태가 많아질수록 이는 확장성이 떨어지기 떄문에 스테이트 패턴으로 구현할 수 있습니다.

```cpp
using UnityEngine;

public class Minster : MonoBehaviour
{
    private enum State
    {
        Idle,
        Move,
        Attack
    }
    private State _state;

    private void Start()
    {
        _state = State.Idle;
    }

    private void Update()
    {
            switch(_state)
            {
                case State.Idle:
                    // Idle 행동 구현
                    break;
                case State.Moce:
                    // Move 행동 구현
                    break;
                case State.Attack:
                    // Attack 행동 구현
                    break;
            }
      }
}
```

![좀비의 FSM을 구현한다면](https://user-images.githubusercontent.com/68003176/208879792-cdc3fbf2-069b-48c7-a975-ed53367093dd.png)


상위 인터페이스인 State 에서는 관련 기능들에 대한 함수들을 준비하고 상속받는 파생 State들에서는 해당 State에서 할 수 있는 함수를 구현하는 것이죠.

## 전략 패턴과의 차이

스테이트 패턴은 전략 패턴과 비슷한 모습을 하게 됩니다. 일단 유래가 스테이트 패턴은 조건문을 대체하는 것이고 전략 패턴은 상속을 대체할 수 있도록 나온 것입니다.

또한 스테이트 패턴에서는 State들의 파생 클래스들이 일을 하는 Context 클래스에 대한 참조를 가지고 있다는 차이를 가지고 있습니다. 대신 전략 패턴은 이런 경우가 없죠.

때문에 **스테이트 패턴은 전부 전략 패턴이 될 수 있는데 전략 패턴은 전부 스테이트가 될 수는 없습니다.**

<br/>

[참고!](https://dodobug.tistory.com/16)
