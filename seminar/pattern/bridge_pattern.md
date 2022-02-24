## Bridge Pattern

Note:
브리지 패턴입니다.

---

### Separate hierarchies
### Abstraction and Implementation

Note:
두 가지 계층구조를 나누는 패턴입니다.<br>
여기서 계층 구조란 추상화 계층과 구현부를 이야기합니다.

---

## Structure
![_](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Bridge_UML_class_diagram.svg/1500px-Bridge_UML_class_diagram.svg.png)
```csharp
var absraction = new RefinedAbstraction(new ConcreteImplementor());
abstraction.function();
```

Note:
조금 복잡하지만 UML먼저 보고 예시를 보겠습니다.<br>
UML을 보면 계층구조, 즉 상속을 사용한 부분이 두 부분이 있습니다. Absraction 부분(추상화 계층)과 Implementor 부분(구현부 계층)입니다.<br>
Absraction은 implementor를 가지고 있고 내부적으로 Implementor를 사용을 합니다.<br>
이런식으로 추상 부분과 구현 부분을 나누어 놓았습니다.<br>
계층구조를 추상 부분과 구현 부분으로 나누는 것이 브리지 패턴의 핵심입니다.<br>
사실 UML만 보고는 이해가 잘 안갑니다.<br>
우리가 일반적으로 상속을 사용할 때에는 추상화 된 인터페이스를 상속하여 구체적인 구현을 가진 클래스를 사용하는데요, <br>
왜 그런 방법이 아니라 이렇게 복잡한 방법을 사용하는지 한번 살펴보겠습니다.

---
## Example
### Shape and Color
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvKhEIImkLd3EB4hEIOLoWWjB4ujIkRZ0QXLiQdHrOV884PWYXzIy5A0D0000)

Note:
원과 정사각형을 그려서 사용하는 Application을 만들고 있습니다.<br>
그릴때 시용하는 함수가 동일하기 때문에 Shape라는 인터페이스를 만들고 각각 Circle과 Square를 상속받아서 계층구조를 만들었습니다.<br>
이런 상황에서 추가 요구사항이 와서 원과 정사각형을 빨간색과 파란색으로 표현할 수 있어야 하는 상황이 되었습니다.<br>
가장 간단한 방법은 클래스를 더 만드는 것 입니다.


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvU82YoZOrEZgAWIbfZXd5YNdf28BEkMKfa94qPG65vOc5c4eXOewfEQb06q60000)

Note: 이런 식으로 각각 Red / Blue 클래스를 만들면 구현하는 입장에서 편하겠죠. 생각없이 구현할 수 있으니까요


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvN9CAYufIamkKN3EB4hEIKNmWmjB4ujIkRZ0EXHiQdHrOKh055GeA3K5YwXJJcagX8-a7MOYc49eHn55Q8SAEwJcfG1z0000)

Note:
조금 더 문제를 구조화 시키기 위해서 이런식으로 계층구조를 더 만들 수도 있습니다.<br>
지금 여기에서는 각 circle과 square를 상속받은 Red / blue 클래스를 만든 것 입니다.<br>
여기서 잠깐 생각해 볼게요, 만약 여기서 노란색을 만들어야 한다. 혹은 삼각형을 만들어야 한다.<br>
어떤 클래스들이 생성되어야 할까요?


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvN9CAYufIamkKGXAJG5B8aISSafJ8K9SO4h1faPN5w4Eoa08EsSM9UTW4GykB4qiIaKs0s4od8MG01k3LGPga4DgNWhGLm00)

Note:
똑같은 방법입니다.<br>
여기서는 색깔을 기준으로 계층을 나누고 클래스를 생성하였습니다.<br>
여기서도 똑같이 생각해보죠, 노란색이 추가될 경우와, 삼각형이 추가되어야 하는 경우<br>
이 두 방식은 지금 두 가지 차원/혹은 자유도 라고 부르는 것을 하나의 상속구조로 표현하려고 만든 구조입니다.<br>
여기에는 "모양" 이라는 자유도와 "색" 이라는 자유도를 가지고 있습니다.<br>
이 두 자유도는 서로 독립적입니다.<br>
이 독립적인 차원을 서로 분리하는 것이 브리지 패턴의 효용입니다.<br>


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuN9CAYufIamkKGZEI2n8hUPILd3Epyaluj9sAKeirz3agkNYoimhIIrAIqnEXKfnWPKgvEiMPQPdbEX2HA62DZMwG87CekISL6IHuCBInA9KBh1AY4XGQWeorocdD9NB8JKl1UWc0000)

Note:
브리지 패턴을 적용하면 이런 모양이 되는데요<br>
"모양"이라는 자유도와 "색" 이라는 자유도를 각각 독립적인 계층구조로 분리한 것입니다.<br>
여기서는 '모양'을 추상화라고 보고, 색을 칠하는 것을 구현체라고 보았습니다.<br>
모양은 색상에 대한 구체적인 업무를 '색' 계층에게 위임하였습니다.<br>
이 Color에 대한 참조가 Shape와 Color사이의 bridge가 됩니다. <br>

---
## Example
### Modem
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbNmpKz9pQtcqbPmoKpC0L8UYNdffGL0Hd1gNWgG2afDJiqiI-MgvU9ApaaiBbO8zWPhWVAyGv1vFkvGNcPHSWxKREUSpDIy4Yuk98w2hguTH98BQfg2Rcc1RWrCq3i4Yw2FEnP11w0ZWVu10000)

Note:
두 번째 예시는 지난번 어댑터 패턴에서 보았던 예시입니다.<br>
기존에 Modem 유저들은 Modem인터페이스를 이용하여 많은 Application들을 구현해 놓은 상황입니다. <br>
인터페이스만을 이용하기 때문에 모뎀의 종류가 늘어나는 것은 아무 영향을 받지 않습니다.<br>
그런데 시대가 바뀌어 새로운 타입의 모뎀이 생겨났죠<br>
dial과 hangup이 필요 없는 전용모뎀 dedicated modem이 생겼습니다.


![_](https://www.plantuml.com/plantuml/png/TO_F2W8X4CRlynJUmrU8XHOAjYvjFO3e5CAiBVxfKhrxRYgPq6bcVlpy-EoS5zQ7YK0RZ_OY9BB3JU7qW1KRqQWuZYXHtn5UbTDhGOHsiPOrZfqmrp172IG9vzWuV7BoDPpgQx-jhnwLbiAZIX2ajf9oZGrfDBwWQ9uTMe2yCCxNoOFA_azBRSZb60ypVnOvTMnWgjh0vdb9BG4DBX4D_lqs-wPtQ5IOw0q0)

Note:
이상적으로 생각하면 인터페이스를 분리하여 dial 역할을 수행하는 인터페이스와 데이터송수신의 역할을 하는 인터페이스로 분리하는 것이 깔끔합니다.<br>
하지만 이렇게 될 경우 기존의 ModemClient를 싹 다 띁어고쳐야 하죠<br>
기존에 어댑터 패턴을 이용해서는 이렇게 문제를 해결했습니다.


#### Adaptor Solution
![_](https://www.plantuml.com/plantuml/png/RL71IWCn4BtFLynTumU8b8g52gqNMpyW9DCQIBDBalLI1GyLHAyUFBJY1uWNFVf5RZx2ORl5LNAQdRVpvhqtwOSeVQmM5eoBv6TI4PuLcXPBsCE1aPRBgNJpgkIF2JdDvPmKcIk26m1bPGWu6JMKjXjDkzrusEq6f8sIaNG37cjPiYYu8XB6eiHYbfyuHCrMlZ--yBBnaZQtc1xNzVjR_NgVDf_fxla00xlRHxyrLpyyOLglqiiggxpCfp5UsJR_YJNauWvYzaKW3v2rX-AwjsL1EuX2zFv9GcSj_zuHsjkXgChVBQDf1XmFC-1V3JmIc7V85oHBSyQXpxdvmNy0)

Note:
중간에 인터페이스를 호환시켜 줄 어댑터를 만들어서 구현하는 방법입니다.


#### Dimension
![_](https://www.plantuml.com/plantuml/png/VOyz2W8n48NxEKKki5UGBGIBrKelC6IU44XMsIITXxSm-6HOBFD-NhwPQzEjzP8bhGtRNIF2vM4e8Z5hhU6OD7-4yOQbg0qsKby_JFqvlGwZpPZpk7nT_FPoyyhvH4L-2cEFdmkxEoPdTapYm1mDpC70oE9kSnSBwpv0gF-16Qlrajy0)

Note: 
이 문제를 브리지 패턴으로 적용하기 전에 분석을 해보면 이렇게 표현할 수 있습니다.<br>
계층구조에 두 가지 자유도가 포함되어 있고 각각 '연결방법'에 대한 자유도와 '제조사'에 따른 자유도를 확인할 수 있습니다.<br>
자유도가 중첩되어 있는 것을 보았으니 이것을 분리해서 브리지 패턴을 만들어봅니다.


#### Bridge Solution
![_](https://www.plantuml.com/plantuml/png/fPBFoXCn5CNtzoakk9Jn0Rhu-C22IdLZzG7Ip9qIo2Hb9kEcAXMgYBeGMh12ArsgY0Ywz8awUGYJwLM6Clq3Gc7kJS_zvPmarwKJXQjo3SeuAZ8X2H_ObF8ftCI-4ZfyxWephYQX6999m-SXIL9F29wrPlgKAYaSfJpS8PQga9hfjxKYutWf3ZykgG3W0fFawe08hR7cx_qgY57f2Y4TOwqn99so9bIki5fJCOKRJP1x-KI7CeRXCZhaabt6xfBSnZf2JPb3cntV6NiOWNupssjYGta88ABEVtYFVZttd-O0nn59DKcUSjgpiiCpExpJQ43zCt3H3P_QywgB6aAdf6ai7058BSeIXuD6nztWKRkxVsVViOQ3T8A19qzgc7T20xnpdq-fzL8kllgHTSxcQBCE2lOQoExdRwR4-_Tlr_NtR_NsjT_yyYzNjngk_pZx2wxVB77tuqNu-SqAdqV7s7uWR3bm_zzp5wQ7zTVFzMABzPUbV_MkNgpFEUe8pcT-_CL0nyxdXk0wfAbo_GS0)

Note:
ModemConnectionController는 연결방식에 대한 계층구조를 만들어냅니다.<br>
그리고 이 클래스는 각각 Modem인터페이스와 DedicatedModem인터페이스를 상속받고 있기 때문에 두 가지 타입의 유저, Modem Client와 DedUser를 만족시킬 수 있죠<br>
그리고 '제조사' 에 대한 구체적인 구현사항을 ModemImplimentation이라는 새로운 계층구조를 만들어서 위임시킵니다.<br>
각 제조사는 ModemImplimentation을 구현하여 요구사항을 맞출 수 있습니다.<br>
이렇게 하면 User들에게는 영향이 없이 내부 구조를 깔끔하게 정리할 수 있겠죠<br>

---
## Applicability
- Separate the object that has Independent dimension.
- Subfunctions that various variations 
- Runtime change the implementation

Note:
이 두 예시들을 보면 알겠지만 브리지 패턴을 사용할 때에는 보통 독립적인 자유도,차원이 함께 구현된 객체를 만났을 때 입니다.<br>
하나의 객체가 너무 많은 변경될 이유를 가지고 있을 때 서로 가진 속성을 기준으로 나누어 브리지패턴을 사용할 수 있겠죠<br>
두 번째는 일부 기능에 대해서 다양한 변형이 있는 경우라고 이야기하는데요 <br>
여기에는 전형적인 예시케이스가 있습니다. 바로 플랫폼 호환성의 어플리케이션 입니다. <br>
특정 어플리케이션이 플렛폼에 따라 다르게 동작해야 하는 경우 이런 브릿지 패턴을 이용합니다.<br>
예를 들어 웹 페이지의 경우 크롬 브라우저에서 표현할지, 익스플로러 에서 표현될지에 따라 다르게 동작외더야 하는 경우가 있는데 이런 경우를 말합니다.<br>
마지막으로 런타임에 구현사항을 변경할 필요가 있는 경우 인데요<br>
이 경우는 스트레터지 패턴을 생각하면 이해가 쉽게 될 겁니다.<br>

---
## Pros and cons
### Pros
- Separate the Abstraction and implementation
- OCP
- SRP

Note: 
장단점까지 빠르게 이야기를 하겠습니다.<br>
장점으로는 추상화와 구현체를 분리할 수 있다는 것입니다.<br>
이것은 곧 OCP와 SRP를 만족시킬 수 있다는 이야기가 됩니다<br>


### Cons
- Complecated

Note:
단점으로는 너무 복잡해 집니다.<br>
브리지 패턴의 경우 패턴을 알고 있음에도 불구하고 다시 한번 보게 만드는 복잡합이 있습니다.<br>
그래서 보통 처음부터 브리지 패턴을 이용하자. 와 같은 발상은 위험합니다. <br>
객체가 충분히 복잡할 경우 브리지 패턴을 사용했을 때와 사용하지 않고 다른 방싱르로 사용했을 떄를 충분히 비교하고 사용을 결정하는 것을 추천합니다.<br>

---
## End