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
### Follow up

![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLd3EoK_ELgZcKj1EJoq9JatEpqifrj24S7qpyqfBKk5SZ_pI8X2dbWkgroKpFRCaCGTXH71gIKXcRceHM4TmINv1UM99SWP42qILhXsgBYi5a0O5pxoqV2w7rBmKeCC0)

Note:
구현하고자 하는 것은 Clock 객체에서 Time정보를 받아와서 Digital Clock에 업데이트 하는 것.<br>
이것을 구현하기 위해 가장 간단한 해결방법으로 ClockDriver라는 것을 만들어서 업데이트 할 수 있게 합니다.



![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLd3EoK_Eve9oN3BJCqiISr34m1oNeioor28kBYIEWcvAVdcUhXr40JOPDECSKlDIWFO20000)

Note:
ClockDriver는 Clock에게서 getTimes를 하여 DigitalClock에게 setTimes를 하는 역할을 합니다.<br>
그런데 이렇게 구성해 놓으면 테스트를 할 수 없으니까 테스트를 위해서 인터페이스를 추가합니다.


![_](https://www.plantuml.com/plantuml/png/ROv12W8n34NtEKKlq2l8W7C3lK2fcJ5Csf4qSUNX1SaY1PVX_VV8ChI8McEeNCP8Lpxh0TGLPIiLqvqQtawJYZvOQISj_7T7_O3OMONUA03XI9bnqtm9uHaBxK_d7lpXbEsLFvFcdmGSh0Nxmnf_0G00)

Note:
이렇게 구성하여 Mock을 사용하면 다음과 같이 됩니다.


![_](https://www.plantuml.com/plantuml/png/ROx13S8m34NldY8BP0LKYLuvmG9HS8jL9qMEmudXGL5LEMgFyVl_vmr5WsXD3953AUxAQro0ig9C8Q9xKCBxMYNY5hZz-U4uqamQ-BHCUJ7L_MJ_6uK-A01WNiupJkelrg33GlDhvssnOUVhst-xMgzy4h3w5hTPOts40-PdJVm3)




```cs [5-7|9,14|10-12,15-17]
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
Source의 시간을 변경하였을 때, Sink의 시간도 변경되는지 확인. <br>
그럼 ClockDriver는 TimeSource의 시간이 바뀐것을 어떻게 알 수 있을까요? 계속 TimeSource의 Get 함수를 호출하면서 polling 하면서 시간이 바뀐것을 알아내기에는 Cpu의 낭비겠죠. TimeSource가 변경되었을 때 Driver에게 알려주는 방식이 좋겠습니다.


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8oSnD3SulBKfEvKhEIImkLl3DJyv62AAhpCpBHb875uAAEUS01LsACijIYxYuk91u2hevgMabkdP8rg5A5sMcPu3a4IQYkaD1jxH0jmP5Fx83eD88xGwfUIb0Zm80)

Note:
일단 UML에서 필요없는 부분은 제거하고 보겠습니다.



![_](https://www.plantuml.com/plantuml/png/RP11JiOW48NtSmgMDHOEO0nfebjNrGCGEhMaq3R3u4QzkoWcfUHV0VE-ztWmH3R4ANXm6oFDng9uTG77FP75JxWVaP_9VI1rPRc3Rx3Un2XUThkswE-vM_hGnyoraMvRMwfDAnJypvvy7fPhi_7jc0nNFgXa8JtEB7NL7SwjK8hS-y9AwThp81uFGoOW8-bhsX-Uuyv6TJyXqo6_AlNw5HUvTBb5wFfzCE0sfU1_0000)

Note:
TimeSource에서 Driver에게 시간이 변경된 것을 알려주려면 TimeSource 가 Driver를 들고 있어야 합니다.



```cs
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
코드로 보면 이런 식으로 들고있다가 SetTime이 되었을 때, Driver의 Update를 호출해서 Sink로 데이터를 전달할 수 있습니다.




![_](https://www.plantuml.com/plantuml/png/RP11JiOW48NtSmgMDHOEO0nfebjNrGCGEhMaq3R3u4QzkoWcfUHV0VE-ztWmH3R4ANXm6oFDng9uTG77FP75JxWVaP_9VI1rPRc3Rx3Un2XUThkswE-vM_hGnyoraMvRMwfDAnJypvvy7fPhi_7jc0nNFgXa8JtEB7NL7SwjK8hS-y9AwThp81uFGoOW8-bhsX-Uuyv6TJyXqo6_AlNw5HUvTBb5wFfzCE0sfU1_0000)

Note:
다시 UML로 돌아와서, UML 을 보니 순환참조를 하고 있는 것이 있네요.
이렇게 되면 TimeSource 인터페이스는 ClockDriver 하고만 사용할 수 밖에 없어집니다.
이 문제를 해결하기 위해서 인터페이스를 하나 더 만들어봅니다


![_](https://www.plantuml.com/plantuml/png/TP113i8W44Ntd89bZIr7Q9herhYe7W3fr992smpWHhsxA1g1w0gOUVyFVnfZELgd5P6J1Uov07gG6jhPeO0hMRTWjoWlW4KuLkHEXEB6qDqinXnzlekXrANnN6uffp6dShT0aNTjetmnZFN2uz9n6-aY-nUuFnd0FsZaH2ktLBSwrMI4WjvMTRG8yhrInjp2M9tg4pdAy_3HXpnnkl21g9ikCcc7uR-8F403K-UqUW00)

Note:
ClockObserver를 만들어서