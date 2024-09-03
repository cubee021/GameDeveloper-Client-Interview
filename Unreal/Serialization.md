# 직렬화(Serialization)
+ **직렬화(Serialization)** : 복잡한 객체나 데이터 구조를 연속된 바이트 스트림으로 변환하는 과정입니다. 이 과정은 데이터를 파일에 저장하거나, 메모리 간에 전송하거나, 네트워크를 통해 데이터를 전송할 때 필요합니다.

+ **역직렬화(Deserialization)** : 직렬화된 바이트 스트림을 다시 원래의 복잡한 객체나 데이터 구조로 복원하는 과정입니다. 네트워크나 파일에서 데이터를 읽어와 다시 원래 형태의 데이터로 복원할 때 필요합니다.

## 직렬화 장점
1. 저장 및 로드 - 현재 프로그램의 상태를 저장하고, 필요한 때 복원할 수 있다.
2. 네트워크 통신 - 멀티플레이어 게임에서는 여러 플레이어 간의 상태를 동기화하기 위해 데이터를 전송해야 한다. 네트워크에서는 바이트 스트림 형식으로만 데이터를 주고받을 수 있기 때문에, 직렬화로 변환해 전송할 수 있도록 한다.
3. 현재 객체의 정보를 클립보드에 복사해서 다른 프로그램에 전송할 수 있다.
4. 데이터 압축, 암호화를 통해 데이터를 효율적이고 안전하게 보관할 수 있다.

> Json 직렬화
> + 웹 환경에서 서버와 클라이언트 사이에 데이터를 주고받을 때 사용하는 텍스트 기반 데이터 포맷
> + 언리얼 엔진의 Json, JsonUtilities 라이브러리 활용

<br/>

## NetSerialize
### FGameplayEffectContext::NetSerialize의 역할
주로 멀티플레이어 게임에서 FGameplayEffectContext 객체를 네트워크를 통해 전송하기 위해 사용됩니다.

### NetSerialize의 목적
+ 동기화 유지 - 네트워크 게임에서 모든 클라이언트와 서버가 동일한 정보를 가지도록 보장.
+ 성능 최적화 - 직렬화된 데이터는 최소한의 크기로 압축되어 전송되므로 네트워크 대역폭을 절약하고, 빠른 전송을 가능하게 한다.

[자세히](https://kimteemo8729.tistory.com/41)

### 직렬화 되는 데이터의 종류
```cpp
// Instigator : 효과를 유발한 주체. ex) 공격자 또는 능력을 시전한 캐릭터
// Effect Causer : 실제로 효과를 발생시킨 개체. ex) Instigator가 소환하거나 생성한 개체일 수 있음
// Ability Class Default Object (AbilityCDO) : 이 효과가 특정 능력에 의해 발생한 경우, 그 능력의 클래스 기본 객체
// Source Object : 효과의 출처로 간주되는 객체
// Affected Actors (Actors) : 이 효과의 대상이 된 모든 액터의 리스트
// Hit Result : 효과가 발생한 위치와 관련된 충돌 정보
// World Origin : 효과가 발생한 월드 좌표
```

### Project2 제작 당시 사용 예시
https://github.com/cubee021/PlayAround_d/blob/768e76650edc6ab37f351cb0a621c35721ed901d/Project2/GameData/MyCharacterStat.h#L48-L53

