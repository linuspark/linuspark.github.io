## Decorator Pattern

### Decorate
![_](https://refactoring.guru/images/patterns/content/decorator/decorator.png)

---
### Attach new behaviors

Note:
Visitor패턴을 할 때, 데코레이터 패턴은 비지터 패턴 그룹이라고 소개하였습니다.
데코레이터 페턴은 비지터 패턴과 마찬가지로 기존의 객체, 혹은 객체의 계층구조를 수정하지 않고 새로운 기능을 할 수 있도롬 만들어 주는 패턴입니다.

---

## Problem
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbNmpKz9pQtcqdR9JCpHqEJI3axDIm7H7ebvwQ4585PGQd8PaAxbuWhs1gjMq2qjqAsnWsZbyWx18XWQa5DQZ4NS0MWwq7u0)

> Add Function: setup the dial volume 

Note:
새로운 요구사항을 가정해보겠습니다.
Dial 함수가 동작할 때 volum 이 크게 동작하도록 하고 싶습니다.

### Templete Method Pattern
```csharp
public abstract class Modem{
	private bool wantsLoudDial = false;
	public void dial(){
		if (wantsLoudDial)
			setVolume(10);
		dialForReal();
	}
	public abstract void dialForReal(){};
}
```
텔플릿 메소드 패턴을 이용하여 문제를 해결해 보았습니다.
Modem 인터페이스를 추상클래스로 바꾸고 dial함수를 수행할 때에 voluem을 올리는 부분을 추가하였습니다.

---
## Decorator Pattern
![_](https://www.plantuml.com/plantuml/png/ROynJmCn38Lt_mgFFQ53i0AgEZ0W5IH4WJsz-Af8Jdngx2wb_nst6KfHf_VqFTjvMLGDMxNCa8hITWm3uPj4odkumUSKs6L59RgyXBAnoSq73JkNIlejR9bcljh10M3WK2k-YiNZvkaCm6fvCjZRrF_CiT7bvnNuzSvMht2uk8WzqwZdz3AK7fjvmAT8J9kWD0SeeeHhKHKV6VzLd6pDQS2Tf8ZHwQpR0sBIrkNs_C_xk-xnRzA6uG1K8XwVW8Kxo_mB)

Note:
데코레이터 패턴을 이용하여 문제를 해결해 보았습니다.
새로운 클래스로 LoudDialModem을 만들고 이 LoudDialModem은 기존의 Modem 인터페이스를 가지고 있습니다.
다른 기능들은 들고있는 Modem인터페이스에게 위임을 시키되 Dial 함수가 실행되면 SetVolume을 수행한 뒤에 dial 기능을 위임시킵니다.
말 그래도 dial에 대해서 decorate만 수행하는 거죠.


```csharp
public void testLoudDialModem(){
    Modem m = new KTModem();
    Modem d = new LoudDialModem(m);
    Assert.areEquals(0, d.getVolume());
    d.dial("11223344");
    Assert.areEquals(10, d.getVolume());
}
```

Note:
검증을 위한 테스트 코드는 이렇게 작성할 수 있겠죠?

---
### Multiple Decorator
![_](https://www.plantuml.com/plantuml/png/ROynJmCn38Lt_mgFFQ53i0AgEZ0W5IH4WJsz-Af8Jdngx2wb_nst6KfHf_VqFTjvMLGDMxNCa8hITWm3uPj4odkumUSKs6L59RgyXBAnoSq73JkNIlejR9bcljh10M3WK2k-YiNZvkaCm6fvCjZRrF_CiT7bvnNuzSvMht2uk8WzqwZdz3AK7fjvmAT8J9kWD0SeeeHhKHKV6VzLd6pDQS2Tf8ZHwQpR0sBIrkNs_C_xk-xnRzA6uG1K8XwVW8Kxo_mB)
> Add Function: Logging when send/recv called

Note:
여기에 또 새로운 기능을 추가하고 싶은 요구사항이 생겼습니다.
Send / Recv 함수가 호출되었을 때, 오고가는 데이터를 Logging 하는 함수를 추가하고 싶습니다.
기존에 LoudDialModem을 만들었던 것 처럼, Logging 하는 Modem을 만들어야겠습니다.


![_](https://www.plantuml.com/plantuml/png/VL1DImCn4BtFhvXZ5rdHgqhfmODG1R7gFTtCXi0aMPgPNgh_tMbpQ9N5qtkyZnSogofk9veOOXRQZMuWV2cUqW6ky34wDjXGzWPFBWUTZBpHi3Ue99-5DT72gXry0mpiQiNdelxOFCq0RDOdWhrE_TSIcxf-dn4_NbdhZ0wNY-OnZN9sVvkbnqRkyC4JKt12Ix1G2367-O7c_TlFHGYtHQHOPFppnKct70VSb-ZHcxhe3e0OfVtb-dodsvlk_j9fORiSPO_79s1bJ1F_0000)

Note:
똑같이 Decorate를 만들었습니다. Send 와 Recv 하기 전에 Logging하는 역할을 수행하는 Logging Modem입니다.
실제 사용할 때에는 LoudDialModem과 Logging Modem을 모두 decorate 해서 사용하겠지요


```csharp
public void Main(){
    Modem m = new KTModem();
	Modem d = new LoudDialModem(m);
	Modem l = new LoggingModem(d);

	l.dial("11223344");
	l.send("SomeMessage");
}
```
Note: 
이런식으로 두 번 데코레이트 해서 사용할 수 있습니다.
여기서 조금만 더 개선을 시켜보겠습니다.
데코레이터가 늘어날 때 마다 기존의 Modem 인터페이스에게 동작을 위임하는 코드가 중복되는 것이 마음에 들지 않습니다.
이 위임하는 코드 부분을 따로 빼서 만들고 데코레이터는 실제로 데코레이트 할 부분만 구현하도록 해야할 것 같습니다.


![_](https://www.plantuml.com/plantuml/png/VL1DImCn4BtFhvXZ5rdHgqhfeGUX5SIgzpIPXa2IMJQJNch_ksaZH9V5qtkyDsy-PfL4ZPA31nU5neFIX2ziA9pW1jTE-G8xYgR0iues3uMyaJuMI2IVx7EWHObsS0RGNgLKuslIF2hXyKVSSZQNTbSJOBUv4kppq7yjQmGsxpFYnwlFQKQ7lsEmEHE3-whZ0puPycILq1AWBTHQJrVVihKkslzA8B8Gxbc40_9XSkUGzzvfFB8pQ8gww4w0wAGUTDi-U7_NFVvsQZ6SWQB1omXO5PQ3_mO0)

Note:
이런식으로 구현하게 되면 실제 Modem인터페이스에게 위임을 하는 작업은 ModemDecorator가 모두 수행하고 ModemDecorator 를 상속받은 클래스들은 자신이 데코레이트 하고 싶은 부분만 구현할 수 있겠습니다.


```csharp
public class ModemDecorator: Modem{
	public ModemDecorator(Modem m){
		itsModem = m;
	}
	public Modem getModem(){
		return itsModem;
	}
	public void Dial(string pno){
		itsModem.Dial(pno);
	}
	public void Send(string msg){
		itsModem.Send(msg);
	}
	...
}

public class LoudDialModem: ModemDecorator{
	public LoudDialModem(Modem m){
		super(m);
	}
	public void Dial(string pno){
		getModem().setVolume(10);
		getModem().dial(pno);
	}
}
```

---
## Structure (UML)
![_](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/400px-Decorator_UML_class_diagram.svg.png)


Note:
데코레이터의 구조입니다.

---
## Applicability
- want to perform additional actions without changing your code
- want to configure several additional behaviors at runtime
- when it is difficult to add other functions by inheriting an object

Note:
그럼 데코레이터 패턴은 언제 사용하는 것이 좋을까요? 3가지 경우를 알아보았습니다.
첫 번째는 기존에 사용중인 객체에 대한 코드를 변경하지 않고 추가 동작을 수행하고 싶은 경우 데코레이터 패턴을 사용할 수 있습니다. 비지터 그룹의 패턴이 가진 전형적인 케이스라고 할 수 있습니다.
두 번째로는 여러가지 추가 동작을 런타임에 구성하고 싶은 경우 데코레이터 패턴을 이용할 수 있습니다. 예를들어 비지니스 로직을 레이어로 구성하고 각 레이어에 대한 데코레이터들을 생성하여 런타임에 이런 로직을 조합하여 데코레이터를 연결하게 되면 런타임에 이런 조합을 연결할 수 있게 됩니다. 클라이언트는 기존과 동일한 방법으로 객체를 사용하기 때문에 문제가 없습니다.
마지막 세 번째는 객체를 상속하여 다른 기능을 추가하기 어려울 경우 데코레이터를 사용할 수 있습니다. 기존에 c# 같은 경우에는 Sealed 클래스, java 같은 경우에는 final클래스 의 경우에는 상속이 불가능합니다. 이런 클래스에 추가 동작을 구현해야 하는 경우 데코레이터 패턴이 답이 될 수 있습니다.

--
## Pros and cons
### Pros
- can add new behaviors without using inheritance.
- can be added or removed behaviors at runtime.
- can combine behaviors by wrapping multiple decorators.

Note:
데코레이터 패턴의 장단점 인데요,
먼저 가장 큰 장점으로는 계속 이야기 하였듯이 코드 변경없이, 그리고 상속을 사용하지 않고서 행위를 추가할 수 있다는 점 입니다.
그리고 두 번째로는 그 행동을 런타임에 추가할 수 있다는 점이지요
마지막으로는 이런 여러가지 추가 행동들이 계속해서 결합될 수 있다는 점 입니다. 여러 데코레이터를 결합하는 방식으로 행동이 추가될 수 있습니다. 


### Cons
- It is difficult to remove only certain decorators.
- The initial code that composes the decorator is not neat.

Note:
단점으로는, 여러 데코레이터를 결합한 경우 특정 데코레이터만 제거하기가 어렵습니다. 그리고 데코레이터를 구성하는 초기 Initialize 코드가 깔끔하지 않다는 점이 단점입니다.

---
## End
Note: 마지막으로 패턴간에 비교를 조금 해보면, 데코레이터 패턴과 비슷하게 생긴 패턴으로 어댑터, 프록시 등이 있습니다. 어댑터 패턴의 경우에는 래핑된 기존 개체의 인터페이스를 변경하죠, 그리고 프록시는 동일한 인터페이스를 제공합니다. 데코레이터 같은 경우는 향상된 인터페이스를 제공하죠. 프록시와 데코레이터가 비슷하다고 느낄 수 있습니다.
또한 컴포지트 패턴과도 비슷한데요. 둘 다 재귀 합성을 이용하기 때문입니다. 하지만 데코레이터는 컴포지트와 다르게 하나의 자식만을 가지고 있습니다. 주요한 차이점은 데코레이터는 객체에게 추가된 기능을 구현하지만 컴포지트는 자식의 결과를 요약하는 것 입니다.
 