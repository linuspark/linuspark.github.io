## Adaptor Pattern

Note:
호환성을 유지시키기 위한 패턴입니다.

---
### Reason of adaptor
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLWWkpon9pk3Ap2j9BKfBJ4w52YGcvQHMSoaeQ2kKb1Rb-USXc6bfNBLGlJwPwHabN5mG7GgwTaXwkS1o2hgb1RerAE8EgNafGDi1)

Note:
switch가 light를 이용해야 합니다. 이런식으로 구현을 할 수 있겠죠.
그런데 만약 Light가 외부 라이브러리에서 가져온 것 이라서 이런식으로 ISwitchable 인터페이스를 상속받을 수 없다고 합시다.
하지만 내 Code는 이미 ISwitchable을 사용하도록 작성되어 있어요. 이럴 경우 어댑터 패턴을 사용할 수 있습니다.

___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLWWkpon9pk3Ap2j9BKfBJ4w52YGcvQHMSoaeQ2kKb1Rb-USXc6bfNBLGlJwPwHabZYc91K2zo49SN11357Jja8pZGbQke5jQe5k3HzeEOfI2bOAIZKrAQavgUc99Qh6TdHANGsfU2j3z0000)

Note:
이런 식으로 ISwitchable 을 상속받는 어뎁터 클래스를 만들고 그 LightAdaptor가 외부 라이브러리인 Light를 제어하도록 하면 됩니다.
말 그대로 어댑터의 역할을 하는거죠

___
```python [|13|2-3|5-10|9-10]
class Light(object):
    def specific_request(self):
        return 'Hello Adapter Pattern!'

class Adapter(object):
    def __init__(self, adaptee):
        self.adaptee = adaptee

    def turn_on(self):
        return self.adaptee.specific_request()

client = Adapter(Light())
print client.turn_on()

```

Note: 
코드로 보면 이런식이 됩니다.
클라이언트가 사용하고 싶은 모양새는 이런 식입니다. turn_on을 호출하면 원하는 동작이 나오기를 바라요
하지만 내가 가져와서 사용해야 하는 라이브러리는 이런 식으로 사용해야합니다.
그래서 어댑터를 만들어서 turn_on함수가 호출되면 specific_request가 호출되도록 한것 입니다. 
이 부분(turn_on)이 어뎁터의 핵심인거죠, 만약 Light 를 사용하는 방법이 specific_request가 아니라 다른 여러개의 함수를 조합해서 사용하는 것 이었다 해도 어댑터가 흡수 할 수 있습니다. 예를들면 light가 grpc로 동작해서, 전류 몇 A 로 설정하고 메시지를 보내고, 전원을 켜는 메시지를 만들어 메시지를 보내고 하는 방식으로 동작하더라도 Client는 turn_on하는 것 만으로 동작시킬 수 있겠죠

---
### Adaptor using class 

![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLWWkpon9pk3Ap2j9BKfBJ4w52YGcvQHMSoaeQ2kKb1Rb-USXc6bfNBLGlJwPwHabZYc91K2zo49SN11357Jja8pZGbQke5jQe5k3Mnee1pNB8JKl1UXS0000)

Note: 
어뎁터를 사용하는 방식에는 방금처럼 위임을 사용하는 것이 아니라 상속을 이용할 수도 있습니다.
*그럼 Adaptor를 위임을 사용하는 것이 아니라 상속을 이용하는 것에는 어떤 장점이 있을까요?*
장점은 구현이 쉽다는 것 입니다.
위임으로 할 때 처럼 위임할 클래스를 연결하거나 하는 작업이 필요가 없어요, 그냥 상속받은 함수를 사용하기만 하면 됩니다.
단점은 강한 결합이 생긴다는 것 입니다.

---
### Example : Modem

![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbNmpKz9pQtcqbPmoKpC0L8UYNdffGL0Hd1gNWgG2afDJiqiI-MgvU9ApaaiBbO8zWPhWVAyGv1vFkvGNcPHSWxKREUSpDIy4Yuk98w2hguTH98BQfg2Rcc1RWrCq3i4Yw2FEnP11w0ZWVu10000)

Note:
모뎀의 예시를 가져왔습니다.
모뎀 인터페이스를 구현한 SK모뎀, KT모뎀 등이 여럿 있어요, 그리고 Client는 인터페이스를 통해서 모뎀을 사용합니다. 지금 클라이언트를 하나만 표시했지만 이런 어플리케이션이 많이 생성되고 사용되고 있습니다.
이런 와중에 전용 모뎀이라는 것이 생겼습니다. 전용 모뎀은 회선 양쪽에서 전용으로 동작하기 때문에 Dial을 걸거나 Hangup을 할 필요가 없어요. 
그럼 이제 이 전용 모뎀을 사용하는 Application을 개발해야 하는 상황입니다.

___
![_](https://www.plantuml.com/plantuml/png/TO_F2W8X4CRlynJUmrU8XHOAjYvjFO3e5CAiBVxfKhrxRYgPq6bcVlpy-EoS5zQ7YK0RZ_OY9BB3JU7qW1KRqQWuZYXHtn5UbTDhGOHsiPOrZfqmrp172IG9vzWuV7BoDPpgQx-jhnwLbiAZIX2ajf9oZGrfDBwWQ9uTMe2yCCxNoOFA_azBRSZb60ypVnOvTMnWgjh0vdb9BG4DBX4D_lqs-wPtQ5IOw0q0)

Note:
이 문제를 해결하기 위한 이상적인 방법은 인터페이스를 분리하는 것 입니다.
한 인터페이스에 두 가지 역할이 있으니 Modem인터페이스를 나누어서 Dialler역할을 분리시키는 것이죠
그러면 DedicatedModem은 Modem 인터페이스만 상속받아서 사용하고, 다른 모뎀은 Modem과 Dialler를 상속받아서 사용하면 됩니다.
DedUsers들은 Send/Receive 함수만 호출해서 사용하면 되고, 기존의 Client들은 연결할 때에는 Dialler인터페이스의 Dial과 Hangup을, 데이터를 전송할 때에는 Send/Receive를 호출하면 됩니다.
하지만 이렇게 될 경우 기존의 ModemClient로 표현된 모든 Application이 변경이 있어야 하는 문제가 있습니다.
호환성이 깨지게 되는 거죠
호환성을 깨지 않는 간단한 방식을 생각해 봅니다.

___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbNmpKz9pQtcqbPmoKpC0L8UYNdffGL0Hd1gNWgG2afDJiqiI-MgvU9ApaaiBbO8zWPhWVAyGv1vFkx8F9VKbCpan9BK5EBjoCilILL8oYyfoSzLo4z3Cy5A8GMe_zNcFGePO0FUp6rGUDCzv_MwDQyNTBoPk-LDwmokvCoYv1oehvnpCbFpIg128BD2YrEBkBYW30LTNJiq2xYGj86b8Q9G7olebXReri04lKEm2FguOn54jKC18U40z3qmCW00)

Note:
DedicatedModem이 기존의 modem 인터페이스를 상속받아서 사용하되, Dial과 hangup은 알아서 적절한 방식으로 처리하는 것 입니다.
이렇게 하면 기존의 user들도 DedicatedModem을 사용할 수 있고, 새로운 DedUsers도 DedicatedModem을 사용할 수 있습니다.
하지만 조금 깔끔하지는 않죠. DedUsers들은 사용하지도 않는 Dial과 Hangup을 알고 있어야 합니다.
만약 Dial/Hangup의 함수 형태가 변하거나 다른 변경사항이 생기면 DedUsers들은 영향을 받을 수 밖에 없습니다.

___
![_](https://www.plantuml.com/plantuml/png/RL71IWCn4BtFLynTumU8b8g52gqNMpyW9DCQIBDBalLI1GyLHAyUFBJY1uWNFVf5RZx2ORl5LNAQdRVpvhqtwOSeVQmM5eoBv6TI4PuLcXPBsCE1aPRBgNJpgkIF2JdDvPmKcIk26m1bPGWu6JMKjXjDkzrusEq6f8sIaNG37cjPiYYu8XB6eiHYbfyuHCrMlZ--yBBnaZQtc1xNzVjR_NgVDf_fxla00xlRHxyrLpyyOLglqiiggxpCfp5UsJR_YJNauWvYzaKW3v2rX-AwjsL1EuX2zFv9GcSj_zuHsjkXgChVBQDf1XmFC-1V3JmIc7V85oHBSyQXpxdvmNy0)

Note: 
Adaptor를 이용해서 해결한 모습입니다.
DedicatedAdaptor가 중간에서 호출을 위임하도록 하고 사용하지 않는 함수들은 알아서 시뮬레이션 해줍니다.
Modem의 인터페이스가 변경되더라고 DedUser에게는 영향이 없겠죠, DedicatedAdaptor가 해당 변화를 흡수해주기 때문입니다.

---
## Applicability
- When compatibility of incompatible interface.

---
### Pros and cons
#### Pros
- Compatibility of incompatible interface(OCP)
- It can isolate interface problem(SRP)

#### Cons
- Complexity

Note:
장단점은 명확합니다.
호환되지 않는 인터페이스를 호환시킬 수 있죠. 이 말은 OCP 를 만족시킬 수 있다는 것 입니다.
새로 추가되어야 하는 기능이 기존의 것을 변경하지 않고도 추가할 수 있게 한다는 점이죠
두 번째 장점은 인터페이스 문제를 분리할 수 있다는 점 입니다.
실제 비지니스 로직과 동작시키는 인터페이스의 문제를 어댑터가 가져가기 때문에 어댑티는 자신의 역할/책임에 집중할 수 있습니다.
단점으로는 역시 코드가 복잡해진다는 점이죠.

---
## End