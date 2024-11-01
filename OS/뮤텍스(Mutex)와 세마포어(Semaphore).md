# 뮤텍스(Mutex)와 세마포어(Semaphore)

> Critical Section(임계 구역) : 공유된 자원 부분

둘 다 이 임계 구역과 관련해 여러 스레드, 프로세스들이 한번에 접근하는것을 막는 방식입니다. 가장 큰 차이라고 하면 **뮤텍스 객체는 한개의 스레드/프로세스만이 가질 수 있고 세마포어의 경우 카운터 변수를 공유하면서 관리**한다는 것입니다.

- Mutex
  - 뮤텍스 객체에 한 스레드가 접근
  - 뮤텍스 객체는 Lock
  - Lock된 객체는 다른 스레드가 접근 불가능
  - 스레드가 사용을 다 하면 뮤텍스 객체를 Unlock해서 타 스레드가 사용할 수 있게 함
  - 프로세스의 범위를 가지며 프로세스가 종료될 때 자동으로 Clean up
  - 자원 소유가능 + 책임을 가짐
  - 소유하고 있는 스레드만이 이 Mutex를 해제할 수 있음
- Semaphore
  - 카운터 변수를 두고 있음
  - 스레드가 접근하면 이 카운터 변수를 -1
  - 만일 카운터 변수가 0이 되면 다른 스레드는 접근 불가능 (Lock)
  - 스레드가 사용을 다 하면 카운터 변수를 다시 1 올려줌
  - 대기 큐에 PCB가 있는 경우, 준비 큐로 옮기고 임계구역으로 옮겨줌
  - 시스템 범위에 걸쳐 있고, 파일 시스템 상의 파일로 존재
  - 자원 소유가 불가
  - Semaphore를 소유하지 않는 스레드가 Semaphore를 해제할 수 있음

> 여기서 뮤텍스는 사실상 Binary Semaphore라고 볼 수 있습니다.

## 모니터
뮤텍스와 세마포어의 단점인 잘못된 사용을 보완한 방식입니다.
- wait와 signal을 자동으로 처리
- 공유자원을 모니터 내부에 숨기고, 인터페이스를 통해서만 접근하도록 함
- 모니터 안에는 한번에 하나의 프로세스만 들어옴
- **조건변수**와 조건변수에 대한 큐를 사용하여 실행순서를 제어할 수 있음

> 뮤텍스와 모니터 차이점
> + 뮤텍스는 다른 프로세스(애플리케이션)간에 동기화할 때 사용할 수 있다. 모니터는 하나의 프로세스(애플리케이션)내에 다른 스레드 간에 동기화할 때 사용된다.
>
> + 뮤텍스는 보통 운영체제 커널,프레임워크,라이브러리에 의해서 제공되는 반면에 모니터는 프레임워크나 라이브러리 그 자체에서 제공된다. 따라서 뮤텍스는 무겁고(heavy-weight) 느리며(slower) 모니터는 가볍고(light-weight) 빠르다(faster).

> 세마포어와 모니터 차이점
> +  세마포어는 실제로는 카운터라는 변수값을 매번 따로 지정해줘야하는 등 조금 번거롭다. 모니터는 캡슐화되어있어서(encapsulation) 카운터값을 1로 주냐 0으로 주냐 고민할 필요 없이 synchronized, wait(), notify() 등의 키워드를 이용해 좀 더 편하게 동기화할 수 있다.
