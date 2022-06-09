## Proxy Pattern

---
### Substitute or placeholder for another object

Note:
프록시(Proxy)는 일반적으로 대리인, 대리권 등을 의미하며 컴퓨터 분야에서는 무언가가 연결될 때 인터페이스의 역할을 하는 어떠한 것을 의미합니다.</br> 
디자인 패턴에서 프록시 패턴이란 어떤 객체에 대한 접근을 제어하는 용도의 대변인/대리인 의 역할을 하는 객체를 제공하는 패턴을 의미합니다.

___
### Problem
![_](https://refactoring.guru/images/patterns/diagrams/proxy/problem-en-2x.png)

 Note:
 Database에 접근하는 코드를 사용하고 있습니다.</br>
 만약 DB에 접근할 때 지연초기화를 사용하고자 한다면 각 클라이언트 코드에 지연초기화 코드를 작성해야 할 것 입니다.</br>
 이런 작업은 많은 중복을 만들어낼 수 밖에 없습니다.

___
### Solution
![_](https://refactoring.guru/images/patterns/diagrams/proxy/solution-en-2x.png)

Note:
이 문제에 대한 해결책으로 중간에 Proxy객체를 두어 Database에 접근하기 전에 Proxy에서 지연초기화를 구현하는 방식으로 처리할 수 있습니다.</br>
즉, 실제 객체를 이용하는 대신 동일한 인터페이스를 가진 대리 객체(Proxy Object)를 이용하여 로직의 흐름을 제어하는 하는 것 입니다.

---
### Structure
![_](https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Proxy_pattern_diagram.svg/878px-Proxy_pattern_diagram.svg.png?20070423140352)

Note:
위 UML은 프록시 패턴이 동작하는 방식을 보여줍니다.</br>
패턴이 적용되는 객체는 세 부분이 있는데 첫 번째 부분은 Client 가 호출할 필요가 있는 모든 메서드가 정의된 인터페이스 부분이고,</br>
두 번째는 실제 구현의 동작이 포함된 RealSubject 부분입니다. 이 부분에서는 로직적인 부분을 담당하여 처리합니다.</br> 
그리고 마지막으로 세 번째 부분은 RealSubject를 가지고 있는 Proxy 부분입니다. Proxy 부분에서는 RealSubject 에게 동작을 위임(delegate)하여 동작을 수행할 수 있습니다.

---
## Example : CD Cover Viewer
![_](https://c.tenor.com/Tu0MCmJ4TJUAAAAC/load-loading.gif)

Note:
CD 커버를 보여주는 Viewer를 만드려고 합니다.
CD 커버이미지는 로컬에 저장하고 있는 것이 아니라 웹 상에서 가져와서 보여줄 것 입니다.
웹에서 이미지를 가져오는 것은 네트워크의 상황과 인터넷 속도에 따라 시간이 오래 걸릴 수도 있습니다.
이 때, 이미지를 불러오는 동안에는 “Loading...” 이라는 이미지를 띄워주도록 구성하고 싶습니다.

___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKhEIImkLl3CJKnFTSulIosgvj9MS4yj0RF3CrDACZGqaKGyKZFJCqh0GW69cNaGGI2tbikne20dCpcn93C_Jq7N3ib0BeVKl1IWJG00)

Note:
먼저 어플리케이션이 이미지 커버를 사용하는 방법은 이렇게 됩니다.
PaintCover() 함수를 호출하게 되면 웹에서 이미지를 다운받아서 UI에 그리게 됩니다.
그리고 이 작업은 시간이 조금 걸리게 되죠.
시간이 걸리는 동안 다른 이미지를 보여주기 위해서 프록시를 사용해봅니다.

___
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbLmpYzBBQhcqbPmJoq1iyCpKqeoD3IHH3nICzCpIi120OcPUH118BUMox6W82SpER4aCpzFGTSEOXQNPsvYUYgOgQ1RMg1RWo8UK9HVKObRt4u1ePlB8JKl1UXO0000)

Note:
기존에 Image Cover가 가지고 있던 함수들을 인터페이스로 분리하고 프록시와 ImageCover모두 같은 인터페이스를 상속받아 구현합니다.
이 때 프록시는 ImageCover 객체를 가지고 있고, Image Cover가 웹에서 이미지를 모두 다운받으면 동작을 위임시킵니다. 그 전에는 프록시가 Loading.. 이라는 이미지를 UI 에 보여주게 됩니다.

___
```csharp [|1-5|7-9|12|20-25|27-32|35-37|38-48]
public interface Cover{
	int GetCoverWidth();
	int GetCoverHeight();
	void PaintCover(Context c);
}

public class ImageCover: Cover {
	// real implementation
}

public class ImageProxy: Cover {
	private ImageCover _cover;
	private bool _isRetrieving = false;
	private string _url;

	public ImageProxy(string url){
		_url = url;
	}

	public int GetCoverWidth(){
		if (_cover != null)
			return _cover.GetCoverWidth();
		else
			return 100;
	}

	public int GetCoverHeight(){
		if (_cover != null)
			return _cover.GetCoverHeight();
		else
			return 100;
	}

	public void PaintCover(Context c){
		if (_cover != null) {
			_cover.PaintCover(c);
		}
		else {
			c.DrawString("Loading...");
			if (!_isRetrieving)
				return;
			
			_isRetrieving = true;
			Task.Run(()=> {
				_cover = new ImageCover(_url);
				c.Repaint();
			});
		}
	}
}
```

___
## Example : Shopping Cart
![_](https://www.plantuml.com/plantuml/png/HSuxxi8m30RmtQU8ElqVfE84g19J9xY2QGAAo0DiHmYXtfscX7Rn4FltvUjOa2G73uD7PhNFB8c2fHVeYC62HcF8CeC-EUzTlFJnd2YWk1GLZ53TWSni3CfaM50om_zaUx7XvqZ9v44swfunYfFdxohBmGI_6nZhUpLBZnkveKHVSx5BJwLtgjcsxL_pwG3pLDgaHrnNvHCeevpNPEXiD3duDm00)

Note: 
장바구니 어플리케이션을 구현하려고 합니다.</br>
고객과 주문 목록(쇼핑카트), 주문 목록에 있는 상품 자체를 위한 객체 등이 있을 수 있습니다. </br>
아주 단순한 구현은 이런 UML로 구현할 수 있습니다.</br>

___
```csharp
public class Order{
	private List<Item> _items = new List<Item>();
	public void AddItem(Product p, int quantity){
		var item = new Item(p, quantity);
		_items.add(item);
	}
}
```
Note:
객체 모델로 구현한다고 했을 때 이런 식으로 구현할 수 있습니다.
우리에게 익숙한 형태죠

___
```csharp
public class AddItemTransaction{
	public void AddItem(int orderId, string sku, int qty) {
		Statement s = itsConnection.CreateStatement();
		s.executeUpdate($"insert into items value {orderId}, {sku}, {qty}");
	}
}
```
Note:
관게형 DB에 쿼리를 통해 저장하는 방식을 구현한다고 했을 때에는 이런 형식이 될 수 있습니다.
세부 내용을 자세히 알 필요는 없습니다. 중요한 것은 데이터베이스에 추가하는 경우 SQL을 작성하고, 쿼리를 날려 저장한다는 것 입니다.

위 두 방식은 모두 데이터를 저장하는 방식을 코드에서 드러내고 있습니다.
그런 방식은 SRP 와 OCP를 위반합니다.

---
## Example : CCSDS Gateway Proxy
![_](https://www.plantuml.com/plantuml/png/ZO_12i8m38RlUOgym5v0HfbiABkDJV2ykMm8JJDcYWe-l9ruQ3rvIlBnaJyfRjglwjd2HNWHHqwnBBkgE_PAz_uPuIzfLdd4U6wRCZ2LmKyHtjUdxWpmAPYPL8iJCFDBzleHegn_18F9nXtc-KYMvZ0R0qwKS11LOPGf_UFAYoNU3ZvKTGlaHjnqA4BdsRu1)

Note:
Gateway에서는 FDIR을 구현하기 위해 프록시를 사용하고 있습니다.
실제 위성과 CCSDS Gateway를 이용해서 Test를 수행하게 되면 잘못된 Telemetry를 수신받기 위해 위성에서 잘못된 Telemetry를 전송하도록 만들거나, CCSDS Gateway에서 잘못된 Telemetry를 전송하도록 만들어야 한다.
실제 Product를 잘못 동작하게 만들 수는 없기 때문에 Gateway Proxy를 이용하여 FDIR을 구현하였다.
Gateway Proxy는 gRPC 인터페이스가 CCSDS Gateway와 동일하기 때문에 AIT SW는 CCSDS Gateway 대신 Gateway Proxy를 이용할 수 있다. 
Gateway Proxy는 모든 gRPC 인터페이스에 대한 동작을 CCSDS Gateway 에게 위임한다.
특정 FDIR를 시험해야 할 때에는 Proxy에서 FDIR의 기능을 On 시키고, 그렇게 되면 Gateway Proxy는 CCSDS Gateway에서 수신된 정상 Telemetry를 잘못된 Telemetry로 변환해서 AIT SW에게 전달한다.
이런식으로 Proxy를 이용하여 FDIR을 구현하게 되면, 실제 Product인 CCSDS Gateway의 변경 없이 원하는 기능을 구현할 수 있다.

---
## Applicability
- Remote Proxy
- Virtual Proxy
- Protected Proxy
- Logging Proxy
- Caching Proxy