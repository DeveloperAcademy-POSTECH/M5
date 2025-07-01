제출일: 2025-07-01

>[!question]
>GQ1. Delegate패턴은 무엇일까?
>GQ2. Swift에서 사용하는 Delegate패턴의 특징은 어떤 것이 있을까?
## Description
> Delegate = 대리자
> 객체가 자신의 책임을 다른 객체에게 위임(delegate)하는 디자인 패턴

![[Pasted image 20250702000245.png]]
- A라는 객체가 어떤 일을 하다가 스스로 처리할 수 없는 특정 이벤트가 발생했을 때, 그 일의 처리를 다른 객체 B에게 맡겨버리는 설계 방식
- 예를 들어 TextField가 있을 때, 이벤트를 처리할 **대리자(Delegate) 객체**를 
  TextField의 delegate 속성에 **지정(assign)** 하여 이벤트 처리를 위임할 수 있습니다.
- 즉 TextField는 순수하게 입력만을 받을 뿐 이후 데이터 처리는 다른 객체에게 위임하는 형태로 만들 수 있다는 것 입니다.

> 정리하자면 Delegate패턴을 이용하기 위해서는 **Delegate를 위한 프로토콜, Delegator, Delegate**가 있어야 한다.

### 목적
> 객체 간의 결합도를 낮춰(느슨하게 만들어) 코드를 더 유연하고, 재사용하기 쉽게 만들기 위해

### 역할
> **위임자 (Delegator)**: 일을 시키는 쪽. "사장님"
> **대리자 (Delegate)**: 일을 받아서 처리하는 쪽. "비서"

-> Delegate패턴에는 두가지 역할(role)이 존재합니다.

- Delegator: 
	-  메인 객체가 일부 작업을 다른 객체에게 **대리수행**하게 합니다.
	   메인 객체는 대리 객체의 메서드를 호출하거나 이벤트를 전달하여 작업을 수행합니다.
	- 이 때 대리객체(Delegate)들이 같은 형태를 가져야 하기에 Delegate프로토콜을 구현하도록 해줘야 합니다.
- Delegate: 
	1. Delegate 객체는 Delegate Protocol(또는 인터페이스)을 채택하게 합니다.
	   이렇게 구현된 메서드는 메인 객체가 요청할 때 실행됩니다.

한개의 Delegator에 적용할 수 있는 여러개의 Delegate가 존재하고그중에서 하나의 Delegate객체를  선택해서 사용 할 수 있는 형태라고 생각하면 됩니다.

#### GPT가 말아주는 쉬운 설명
- **화면 (위임자, Delegator)**: 
	- 화면은 자기가 가진 '버튼'이 언제 눌릴지 모릅니다. 
	- 버튼이 눌렸을 때 무슨 일을 해야 할지도 스스로 정하지 않아요. 화면은 단지 "어? 버튼 눌렸다!"는 사실만 알려줄 뿐입니다.
- **개발자 (대리자, Delegate)**: 
	- 개발자가 미리 화면에게 약속합니다. 
	- "버튼 눌리면 **나한테 알려줘!** 그럼 내가 **이 함수를 실행시킬게**."
	
- **Delegate 패턴은 '역할 분담'이다.**
- **위임자(Delegator)는 이벤트 발생만 알리고, 실제 처리는 신경 쓰지 않는다.
- **대리자(Delegate)는 위임자로부터 소식을 듣고, 실제 작업을 처리한다.

## 장단점
#### 장점
- 상황에 따라 다른 대리자(delegate)를 지정하여 하나의 컴포넌트에 여러개의 처리자를 적용하는 형식으로 코드를 만들 수 있다.(코드 재사용성을 높일 수 있다.)
- 객체간의 결합도를 감소시킨다.

#### 단점
- 대리자(delegate)의 지정이 컴파일타임에 확정되다 보니 런타임에 동적인 위임자 적용이 어렵다.
- 코드의 가독성이 저하될 수 있다: 하나의 기능이 작동하는 과정이 여러개의 객체를 통하다 보니 가독성에 문제생길 수 있다.

### GQ2. Swift에서 사용하는 Delegate패턴의 특징은 어떤 것이 있을까?
- SwiftUI에서는 잘 사용하지 않는다, UIKit에서만 주로 사용하는 패턴이다.
- delegate객체는 미리 정의된 delegate 프로토콜을 채택해서 구현해야 한다.
- Delegator내부에 Delegate 객체를 저장하는 프로퍼티의 경우 weak를 사용해서 메모리 누수를 방지하는것을 매우 권장한다(GPT피셜...)

## 코드 예시
~~~ swift
import Foundation

// 1. Delegate를 위한 Protocol
protocol ChefDelegate {
    func makeMainDish() -> String
}

// Delegator: 손님
class Customer {
    var chef: ChefDelegate

    init(chef: ChefDelegate) {
        self.chef = chef
    }

    func orderDinner() {
        print("손님: 메인 요리 하나 주문할게요!")
        let mainDish = chef.makeMainDish()
        print("손님: '\(mainDish)'가 나왔네요. 잘 먹겠습니다!")
    }
}

// Delegate: 요리사들
class KoreanChef: ChefDelegate {
    func makeMainDish() -> String {
        let dish = "불고기"
        print("한식 요리사: 🇰🇷 \(dish)를 만듭니다.")
        return dish
    }
}

class WesternChef: ChefDelegate {
    func makeMainDish() -> String {
        let dish = "스테이크"
        print("양식 요리사: 🇮🇹 \(dish)를 굽습니다.")
        return dish
    }
}

// --- 실행 코드 ---
print("====== 한식당 방문 🍚 ======")
let koreanChef = KoreanChef()
var customer = Customer(chef: koreanChef)
customer.orderDinner()

print("\n====== 담당 요리사 교체 🍝 ======")
let westernChef = WesternChef()
customer.chef = westernChef // 요리사 교체
customer.orderDinner()
~~~

~~~
====== 한식당 방문 🍚 ======
손님: 메인 요리 하나 주문할게요!
한식 요리사: 🇰🇷 불고기를 만듭니다.
손님: '불고기'가 나왔네요. 잘 먹겠습니다!  

====== 담당 요리사 교체 🍝 ======
손님: 메인 요리 하나 주문할게요!
양식 요리사: 🇮🇹 스테이크를 굽습니다.
손님: '스테이크'가 나왔네요. 잘 먹겠습니다!

Program ended with exit code: 0
~~~

## Keywords
- UIKit
- 디자인패턴
- 위임
- 



## References
- https://ssdragon.tistory.com/139
- https://june0122.github.io/2021/08/21/design-pattern-delegate/
- https://velog.io/@zooneon/Delegate-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C
- https://velog.io/@nala/iOS-Delegate-%ED%8C%A8%ED%84%B4%EC%9D%84-%EC%9D%B4%ED%95%B4%ED%95%B4%EB%B3%B4%EC%9E%90
- https://chanhhh.tistory.com/334
- https://welcometodannas.tistory.com/72




## 작성자
- Steve
