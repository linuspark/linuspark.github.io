## State Pattern

---
#### an object alter its behavior when its internal state changes.

Note:
상태가 변할 때 객체가 행동해야 하는 것을 정의합니다.
즉, 스테이트 머신에 대한 패턴

---
### Automata (Finite State Automata)
![_](https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/DFAexample.svg/1920px-DFAexample.svg.png)

Note:
오토마타에 대해서 먼저 이야기를 하려고 합니다.
그 중에서도 유한상태 오토마타에 대한 이야기 입니다.
오토마타 이론은 컴퓨터과학 CS에서 계산 능력이 있는 추상기계(오토마타)와 그 기계를 이용해서 풀 수 있는 문제를 연구하는 분야인데요.
흔히 우리가 오토마타를 이야기 할 때에는 유한상태머신을 이야기 합니다.
유한 상태 머신은 위 그림 처럼 기계에는 상태가 있고, 특정 입력이 주어지면 다른 상태로 전이되는 것을 표현합니다. 위 예시에서는 S1상태에서 입력으로 1을 받게 되면 다시 S1상태로, 0을 받게 되면 S2상태로 전이됩니다.

___
### Example(Turnstile)
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuOhMYbNGrRLJyCbFpavDuO9G2hef-ULvGD7Gl1o8W9L2Sdvcddufc0zL1T47aoz8B2xMJyn9BChbWklIBIt0K0AIGbA1nPAIZCIyxChyKYw7rBmKe1i1)

Note:
위 이미지는 지하철 개찰구의 유한상태머신을 상태전이 다이어그램으로 표현한 것 입니다.
유한 상태 머신에는 크게 3가지의 요소가 있는데요, 상태 / 이벤트 / 액션 입니다.
위 다이어그램에서 보면 지하철 개찰구의 상태는 2가지가 있습니다. Locked상태와 Unlocked 상태. 
이벤트도 두 가지 이벤트가 있죠. Coin 돈을 넣는 이벤트와 Pass 개찰구를 통과하는 이벤트.
액션은 4가지가 있습니다. Locked 상태에서 Pass 개찰구를 통과하려고 하면 Alarm 이 울립니다. Locked 상태에서 Coin 돈을 넣는 이벤트가 발생하면 Unlock 행동이 수행되어야 합니다. 그리고 Unlocked상태로 상태가 전이되어야 합니다. Unlocked 상태에서 또 Coin 돈을 넣게 되면 Thankyou 고맙다고 하네요, Unlock 상태에서 Pass 개찰구를 지나게 되면 다시 Lock 행동이 수행되고 Locked 상태로 상태가 전이됩니다. 

___
```csharp
public class Turnstile
{
	public void event(int eventCode)
	{
		switch(this.state)
		{
			case LOCKED:
				switch(eventCode)
				{
					case COIN:
						this.state = UNLOCKED;
						turnstileController.Unlock();
						break;
					case PASS:
						turnstileController.Alarm();
						break;
				}
				break;
			case UNLOCKED:
				switch(eventCode)
				{
					case COIN:
						turnstileController.Thankyou();
						break;
					case PASS:
						this.state = LOCKED;
						turnstileController.Lock();
						break;
				}
				break;
		}
	}
}
```

Note:
유한 상태 머신을 가장 직관적으로 해결할 수 있는 방법은 스위치 문을 사용하는 것입니다.
각각의 상태에 대한 스위치에서 또, 각각의 이벤트에 대한 스위치를 추가하면 액션을 수행하게 되는 것 이지요.
코드로 봐도 직관적인 것을 볼 수 있습니다.

---
## State Pattern
![_](https://upload.wikimedia.org/wikipedia/commons/e/ec/W3sDesign_State_Design_Pattern_UML.jpg)

Note:
State 패턴은 이런 유한 상태 머신으로 동작하는 기계를 전형적인 패턴으로 풀고자 만들어놓은 방식입니다.
먼저 UML 을 통해 이 패턴이 어떻게 동작하는지를 알아보고 가겠습니다.
패턴의 구조 자체는 간단합니다. 여기서는 state 라는 인터페이스가 있고 이 인터페이스를 상속받은 여러가지 State구현체들이 존재합니다. 즉, 각각의 상태마다 하나의 클래스에서 구현하고 있습니다.
이 상태 클래스를 Context라는 객체가 들고 있습니다. 현재 상태에 대한 클래스를 가지고 있게 되는거죠.
Sequence 다이어그램을 보면 상태가 전이되는 것을 볼 수 있는데요
현재 상태가 State1이라고 할 때, Context는 State1을 들고 있습니다. 그리고 이벤트가 발생하여 Operation이 수행되면 State1은 다음에는 어떤 상태로 전환해야 하는지를 알려주게 되요. 그러면 Context는 현재 상태였던 State1을 다음 상태인 State2로 바꾸는 작업을 합니다. 그러다가 다시 이벤트가 발생하여 Operation이 수행되면 State2는 다음의 상태를 넘겨주게 되겠죠.
이런 방식으로 동작하게 됩니다.

---
### Example(Turnstile)
![_](https://www.plantuml.com/plantuml/png/ZOv13e9034NtFGM96ms18nWEu4uzG3ECCg4KCjDP6ENk5XE4J2HnrQR_vVLNGT1Bx0WCOGZP9NeEuiXcJyXDxtX_W7pGHEUEUjDEC_AyIOFSFleuxKZeErr60CTY_GsDNNndGR6pytkTvQl326cLapwpzKUGHbUcLGXB-yAxUoF5CIa0FwGAnw5uRQFw98N_zoxafh4irr1bkOuTlG40)

Note:
위에서 보았던 지하철 개찰구 문제를 State 패턴으로 해결한 모습입니다.
State인터페이스를 상속받은 두 가지 상태 클래스가 있습니다. LockedState와 UnlockedState 입니다.
그리고 각각 이 상태 클래스들은 이벤트에 대한 내용을 구현해야 합니다. 자신의 상태일 때 이벤트가 발생하면 수행해야 하는 동작들을 정의하는 것 입니다.
Context 역할을 하는 Turnstile 객체는 모든 이벤트와 엑션들을 가지고 있습니다. 이벤트의 경우 특정 이벤트가 발생하였을 때 현재 상태에 따른 이벤트를 발동시키기 위해서 가지고 있는 것 이구요, 액션들은 실제로 Context가 수행해야 하는 작업들을 가지고 있는 것 입니다.
실제 코드로 구현을 보겠습니다. 

___
```csharp [|1-5|7-18|20-31]
interface TurnstileState
{
	void coin(Turnstile t);
	void pass(Turnstile t);
}

public class LockedTurnstileState: TrunstileState
{
	public void coin(Turnstile t)
	{
		t.setUnlocked();
		t.Unlock();
	}
	public void pass(Turnstile t)
	{
		t.alarm();
	}
}

public class UnlockedTurnstileState: TurnstileState
{
	public void coin(Turnstile t)
	{
		t.thankyou();
	}
	public void pass(Turnstile t)
	{
		t.setLocked();
		t.lock();
	}
}
```
Note:
개별 상태 클래스는 단순합니다.
이벤트에 대한 구현을 각각의 상태에 대해서 정의하고 있으면 됩니다.
예를들어 Locked 상태에서는 Coin이벤트를 받았을 경우 Unlock 상태로 전환하고 Unlock 에서 수행해야 하는 액션을 수행합니다.
Pass 이벤트에서는 Alarm 액션을 수행하게 됩니다.
Unlocked 상태에서는 Coin 이벤트를 받으면 Thankyou 액션을 수행하고, Pass 이벤트를 받으면 Locked상태로 전이하고 Lock 액션을 수행하면 됩니다.

___
```csharp [|3-7|9-12|13-20|21-28|29-44]
public class Turnstile
{
	private static TurnstileState lockedState = new LockedTurnstileState();
	private static TurnstileState unlockedState = new UnlockedTurnstileState();

	private TurnstileController controller;
	private TurnstileState state = lockedState;

	public Turnstile(TurnstileController c)
	{
		controller = c;
	}
	public void coin()
	{
		state.coin(this);
	}
	public void pass()
	{
		state.pass(this);
	}
	public void setLocked()
	{
		state = lockedState;
	}
	public void setUnlocked()
	{
		state = unlockedState;
	}
	public void thankyou()
	{
		controller.thankyou();
	}
	public void alarm()
	{
		controller.alarm();
	}
	public void lock()
	{
		controller.lock();
	}
	public void unlock()
	{
		controller.unlock();
	}
}
```
Note:
Context 객체에서는 할 일들이 조금 많이 있습니다.
가장 중요한 할 일은 현재의 State를 가지고 있고 이벤트가 발생했을 경우 현재 상태 클래스에게 자기 자신을 넘겨주면서 이벤트를 호출하는 것 입니다.
만약에 상태가 바뀌어야 하는 상황이 오면 상태 클래스가 알아서 상태를 바꿔놓을 것 입니다.
그렇게 하기 위해서 상태 전이에 대한 함수를 정의하고 있어야겠죠.
그리고 마지막으로 액션에 대한 내용을 정의하고 있어야 합니다.
Context를 통해서 각각의 상태 클래스가 액션을 수행하기 때문에 Context는 각각의 액션에 대한 정보를 가지고 있어야 합니다.
이렇게하면 잘 동작하겠죠

---
### Compare with strategy pattern
Strategy pattern
![_](https://upload.wikimedia.org/wikipedia/commons/4/45/W3sDesign_Strategy_Design_Pattern_UML.jpg)
State Pattern
![_](https://upload.wikimedia.org/wikipedia/commons/e/ec/W3sDesign_State_Design_Pattern_UML.jpg)

Note:
두 패턴의 UML 을 보면 아주 비슷한 것을 알 수 있습니다.</br>
두 패턴 모두 Context 가 있고, 파생형이 있는 여러가지 다형적인 기반클래스에게 동작을 위임한다는 공통점이 있습니다.</br>
차이라고 한다면 파생형 클래스가 Context를 들고 있다는 점이 있습니다. 이 참조를 통해서 Context 의 메서드를 호출할 수 있고, 상태를 바꿀 수 있다는 것이 스테이트 패턴에서의 중심점입니다.</br>
반면 스트레터지 패턴에서는 이런 제약이 없습니다. 즉, 스테이트 패턴은 스트레터지 패턴의 변형이라고 볼 수 있는것 입니다. 다만 사용의 이유를 생각해 보면 언제 다르게 써야 하는지를 알 수 있습니다.</br>

---
### Pros and Cons
#### Pros
- Particular states into separate classes. (SRP)
- New states without changing. (OCP)
- Simplify the code of the context

Note:
장점으로는 상태에 대한 것을 개별 클래스로 분리할 수 있다는 것 입니다.</br>
상태에 대한 점을 Switch 케이스가 아니라 상태로 분리할 수 있다는 것은 코드를 조금 더 책임에 따라 나눌 수 있다는 것을 의미합니다.</br>
두 번째로는 새로운 스테이트를 추가하기 위해서는 기존의 코드를 크게 변경하지 않아도 된다는 점 입니다. OCP 를 만족하는 것 입니다.</br>
마지막으로 Context 코드를 간단하게 만든다는 것 인데요, 여기는 조금 의견이 갈릴 수 있겠지만, 기본적으로 Switch 문으로 작성된 것과 비교를 하면 Context 에 상태 변환 로직이 빠지기 때문에 조금 더 깔끔해지는 효과가 있습니다.</br>

___
#### Cons
- Logic is scattered.

Note:
단점으로는 로직이 복잡해진다는 점.
---
## End