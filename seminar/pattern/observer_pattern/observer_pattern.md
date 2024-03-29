## Observer Pattern

Note:
오늘 이야기하려는 패턴은 옵저버 패턴입니다.

---
### Subscribe 
### Notify

Note: 
이 패턴은 구독과 알림설정을 정의하는 패턴<br>
한 객체의 상태가 바뀌면 해당 객체를 구독하고 있는 다른 객체들에게 알람이 가고, 자동적으로 내용을 갱신하는 방식의 의존성을 정의합니다.<br>
우리가 잘 알고 있는 용어로는 발행-구독 모델이라고도 부릅니다.

---
### Observer pattern
![uml](https://www.plantuml.com/plantuml/png/POvB2i9038RtEKMe6nzCyGIbu5v1Jp2TfbAPFaWo1T7UNR4FiDs5B___9QcePGsLXx9Mui8wmaicn1tn2n0FeSsj4lHWCr6sJj5vAuAta3t8wIzpfNifIZmLjxk1Lar7VsnpRhGidXEJB-nXy9sQsZ7fd5_WyHp0EA19ecCSxoES2qi3cj2QTx8Ep8fXFwdNVK-5ccJrGqfr7Yh_0G00)

```cs
# Subject
public void NotifyObservers(string info){
	foreach (var observer in observers){
		observer.update(info)
	}
}
```

Note:
UML을 보면 이런식으로 구성되어 있습니다.<br>
단순합니다. Subject는 Observer 인터페이스를 여러개 들고 있을 수 있고, Notify함수를 실행하면 들고있는 Observer들의 특정함수(update)를 실행하면 됩니다.

---
### Grow up

![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLd3EoK_ELgZcKj1EJoq9JatEpqifrj24S7qpyqfBKk5SZ_pI8X2dbWkgroKpFRCaCGTXH71gIKXcRceHM4TmINv1UM99SWP42qILhXsgBYi5a0O5pxoqV2w7rBmKeCC0)

#### Purpose 
Get time data from Clock when time is updated.

Note:
구현하고자 하는 것은 Clock 객체에서 Time정보를 받아와서 Digital Clock에 업데이트 하는 것.<br>
이것을 구현하기 위해 가장 간단한 해결방법으로 ClockDriver라는 것을 만들어서 업데이트 할 수 있게 합니다.

___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLd3EoK_Eve9oN3BJCqiISr34m1oNeioor28kBYIEWcvAVdcUhXr40JOPDECSKlDIWFO20000)

Note:
ClockDriver는 Clock에게서 getTimes를 하여 DigitalClock에게 setTimes를 하는 역할을 합니다.<br>
그런데 이렇게 구성해 놓으면 테스트를 할 수 없으니까 테스트를 위해서 인터페이스를 추가합니다.

___
![_](https://www.plantuml.com/plantuml/png/ROv12W8n34NtEKKlq2l8W7C3lK2fcJ5Csf4qSUNX1SaY1PVX_VV8ChI8McEeNCP8Lpxh0TGLPIiLqvqQtawJYZvOQISj_7T7_O3OMONUA03XI9bnqtm9uHaBxK_d7lpXbEsLFvFcdmGSh0Nxmnf_0G00)

Note:
이렇게 구성하면 ClockDriver를 테스트할 수 있습니다.
Mock 을 이용해서 TimeSource와 TimeSink 를 구현하면 됩니다.
Mock 을 달면 다음 그림처럼 됩니다.

___
![_](https://www.plantuml.com/plantuml/png/ROx13S8m34NldY8BP0LKYLuvmG9HS8jL9qMEmudXGL5LEMgFyVl_vmr5WsXD3953AUxAQro0ig9C8Q9xKCBxMYNY5hZz-U4uqamQ-BHCUJ7L_MJ_6uK-A01WNiupJkelrg33GlDhvssnOUVhst-xMgzy4h3w5hTPOts40-PdJVm3)<!-- .slide: section=data-auto-animate -->

Note:
이제 Mock 을 이용해서 Clock Driver를 테스트 해볼게요.

___
![_](https://www.plantuml.com/plantuml/png/ROx13S8m34NldY8BP0LKYLuvmG9HS8jL9qMEmudXGL5LEMgFyVl_vmr5WsXD3953AUxAQro0ig9C8Q9xKCBxMYNY5hZz-U4uqamQ-BHCUJ7L_MJ_6uK-A01WNiupJkelrg33GlDhvssnOUVhst-xMgzy4h3w5hTPOts40-PdJVm3)<!-- .slide: section=data-auto-animate -->
```cs [|5-7|9,14|10-12,15-17]
[TestFixture]
public class ClockDeriverTest{
	[Test]
	public void TestTimeChange(){
		var source = new MockTimeSource();
		var sink = new MockTimeSink();
		var driver = new ClockDriver(source, sink);
		
		source.SetTime(3, 4, 5);
		Assert.AreEqual(3, sink.GetHours());
		Assert.AreEqual(4, sink.GetMinutes());
		Assert.AreEqual(5, sink.GetSeconds());

		source.SetTime(6, 7, 8);
		Assert.AreEqual(6, sink.GetHours());
		Assert.AreEqual(7, sink.GetMinutes());
		Assert.AreEqual(8, sink.GetSeconds());
	}
}
```

Note:
이런 식의 ClockDriver Test코드를 구성할 수 있습니다.<br>
ClockDriver를 생성할 때 TimeSource와 TimeSink를 함께 넘겨줍니다. <br>
그리고 Source의 시간을 변경하였을 때, Sink의 시간도 변경되는지 확인합니다. <br>
그럼 이제 ClockDriver를 어떻게 구현할지 한번 생각해볼게요 <br>
ClockDriver는 TimeSource의 시간이 바뀐것을 어떻게 알 수 있을까요? 계속 TimeSource의 Get 함수를 호출하면서 polling 하면서 시간이 바뀐것을 알아내기에는 Cpu의 낭비겠죠.<br> 
TimeSource가 변경되었을 때 Driver에게 알려주는 방식이 좋겠습니다.<br>
다시 UML 로 넘어가서. 

___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8oSnD3SulBKfEvKhEIImkLl3DJyv62AAhpCpBHb875uAAEUS01LsACijIYxYuk91u2hevgMabkdP8rg5A5sMcPu3a4IQYkaD1jxH0jmP5Fx83eD88xGwfUIb0Zm80)

Note:
일단 UML에서 필요없는 부분은 제거하고 보겠습니다. <br>
TimeSource 에서 Driver에게 시간이 변경된 것을 알려주려면 TimeSource가 Driver를 들고 있어야 할 것입니다.<br>
그리고 Time 이 업데이트가 될 때 Driver 를 업데이트 시켜주면 되겠죠<br>
그렇게 UML 을 업데이트하면 다음 그림처럼 됩니다.

___
![_](https://www.plantuml.com/plantuml/png/RP11JiOW48NtSmgMDHOEO0nfebjNrGCGEhMaq3R3u4QzkoWcfUHV0VE-ztWmH3R4ANXm6oFDng9uTG77FP75JxWVaP_9VI1rPRc3Rx3Un2XUThkswE-vM_hGnyoraMvRMwfDAnJypvvy7fPhi_7jc0nNFgXa8JtEB7NL7SwjK8hS-y9AwThp81uFGoOW8-bhsX-Uuyv6TJyXqo6_AlNw5HUvTBb5wFfzCE0sfU1_0000)<!-- .slide: section=data-auto-animate -->

Note:
TimeSource에 SetDriver 함수를 만들어서 Driver를 들고 있을 수 있게 하였습니다.<br>
그리고 Time이 변하는 시점을 컨트롤하기 위해 Mock에 SetTime이라는 함수를 만들었습니다. <br>
TimeSourceMock 에서 SetTime 함수가 호출되면, Clock Driver의 Update 함수를 호출하면 되겠죠<br>
코드도 같이 볼게요<br>

___
![_](https://www.plantuml.com/plantuml/png/RP11JiOW48NtSmgMDHOEO0nfebjNrGCGEhMaq3R3u4QzkoWcfUHV0VE-ztWmH3R4ANXm6oFDng9uTG77FP75JxWVaP_9VI1rPRc3Rx3Un2XUThkswE-vM_hGnyoraMvRMwfDAnJypvvy7fPhi_7jc0nNFgXa8JtEB7NL7SwjK8hS-y9AwThp81uFGoOW8-bhsX-Uuyv6TJyXqo6_AlNw5HUvTBb5wFfzCE0sfU1_0000)<!-- .slide: section=data-auto-animate -->
```cs[|2,7-9|10-12|21-23|27,33-37]
public interface TimeSource {
	void SetDriver(ClockDriver driver);
}

public class MockTimeSource:TimeSource {
	private ClockDriver _driver;
	public void SetDriver(ClockDriver driver){
		_driver = driver;
	}
	public void SetTime(int hours, int minutes, int seconds){
		_driver.Update(hours, minutes, seconds);
	}
}

public class ClockDriver{
	private TimeSink _sink;
	public ClockDriver(TimeSource source, TimeSink sink){
		source.SetDriver(this);
		_sink = sink;
	}
	public void Update(int hours, int minutes, int seconds){
		_sink.SetTime(hours, minutes, seconds);
	}
}

public interface TimeSink {
	void SetTime(int hours, int minutes, int seconds);
}
public class MockTimeSink: TimeSink{
	private int _hours;
	private int _minutes;
	private int _seconds;
	public void SetTime(int hours, int minutes, int seconds){
		_hours = hours;
		_minutes = minutes;
		_seconds = seconds;
	}
	public int GetHours() => _hours;
	public int GetMinutes() => _minutes;
	public int GetSeconds() => _seconds;
}
```
Note:
코드로 보면 TimeSource가 SetDriver를 통해 Driver를 인자로 받아 자기 내부변수에 들고 있습니다.<br>
그리고 시간이 바뀔 때가 되어 SetTime 함수가 호출되면 Driver의 Update 함수를 호출하는 거죠.<br>
그럼 Driver의 Update 함수는 자신이 들고 있는 TimeSink 의 시간을 업데이트 할 수 있습니다. <br>
TimeSink는 SetTime 함수가 호출되어서 자기 시간을 업데이트 하겠죠

___
![_](https://www.plantuml.com/plantuml/png/RP11JiOW48NtSmgMDHOEO0nfebjNrGCGEhMaq3R3u4QzkoWcfUHV0VE-ztWmH3R4ANXm6oFDng9uTG77FP75JxWVaP_9VI1rPRc3Rx3Un2XUThkswE-vM_hGnyoraMvRMwfDAnJypvvy7fPhi_7jc0nNFgXa8JtEB7NL7SwjK8hS-y9AwThp81uFGoOW8-bhsX-Uuyv6TJyXqo6_AlNw5HUvTBb5wFfzCE0sfU1_0000)<!-- .slide: section=data-auto-animate -->

Note:
이제 코드가 돌아는 가게 되었으니, 구조를 보고 업데이트할 것이 있나 생각해볼게요 <br>
UML 을 보니 순환참조를 하고 있는 것이 있네요.<br>
이렇게 되면 TimeSource 인터페이스는 ClockDriver 하고만 사용할 수 밖에 없어집니다.<br>
TimeSource를 사용하기 위해서는 무조건 ClockDriver를 사용할 수 밖에 없는거죠<br>
이 문제를 해결하기 위해서 인터페이스를 하나 더 만들어봅니다

___
![_](https://www.plantuml.com/plantuml/png/RP1D3i8W48Ntd89bZIr7Q9herhYe7W3fr992smpGZNftwmyAf3kOUT-RULCPoz4whOWSB63B1T2Jr52F3WNSoBO6UxmOm1amswbHzFwt8GyY53U67fPgohp-MPVht2owr5iEVQgAtRoAV6llmNkKC-02dgU6su0BxACDrwI14otSLDpBc4aK2bfRbC55oFz96NCJOsNCHpZAQ-VvJvumdL_WruqF6RJzy3L56g22eN5QFG40)

Note:
ClockObserver를 만들어서 순환 참조를 없앴습니다.<br>
그리고 TimeSource는 Driver와의 의존성이 없어지고 Observer를 구현하는 구현체를 받아 사용할 수 있게 되었습니다.<br>
더 개선할 수 있는것이 있나 살펴보면, ClockObserver와 TimeSink 인터페이스의 형태가 같은 것을 볼 수 있습니다.<br>
둘 모두 시간을 업데이트 하는 형태를 가지고 있죠<br>
TimeSink 에서 ClockObserver 역할을 해도 될 것 같아 보입니다. <br>
그렇게 하면 Clock Driver라는 구현체를 없애도 될 것 같아요<br>

___
![_](https://www.plantuml.com/plantuml/png/ROv13e8m44NtSuekCKAFG4XS6nVe0MePfQ4jP3frezvTN11Qmd9-tdjj4rWHHsV1U4PwA8tYQXosOoIDRpYso9TxG7eX5ISxwc6v3l05RLK8uZolM-T_5ttfoh336Jz0ybwMdVNRr2bER5ZZGaeopvwbh3CB88sBAaxLfnOvadzpOTO5zeXjf47VHMT_)

Note:
해서 이렇게 바뀌었습니다 <br>
여기에 TimeSource에서 여러개의 TimeSink가 시간을 받을 수 있으면 좋겠습니다.<br>
Observer가 하나가 아니라 여러개를 등록할 수 있도록 바꾸겠습니다. 

___
![_](https://www.plantuml.com/plantuml/png/ROwn3e8m48RtUugEgD34rO6GE1iJqGUevOI65aZlwgA-kpWGg67y__xVrok8bUVWB9YEqJ-KHd4r3ii-U8qls5smDZI-dE-4_ea-ETfUjrFQm0UqLKJYDBOHM2B_SjAaBgMLdUbQM7mQQKVyDbIA5pJCSY6bDtN3KkOH1R2KYomCsJkFnH2VEMtc1jOMVn9n47ifjr1WLmLdlm00)

Note:
뭔가 그럴듯 해졌죠.
코드로 한번 볼게요

___
![_](https://www.plantuml.com/plantuml/png/ROwn3e8m48RtUugEgD34rO6GE1iJqGUevOI65aZlwgA-kpWGg67y__xVrok8bUVWB9YEqJ-KHd4r3ii-U8qls5smDZI-dE-4_ea-ETfUjrFQm0UqLKJYDBOHM2B_SjAaBgMLdUbQM7mQQKVyDbIA5pJCSY6bDtN3KkOH1R2KYomCsJkFnH2VEMtc1jOMVn9n47ifjr1WLmLdlm00)
```cs [|5-7|9,12|10,13,15-19|26-36|28-30|31-35|45-49]
[TestFixture]
public class ClockDeriverTest{
	[Test]
	public void TestTimeChange(){
		var source = new MockTimeSource();
		var sink = new MockTimeSink();
		source.RegisterObserver(sink);

		source.SetTime(3, 4, 5);
		AssertSinkEquals(sink, 3, 4, 5);

		source.SetTime(6, 7, 8);
		AssertSinkEquals(sink, 6, 7, 8);
	}
	private void AssertSinkEquals(MockTimeSink sink, int hours, int minutes, int seconds){
		Assert.AreEqual(hours, sink.GetHours());
		Assert.AreEqual(minutes, sink.GetMinutes());
		Assert.AreEqual(seconds, sink.GetSeconds());		
	}
}

public interface TimeSource {
	void RegisterObserver(ClockObserver driver);
}

public class MockTimeSource: TimeSource {
	private List<ClockObserver> _observers = new List<ClockObserver>();
	public void RegisterObserver(ClockObserver observer){
		_observers.Add(observer);
	}
	public void SetTime(int hours, int minutes, int seconds){
		foreach(var observer in _observers){
			observer.Update(hours, minutes, seconds);
		}
	}
}

public interface ClockObserver {
	void Update(int hours, int minutes, int seconds);
}
public class MockTimeSink: ClockObserver{
	private int _hours;
	private int _minutes;
	private int _seconds;
	public void Update(int hours, int minutes, int seconds){
		_hours = hours;
		_minutes = minutes;
		_seconds = seconds;
	}
	public int GetHours() => _hours;
	public int GetMinutes() => _minutes;
	public int GetSeconds() => _seconds;
}
```
Note:
TimeSource와 Sink 를 만든 뒤에 Source에게 Sink를 등록시켜 줍니다.<br>
그러고 나면 Source의 시간이 변경되었을 때, Sink 에게도 업데이트가 되도록 하는 것이 목적이죠<br>
그렇게 하기 위해서 TimeSource의 구현을 보면, Register Observer를 하면 Observer들을 List로 들고 있습니다. <br>
그러다가 SetTime 으로 시간이 변경될 타이밍이 되면 들고 있는 모든 Observer들의 Update를 호출해주는거죠<br>
그런 Observer인 TimeSink 는 Update 함수가 호출되었으니, 자신의 시간을 업데이트 할 수 있습니다.

___
![_](https://www.plantuml.com/plantuml/png/ROwn3e8m48RtUugEgD34rO6GE1iJqGUevOI65aZlwgA-kpWGg67y__xVrok8bUVWB9YEqJ-KHd4r3ii-U8qls5smDZI-dE-4_ea-ETfUjrFQm0UqLKJYDBOHM2B_SjAaBgMLdUbQM7mQQKVyDbIA5pJCSY6bDtN3KkOH1R2KYomCsJkFnH2VEMtc1jOMVn9n47ifjr1WLmLdlm00)

Note:
다시 UML로 돌아와서, MockTimeSource에서 SetTime이 호출될 때 모든 Observer 들의 Update를 호출해줘야 합니다<br>
그런데 우리는 실제로 Mock객체를 사용할 것이 아니라 Concrete객체를 만들어 사용해야 할 텐데 그 때마다 특정 이벤트가 발생하였을 때 Observer의 Update를 호출하는 코드를 작성해야 하는거죠.<br>
갱신 작업을 하는 코드를 중복해서 만들기 싫을 뿐더러 우리가 사용할 Clock 객체에 Observer의 등록/업데이트 작업이 속한다는 생각도 들지 않습니다.<br>
그래서 Observer의 등록과 업데이트 작업을 따로 빼서 공통된 모듈로 만들겠습니다.

___
![_](https://www.plantuml.com/plantuml/png/PP3D2e9058Ntzoa6scWvGji24LfNaFK0utZLmH_bpZM8wjqpI7HeUORlV6UuiML5F3GrgDGoAStYQXfCke4qFc5pmS9OHZgd5kcEv1tgJbTJyc5rwjZa3wyCci3wUtY3hfMruZXIZYX1_kOV-C-PjW8mBIFbIgDmPiRwhSyKBzemouKaKvGi8wSZTc8RXck0vOAGGozVaMi7zwyJCxy0nDXcuuq-)

```cs [|3]
public class MockTimeSource: TimeSource {
	public void SetTime(int hours, int minutes, int seconds){
		Notify(hours, minutes, seconds);
	}
}
```

Note: 
TimeSource를 추상클래스로 바꾸고 TimeSource 내에서 Observer를 등록하고 업데이트 하는 작업을 가져갔습니다.<br>
상속받은 클래스에서는 그냥 Notify함수를 호출하기만 하면 Observer들에게 알림이 갑니다.<br>
최종 UML을 한번 그려보면 다음과 같이 됩니다.<br>

___
![_](https://www.plantuml.com/plantuml/png/LP1D2i8m48NtSug0crQRWbk5Kj2rWjK3fEcq3QO_95D1rBiRYzfc6TxxlXScjIGC3Oq6aLioGX8xgmQpRZ0I7x0wOQKieJdc5iqDJR3JdRp-NY4i3XsfyXxKKHFPS0ila5fOoyQQupEaZ--R_-EzgXG9FRO0LEiMIY6HUNQ7N_f2q8o6wNEC6rNLn1EFOHksZkCbm7o1yQ7dpyItWnTDNaYnBTn1tW00)

Note:
이렇게 구현하고 보니, 옵저버 패턴이 만들어졌습니다.<br>
일부러 옵저버 패턴으로 유도하기는 했지만 코드가 진화하는 과정을 한번 살펴보았습니다.<br>
그럼 옵저버 패턴의 기본적인 두 가지 구현 방법을 볼게요.

---
### Two Case

1. Pull Model
    - Observer class has subject.
    - When Notify is called, get the data from subject

2. Push Model
    - When Notify is called, subject push data(or hint) to observer

Note:
옵저버패턴에는 기본적으로 두 가지 구현 방식이 있습니다. <br>
데이터를 가져오는 방식의 pull model과 데이터를 밀어넣어 주는 방식의 push model입니다. <br>
데이터를 가져오는 방식은 Observer가 Subject를 가지고 있고 Notify신호가 오면 들고있는 객체에서 데이터를 꺼내오면 되는 방식입니다. <br>
반면 데이터를 밀어넣어 주는 push model 은 Observer가 Subject를 가지고 있을 수도 있고 가지고 있지 않을 수도 있습니다. <br>
중요한 것은 Notify 신호를 줄 때 전달하고자 하는 데이터를 함께 전달해준다는 것입니다. <br>
각각 언제 사용하면 될까요? <br>

___
### Pull Model
![_](https://www.plantuml.com/plantuml/png/NOz12y8m38Nl-HKzReuEl7eO0pqhAFw0haj7jMwZJGLH_xl56X5l8U-zxoLj8EKfNXnefq8GXzYTK9EuGxN7mGP2N-owWFwAleHgEv4rjwA49u0TasYKHi6653hElIBCXanSkqcVF_F63fQKoolWBjby2IknhEi5l0r2nba-6Zu9EFohSGx-L8U64ZONjJZswSCN)
```cs
public class DigitalClock: Observer{
	public void Update(){
		this._hours = _clock.GetHours();
		this._minutes = _clock.GetMinutes();
		this._seconds = _clock.GetSeconds();
	}
}
```
Note:
코드를 보면 알겠지만, Notify와 Update함수는 그냥 이벤트에 대한 신호 역할만 수행합니다.  <br>
신호를 받으면 Digital Clock이 알아서 시간을 가져와야 하는거죠. <br>

___
### Push Model
![_](https://www.plantuml.com/plantuml/png/JO_F2i8m38VlUOeUDxSEl7eO0pqhA3v0rsMpslsXIGLHtztYhEmMak_x9Qc8bMFVMz1M4OcJhw-eMJmXEs9dYD4bXvhGtT6baEr7DkqZkUHzJYcy0SmGY5Pf594Avdbg5EE2chEtTjItNxqpdM5bvnR4hRBynsp4kYXMy0M4z9DybV4uYF9o5ZseS6Z2Fny0)
```cs
public class DigitalClock: Observer{
	public void Update(int hours, int minutes, int seconds){
		this._hours = hours;
		this._minutes = minutes;
		this._seconds = seconds;
	}
}
```
Note:
여기 보면 밀어내는 구조에서는 Notify와 Update에서 데이터를 전달해 줍니다. <br>
Digital Clock은 전달받은 Data를 이용하여 업데이트를 하면 됩니다. <br>

---
### Example : 
#### CCSDS Gateway (Telemetry Manager)

![_](https://www.plantuml.com/plantuml/png/VP11IyD048Nl-ok6db8b1QyYeL14Sj0AzGziDg_9ucQtp4wielvtanQgpKflEpllVJDlbb4qIzyvPRs0jzg0odKLmM_WknSuT13-AFqs59_gUkrNeTiv2EfiFfRtp86FpoUyItRccAll5AihXnIywQjes5R8HfCoJiT89zLpNpaRM_2qiTAcXLBdDUNzBlyg_WvKAMgY0YUOaFysR-bciRXA5dlKNZVWShZ9488U81jXwEwOuZ_POMmnIN1HcHx11m6nRLgXuCbP_qhACBI0jA-9qTWeODfRQSaSigXz2q-pquApuTyvr0g3OUIfvV3gE_S3)

Note: 
예시로는 CCSDS Gateway의 Telemetry Manager 를 가져왔습니다. <br>
Telemetry Manager에는 옵저버 패턴이 적용되었습니다. <br>
Telemetry Cache는 새로운 TC가 전송되거나 TM 을 수신하였을 때 해당 정보가 저장되는 저장소 입니다. <br>
Telemetry Manager Impl 은 새로운 Stream 요청이 들어오면 Telemetry Publisher를 생성해서 Cache 에 등록합니다. <br>
Stream 은 특정 Key를 가진 TM 에 대한 Stream 일 수도 있고, Decoded TM 에 대한 Stream일 수도 있습니다. 어떤 정보를 가진 Stream 인지는 각 Telemetry Publisher가 가지고 있습니다. <br>
Telemetry Cache 에서는 새로운 TM 이 수신되면 해당 정보를 등록된 모든 Telemetry Publisher 에게 전달합니다. <br>
각 Telemetry Publisher는 자신에게 필요한 정보만을 필터링해서 Stream에 업데이트 합니다. <br>

---
## Applicability
- When tracking the status of a dynamically changing object.
- Need to observe the state of an object only under specific use case.

Note:
옵저버 패턴의 사용을 고려해야 할 때를 정리해보았습니다. <br>
먼저, 동적으로 변하거나 언제 변할지 모르는 객체의 상태를 추적하려고 할 때 옵저버를 고려해야 합니다. <br>
예를들면 유저인터페이스를 통해 변하는 객체의 상태변화 등을 예로 들 수 있습니다. <br>
그리고, 제한된 시간이나 특정 상황에서만 객체의 상태를 구독해야 하는 경우 옵저버를 고려합니다. <br>
이건 런타임에 특정 객체의 상태에 대한 알림을 받거나 받지 않거나 선택할 수 있어야 하는 상황을 이야기합니다.  <br>

---
## End