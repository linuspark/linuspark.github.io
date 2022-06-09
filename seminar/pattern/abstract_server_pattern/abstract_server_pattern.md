## Abstract Server Pattern

Note:
추상 서버 패턴은 굉장히 간단한 구조적인 패턴입니다.

---
### Example : Light
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLWWkpon9pe1oV3BJCqggkHGKj1LAIelo_FqGpBGqhbekBeXg1LqxY58kXzIy5A1P0000)

Note:
이 UML을 개선시킬 것 입니다.
지금의 상황에서 Switch로 제어해야 하는 것이 Light뿐만 아니라 FAN이 생겼습니다.
___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLWWkpon9pe1oV3BJCqgWRBV4F8WSWi_51H5HqBM3AWKA8eI076I1qWBT6ZkO-GwfUIb0Im40)

Note:
엄.. 기존의 sw 를 바꾸지 않으려고 하다보니 이런 구조가 발생하였습니다. 
구조가 조금 이상하죠? 나라면 이런식으로 코딩하지 않겠어 라고 하지만 생각보다 쉽게 발생할 수 있는 구조입니다.
*이 구조는 어떤 문제가 있을까요?*
- FanSwitch는 light 를 가지고 있다
기존의 코드를 변경하고 싶지 않다는 마음과 어떻게든 목적은 달성해야겠다는 생각이 있으니까요
이 것을 추상 서버 패턴으로 풀어보겠습니다.
___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLWWkpon9pk3Ap2j9BKfBJ4w52YGcvQGgL7CfA6Whb9GMvVdx8PXfQLorKCq-cUaP9L2sMs8U5nT4iuAk7P8nN61L2hgb1RerAE907LX47LBpKe3E0m00)

Note:
비교적 깔끔한 해결 방법이죠.
이제 게속해서 Switch로 제어할 객체가 생겨도 문제가 없습니다.
---
### Interface Ownership
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuIf8JCvEJ4zL283IrLp9EOd5nGh1vPaboHbScNabgKLfYScfK874f1oG6WK5ROMIqg8yVpy4CosDgnO88TfzCjCpIg1ijyGyBYw8TWLTEoI3kC2g57HB2tHhKCI1Eh28EgJcfG2T3000)

Note:
인터페이스의 소유에 대해서 이야기를 하려고 합니다. (예전에 의존 관계 역전 원칙을 할때에도 한번 이야기 한 적이 있습니다.)
이 UML을 보면 인터페이스의 이름이 ISwitchable 입니다.
ILight가 아닌 거죠. Light를 제어하기 위한 인터페이스니까 ILight라고 생각할 수도 있지만 인터페이스를 생각할 때에는 인터페이스를 사용하는 입장, 그러니까 클라이언트를 먼저 생각해야 합니다.
인터페이스와 그 인터페이스를 구현한 파생형 클래스 보다, 인터페이스와 클라이언트 사이의 "논리적 구속력"이 더 크다는 사실을 기억해야 합니다. 
기본적으로 인터페이스와 그 상속된 클래스를 같은 패키지에 넣기 쉽지만, 그것이 아니라 인터페이스는 해당 인터페이스를 사용하는 클래스와 같은 패키지에 구성되어야 합니다.

---