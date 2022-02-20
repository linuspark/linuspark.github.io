## Bridge Pattern

Note:
브리지 패턴입니다.

---

### Separate hierarchies
### Abstraction and Implementation

Note:
두 가지 계층구조를 나누는 패턴입니다.
여기서 계층 구조란 추상화 계층과 구현부를 이야기합니다.

---

## Structure
![_](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Bridge_UML_class_diagram.svg/1500px-Bridge_UML_class_diagram.svg.png)


```csharp
var absraction = new RefinedAbstraction(new ConcreteImplementor());
abstraction.function();
```

Note:
조금 복잡하지만 UML먼저 보고 예시를 보겠습니다.
UML을 보면 계층구조, 즉 상속을 사용한 부분이 두 부분이 있습니다. Absraction 부분(추상화 계층)과 Implementor 부분(구현부 계층)입니다.
Absraction은 implementor를 가지고 있고 내부적으로 Implementor를 사용을 합니다.<br>
이런식으로 추상 부분과 구현 부분을 나누어 놓았습니다.
계층구조를 추상 부분과 구현 부분으로 나누는 것이 브리지 패턴의 핵심입니다.<br>
사실 UML만 보고는 이해가 잘 안갑니다.
우리가 일반적으로 상속을 사용할 때에는 추상화 된 인터페이스를 상속하여 구체적인 구현을 가진 클래스를 사용하는데요, 왜 그런 방법이 아니라 이렇게 복잡한 방법을 사용하는지 한번 살펴보겠습니다.

---
## Example
### Shape and Color
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvKhEIImkLd3EB4hEIOLoWWjB4ujIkRZ0QXLiQdHrOV884PWYXzIy5A0D0000)


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvU82YoZOrEZgAWIbfZXd5YNdf28BEkMKfa94qPG65vOc5c4eXOewfEQb06q60000)


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvN9CAYufIamkKN3EB4hEIKNmWmjB4ujIkRZ0EXHiQdHrOKh055GeA3K5YwXJJcagX8-a7MOYc49eHn55Q8SAEwJcfG1z0000)


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8paWiIAtcqdOfIYpNqEIgvN9CAYufIamkKGXAJG5B8aISSafJ8K9SO4h1faPN5w4Eoa08EsSM9UTW4GykB4qiIaKs0s4od8MG01k3LGPga4DgNWhGLm00)


![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuN9CAYufIamkKGZEI2n8hUPILd3Epyaluj9sAKeirz3agkNYoimhIIrAIqnEXKfnWPKgvEiMPQPdbEX2HA62DZMwG87CekISL6IHuCBInA9KBh1AY4XGQWeorocdD9NB8JKl1UWc0000)


## Example
### Modem
![_](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbNmpKz9pQtcqbPmoKpC0L8UYNdffGL0Hd1gNWgG2afDJiqiI-MgvU9ApaaiBbO8zWPhWVAyGv1vFkvGNcPHSWxKREUSpDIy4Yuk98w2hguTH98BQfg2Rcc1RWrCq3i4Yw2FEnP11w0ZWVu10000)


![_](https://www.plantuml.com/plantuml/png/TO_F2W8X4CRlynJUmrU8XHOAjYvjFO3e5CAiBVxfKhrxRYgPq6bcVlpy-EoS5zQ7YK0RZ_OY9BB3JU7qW1KRqQWuZYXHtn5UbTDhGOHsiPOrZfqmrp172IG9vzWuV7BoDPpgQx-jhnwLbiAZIX2ajf9oZGrfDBwWQ9uTMe2yCCxNoOFA_azBRSZb60ypVnOvTMnWgjh0vdb9BG4DBX4D_lqs-wPtQ5IOw0q0)


![_](https://www.plantuml.com/plantuml/png/RP6nJiCm48RtUuhR8Jm0GrL4fIAbBBGyW6LVYoNNYMA7fI0n50d1oi3G8Xu0iS384n4_1AVK4Q7ovDmV__T_TnuwBofVBmrewwcQ2qbuNYXQhk00HreOFiw4NTGbVqp9AZukI9A-9hW5a4OuXzEyjSGgd_MhoRXVZKXfAJofLZnHekHGi8Kdz4M9nJnzunWLe_nyVkDLonAsTnYUDit7U_Fs4BPVuVxb21tJ7MArWsiUdp5irk5rdKnUfQVHbJHn_bEZoVfEOlUbOr98uc5sCzYv85x4B1liHKAlBV_k375tcDNbNvTybX0duuJuFmEvRPgGDqY65gp3aEVc1_y5)
Adaptor


![_](https://www.plantuml.com/plantuml/png/VOyz2W8n48NxEKKki5UGBGIBrKelC6IU44XMsIITXxSm-6HOBFD-NhwPQzEjzP8bhGtRNIF2vM4e8Z5hhU6OD7-4yOQbg0qsKby_JFqvlGwZpPZpk7nT_FPoyyhvH4L-2cEFdmkxEoPdTapYm1mDpC70oE9kSnSBwpv0gF-16Qlrajy0)


![_](https://www.plantuml.com/plantuml/png/fPBFoXCn5CNtzoakk9Jn0Rhu-C22IdLZzG7Ip9qIo2Hb9kEcAXMgYBeGMh12ArsgY0Ywz8awUGYJwLM6Clq3Gc7kJS_zvPmarwKJXQjo3SeuAZ8X2H_ObF8ftCI-4ZfyxWephYQX6999m-SXIL9F29wrPlgKAYaSfJpS8PQga9hfjxKYutWf3ZykgG3W0fFawe08hR7cx_qgY57f2Y4TOwqn99so9bIki5fJCOKRJP1x-KI7CeRXCZhaabt6xfBSnZf2JPb3cntV6NiOWNupssjYGta88ABEVtYFVZttd-O0nn59DKcUSjgpiiCpExpJQ43zCt3H3P_QywgB6aAdf6ai7058BSeIXuD6nztWKRkxVsVViOQ3T8A19qzgc7T20xnpdq-fzL8kllgHTSxcQBCE2lOQoExdRwR4-_Tlr_NtR_NsjT_yyYzNjngk_pZx2wxVB77tuqNu-SqAdqV7s7uWR3bm_zzp5wQ7zTVFzMABzPUbV_MkNgpFEUe8pcT-_CL0nyxdXk0wfAbo_GS0)

---
## Applicability
- Independant dimention
- subfunctions that varias 
- Runtime change the implementation

---
## Pros and cons
### Pros
- Separate the Abstraction and implementation
- OCP
- SRP


### Cons
- Complecated

---
## End