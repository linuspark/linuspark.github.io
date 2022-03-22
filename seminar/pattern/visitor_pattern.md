## Visitor Pattern

---

### Saparate altorithm from the object on which they operate.

Note:
알고리즘이 객체와 분리되어 동작시키기 위한 패턴을 알아보려고 합니다.
스트레터지 패턴이나, 템플릿 메서드 패턴 처럼 기존에도 알고리즘을 분리하기 위한 디자인 패턴들을 많이 했었는데요. 여기서 분리하는 알고리즘은 어떻게 다르게 분리하는지 알아보려고 합니다.

---
## Problem
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbNmpKz9pQtcqdR9JCpHqEJI3axDIm7H7ebvwQ4585PGQd8PaAxbuWhs1gjMq2qjqAsnWsZbyWx18XWQa5DQZ4NS0MWwq7u0)

> Add Function: ConfigureForUnix(string)

Note:
문제상황부터 한번 보고 가겠습니다.
여러번 나온 모뎀 예시 인데요, 이번에는 모뎀계층구조에 ConfigureForUnix라는 함수를 추가해야 하는 상황입니다. 모뎀을 설정하는데 Unix에서 사용하려면 사용하기 전에 Config string 을 전달해 주어야 한데요. 이 문제를 해결하기 위해서는 몇 가지 방법이 있겠지만, 가장 단순하게 생각하면 모뎀 인터페이스에 ConfigForUnix라는 함수형태를 추가하고 각 구현체에서 해당 구현을 하도록 하는 것이 가장 단순하게 생각할 수 있는 방법이겠죠?
조금만 더 생각해보면 ConfigForUnix가 있다는 것은 ConfigForWindow가 있을 수 있다는 것 아닐까요?
그리고 만약 모뎀 인터페이스를 상속받고 있는 클래스가 3개가 아니라 더 많이 있었다면, 수 많은 클래스를 수정해야 할 것 입니다. 즉, 함수가 추가될 때 마다 계층구조에 있는 모든 클래스가 수정되어야 하는 것입니다.
이런 문제를 해결하기 위해 나온 패턴이 비지터 패턴 그룹 입니다.

---
## Visitor Pattern Group
- Visitor Pattern
- Acyclic Visitor Pattern
- Decorator Pattern
- Extension Object 

Note: 
비지터 패턴을 보기 전에 먼저, 비지터 패턴 그룹에 대해서 이야기를 하려고 합니다. 알고리즘을 객체 구조에서 분리시키기 위한 패턴들을 비지터 패턴 그룹이라고 합니다. 모두 기존의 계층구조를 변경하지 않은채로 새로운 메서드를 계층구조에 추가할 수 있도록 하는 패턴들입니다. 이런 방식은 개방-폐쇄 원칙을 지킬 수 있게 합니다.
이 중에서 지금 하려고 하는 비지터 패턴은 이중 디스패치(더블 디스패치 라고도 하는)를 이용해서 문제를 해결하고 있습니다.

---
### Dispatch
> Noun: the act of sending someone or something to a particular place for a particular purpose.

- Static dispatch
- Dynamic dispatch

Note:
그럼 디스패치가 무엇인지에 대해서 먼저 이야기를 하고 진행하겠습니다.</br>
디스패치는 사전에서 찾아보면 무언가를 보내는 행위, 등의 뜻을 가지고 있습니다.</br> cs에서 디스패치는 메시지를 보내어 원하는 함수를 호출하는 것을 이야기 합니다.</br>
디스패치는 정적 디스패치와 동적 디스패치로 나눌 수 있는데요, 말 그대로 정정 디스패치는 어떤 함수를 호출할 것인지가 컴파일 타임에 이미 정해져 있는 것이고, 동적 디스패치는 런타임에 어떤 함수를 호출할 것인지 파악하여 호출하는 것을 의미합니다.


#### Static Dispatch
```csharp
public class Service(){
    public void Run() {}
}

class TestClass
{
    static void Main(string[] args)
    {
		Service service = new Service();
		service.Run() // Service 클래스의 Run을 호출
    }
}
```
Note:
정적 디스패치의 예시를 먼저 보겠습니다.</br>
사실 이 코드만 보면 무엇이 dispatch를 이야기하는 것인지 의미를 알 수 없을 것입니다.</br>
부연 설명을 하면 main은 Service의 Run함수를 호출합니다. 컴파일 시점에 이미 Service라는 클래스의 Run 함수를 호출할 것을 알고 있습니다.


#### Dynamic Dispatch
```csharp [|23-34|31-33|15-19|1-13|23-34]
interface Service{
	void Run();
}

class ServiceImplA: Service{
	public override Run() { // Do Something 
	}
}

class ServiceImplB: Service{
	public override Run() { // Do Something 
	}
}

class Client(){
	public void UseService(Service service){
		service.Run();
	}
}

class TestClass
{
    static void Main(string[] args)
    {
		var serviceList = new List<Service>();
		serviceList.Add(new ServiceImplA());
		serviceList.Add(new ServiceImplA());

		Client client = new Client();
			
		foreach (var service in serviceList){
			client.UseService(service);
		}
    }
}
```

Note:
동적 디스패치 입니다.
Client 는 Service를 인자로 해서 UseService를 호출합니다. </br>
client의 함수를 보면 Service의 Run을 호출하는 것을 볼 수 있죠 </br>
헌데 자신이 실제로 호출하는 것이 ServiceImplA인지 ServiceImplB 인지는 모릅니다. 그저 전달받는 Service를 상속하는 객체의 함수를 호출할 뿐입니다.</br>
이 것이 동적 디스패치입니다.</br>
인자를 전달받아서 해당 인자의 함수를 호출하는것이죠.


#### Dynamic Dispatch : Double dispatch
```csharp [|1-3|5-13|15-19|17|23-33]
interface Service(){
	void Run(Client client);
}

class ServiceImplA: Service{
	public override Run(Client client) { // Do Something using Client class
	}
}

class ServiceImplB: Service{
	public override Run(Client client) { // Do Something using Client class 
	}
}

class Client(){
	public void UseService(Service service){
		service.Run(this);
	}
}

class TestClass
{
    static void Main(string[] args)
    {
    	var serviceList = new List<Service>();
		serviceList.Add(new ServiceImplA());
		serviceList.Add(new ServiceImplA());

		Client client = new Client();
			
		foreach (var service in serviceList){
			client.UseService(service);
		}
    }
}
```
Note:
이중 디스패치입니다.
동적 디스패치를 두 번 하는 것입니다.</br>
이전 동적 디스패치와 달라진 점을 보면, Service 인터페이스에서 Run을 실행시키기 위한 인자로 Client를 전달받는 것 입니다</br>
그리고 서비스에서는 전달받은 Client 를 이용해서 Run 함수를 실행합니다.</br>
그럼 클라이언트는 Service의 Run 을 실행시키기 위해서 자기 자신을 넘겨줘야겠죠.</br>
Main 에서는 동일하게 동작합니다.</br>
즉, 런타임에 전달된 객체의 함수를 실행하는데(이것이 동적 디스패치이죠), 실행할 함수에 자기 자신을 넘겨주어 다시한번 동적 디스패치를 수행합니다.</br>
클라이언트는 자신이 실행할 서비스가 어떤 서비스인지 자신은 모르고 있습니다. 그냥 전달받은 서비스의 Run을 호출하는 것으로 동적 디스패치가 이루어지게 됩니다. 서버 또한 자신이 수행 할 함수를 외부로부터 인자로 받아서 수행하게 되죠. 동적 디스패치가 두 번 이뤄지게 되는 것입니다.</br>
여기까지가 디스패치의 개념이었습니다.</br>
이제 Visitor 패턴에서 이 동적 디스패치를 어떻게 이용하는지 살펴보겠습니다.

---
## Visitor Pattern
![_](https://www.plantuml.com/plantuml/png/RK_1IWCn4BtFLyonYpJW3qX13qfHn5hl8KstWMnsIIP5gFllpiP1crrpcSbxRzwysIJIaNBduIcGZKTjB3xt1zlX1MxmmMdFPMV3WSkZ3cqUk7cpvin56sC7MXNvXqkE-jZ027CdeIQ_yzIkjky5Rtw1tNO6x5zzJeADOBnE2MLAVZ8YlpyzGEZ9eaxuSWluRyGBO7dNeFhPIoUN6gP7u8jnSWA0eaEbecjFfDHTDGWI2zTvM7y91vAk1YNa0h-smybVB9U4s2w8wlvzS9-blU_3qRKvQbR9nZhwXe_CdVy6)

Note:
비지터 패턴을 이용해서 문제를 해결한 모습입니다.</br>
아직 비지터 패턴이 무엇인지 정확히 모르겠지만 어떻게 해결했는지 한번 살펴보겠습니다.</br>
새로운 함수 accept라는 것이 생겼습니다. </br>
이 Accept는 ModemVisitor라는 인터페이스를 인자로 받는데요, 이 ModemVisitor는 visit라는 함수를 가지고 있습니다. </br>
그리고 각 Modem클라이언트를 인자로 받네요.</br>
개별 클래스에서 동작하는 함수들이 따로 있다고 보는것이 맞겠죠?</br>
UnixModemConfigurator 는 ModemVisitor를 상속받아 그 내용을 구현하고 있습니다.</br>
코드로 보면 더 이해가 쉬울겁니다.


```csharp [|1-6|8-13|15-20|17-19|18|22-32]
public class KTModem: Modem {
	...
	public void accept(ModemVisitor v) {
		v.visit(this);
	}
}

public class SKModem: Modem {
	...
	public void accept(ModemVisitor v) {
		v.visit(this);
	}
}

public class UPlusModem: Modem {
	...
	public void accept(ModemVisitor v) {
		v.visit(this);
	}
}

public class UnixModemConfigurator: ModemVisitor {
	public void visit(KTModem modem){
		modem.configuration_something
	}
	public void visit(SKModem modem){
		modem.configuration_something
	}
	public void visit(UPlusModem modem){
		modem.configuration_something
	}
}
```
Note:
모든 모뎀 클래스는 accept 라는 함수를 구현하고 있습니다.
그리고 이 Accept 함수가 하는 역할은 인자로 전달받은 ModemVisitor 객체의 visit 함수를 호출하는 역할을 합니다. 자기 자신을 인자로 전달하면서 말이죠</br>
그럼 ModemVisitor의 경우 각 인자들 별로 그에 해당하는 함수를 수행합니다. </br>
이렇게 구성하게 되면 ModemVisitor의 새로운 파생형을 만드는 것으로 게층구조에 새로운 함수를 추가할 수 있습니다. </br>
즉, 새로운 함수를 추가하고 싶을 때 마다 계층구조를 변경할 필요가 없어지는 것 입니다.

---
## Structure (UML)
![_](https://upload.wikimedia.org/wikipedia/commons/0/00/W3sDesign_Visitor_Design_Pattern_UML.jpg)

Note:
비지터 패턴의 일반적인 UML 입니다.</br>
클라이언트가 accept함수를 호출하게 되면 실제 구현체인 element a, b 들은 visitor 의 visitElement 함수를 호출하면서 자기 자신을 인자로 넘겨주게 될 것입니다. </br>
그럼 visitor는 전달받은 element 객체를 이용해서 특정 작업을 수행하고, 반환을 하게 되는거죠. </br>
Sequence Diagram을 보면, Client가 요청한 작업을 ElementA가 직접 수행하는 것이 아니라 Visitor 객체를 통해서 작업을 수행하고 있는 것을 볼 수 있습니다.</br>
그럽 비지터 패턴을 언제 사용하면 좋을지 알아보겠습니다. 

---
## Applicability
- to clean up the business logic of **auxiliary behaviors.**
- to perform an operation on all elements of a **complex object structure**
- to implement a behavior that is **only meaningful for some classes**

Note:
첫 번째로는 auxiliary behavior </br>
즉, 보조동작의 비지니스 로직을 깨끗하게 하기 위해서 사용할 수 있습니다.</br>
이 말은 보조동작을 계층구조와 분리해서 계층구조의 클래스들은 메인 비지니스 로직에 집중시키고 보조동작은 계층구조 밖으로 빼낼수 있다라는 말 입니다. </br>
위 Modem예시에서 Modem의 메인 비지니스 로직은 dial, hangup, send, recv입니다. </br>
unix를 위해서 config 를 설정하는 것은 보조동작입니다. </br>
이런 보조동작의 구현은 밖으로 빼서 계층구조의 클래스에서 처리하지 않도록 할 수 있습니다.</br>
두 번째로는 complex object sturcure, 복잡한 계층구조를 가진 객체들 모두에게 특정 operation, 즉, 함수를 추가하기 위해서 입니다.</br>
복잡하게 계층구조과 관계를 가진 모든 객체에게 특정함수를 추가하기 위해서는 많은 노력이 필요합니다. (많은 클래스를 수정해야겠죠) 이런 문제를 Visitor를 통해 해결할 수 있습니다.</br>
마지막으로, 특정 클래스에서만 의미있는 동작을 구현하고자 할 때에도 비지터 패턴을 사용할 수 있습니다. </br>
위에서 설명했던 예로 들자면, KT Modem과 SK Modem은 Unix관련한 Config설정을 해야하고 UPlus Modem은 Config설정이 필요 없다고 했을 때 Uplus Modem이 visitor를 방문했을 때에는 아무것도 수행하지 않도록 구현할 수 있습니다.

---
## Pros and Cons
- Strong for **No changes in hierarchy** and **Frequent changes in behavior.**
- Week for Frequent changes in hierarchy and No changes in behavior.

Note:
비지터 패턴의 장단점 입니다. </br>
비지터 패턴의 경우 계층구조에 대한 변경이 별로 없는 경우, 즉, 파생형이 추가되지 않는 프로그램에 효과적입니다. </br>
파생형을 만들게 되면 기존의 visitor를 구현한 클래스에 해당 파생형의 동작을 모두 추가해주어야 하기 때문입니다. </br>
반면 계층구조에 대한 변경이 없는 경우 기존 클래스의 동작을 OCP를 만족시키며 아주 쉽게 변경이 가능합니다.

---
## End