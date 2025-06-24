제출일: 2025-06-24

>[!question]
>GQ1. POP는 OOP와 어떤 철학적 차이를 가지는가?
>GQ2. Extension은 어떤 방식으로 결합되며, 함께 쓰일 때 어떤 효과를 가지는가?
>GQ3. POP이 Swift에서 강조되는 이유는 무엇이며, 플랫폼 최적화와 어떤 연관이 있는가?
>GQ4. Extension + ??? 활용 케이스는 어떤 것이 있는가?

## Description

> [!NOTE]
> extension 은 POP 구현을 가능하도록 하는 실질적인 도구 역할을 한다. 그렇다면 기존의 프로그래밍 언어 설계 철학이 OOP -> POP 로 진화한 이유와, 실제 extension 을 더 POP 스럽게 사용할 수 있는 방법을 깊게 알아본다.

### 1. OOP 와 POP 의 철학적 차이
| 항목     | OOP                      | POP                                 |
| ------ | ------------------------ | ----------------------------------- |
| 추상화 단위 | 클래스                      | 프로토콜                                |
| 상속     | 계층적 (상속 트리)              | 비계층적 (기능 조합)                        |
| 코드 재사용 | 클래스 상속 (단일 or 제한된 다중)    | 다중 프로토콜 채택 + 기본 구현                  |
| 다형성 구현 | virtual dispatch, vtable | static dispatch, protocol extension |
| 타입 안전성 | 런타임 다형성                  | 컴파일 타임 타입 안정성                       |

- OOP 는 전통적인 프로그램 서계 방식으로 타입 관계에서 위계를 중시한다. 계층 구조를 만들고, 상위의 속성과 기능을 하위가 상속받는 것이 중심이다.
- POP는 는 프로토콜 자체가 중심이다. 이 타입은 어떤 행위를 할 수 있어야한다. 를 근간으로 행위를 중심으로 한 설계가 이뤄진다. 타입이 할 수 있는 일(행위)의 집합을 정의하기 때문에 이것을 조합하여 기능을 확장시킨다.


```Swift
protocol Printable {
// Pritable 은 printDescription 행위를 해야한다고 정의한다. (행위 부여)
    func printDescription()
}

protocol Identifiable {
// Identifiable를 따르는 모든 타입은 고유 식별자(id)를 가져야한다. (특성 부여 : 고유 식별 능력이 있다는 의미)
    var id: String { get }
}

struct Person: Printable, Identifiable {
    let id: String
    let name: String

// 행위를 구현한다
    func printDescription() {
        print("Person: \(name)")
    }
}
```



### 2. POP 안에서의 Extension 결합 효과

> [!NOTE]
> protocol 과 extension 을 결합하면 이와 같은 효과를 보인다.
> 1. 중복 없이 기능 공유 가능
> 2. 유연한 사용자 정의(override)
> 3. 정적 타입에 대한 안정성
> 4. 클래스 상속 없이도 재사용이 가능

좀 더 살펴보자면, 추상화(프로토콜) - 구현(extension) - 활용(여러 타입에서의 공유) 의 효과가 있다.
코드 예시를 추가해두겠음 ⬇️

1. protocol은 타입이 갖춰야하는 기본 행위를 명시적으로 요구 하고, extension은 이를 재사용할 수 있도록 제공한다. = 유연해짐
2. 클래스가 없어도 다양한 타입에 공통 기능을 부여할 수 있고, 그 영역이 클래스 뿐 아니라 struct, enum, class 모두 가능하다.
3. 조합 가능한 기능을 설계함 (이해하기가 어려웠음.. 이 부분도 코드 예시에 추가 / `extension P where Self: Q`)
4. 정적 디스패치 기반의 성능 최적화 (GQ3. 의 답변이므로 GQ3에서 설명)
5. Extension 을 사용한 다는 것은 모듈성(modularity), 재사용성(reusability), 성능 최적화(performance optimization)을 모두 만족시킨다는 것.


### 3. POP 플랫폼 최적화와 어떤 연관이 있는가?

extension 을 통한 기본 구현은 정적 디스패치(static dispach) -> 컴파일 타임에서 결정 -> vtable 없이도 빠른 호출이 가능함.

프로토콜을 선언만 해두고 활용하지 않으면 런타임 비용이 증가하는 것은 당연함.



### 4. Extension 활용 케이스는 어떤 것이 있는가?
| 분야         | 예시                                     | 설명                   |
| ---------- | -------------------------------------- | -------------------- |
| SwiftUI    | `View` 프로토콜 + `.modifier()` 확장         | 기능 주입, 유연한 UI 설계     |
| Combine    | `Publisher` 프로토콜 + `.map`, `.filter` 등 | 스트림 구성 연산자           |
| Foundation | `Collection`, `Equatable`, `Codable` 등 | 조건부 프로토콜 + extension |


## 주요 기능

| 활용            | 예시                                        | 사용 이유                             |
| ------------- | ----------------------------------------- | --------------------------------- |
| UI 확장         | ViewModifier, View Extension              | 공통 UI 스타일, 동작 주입                  |
| 데이터 모델링       | `Identifiable`, `Codable`, `Equatable` 조합 | 식별, 저장, 비교 기능을 재사용 가능하게           |
| 네트워크 레이어      | APIService, Requestable 등                 | 요청-응답 패턴 추상화 및 공통 구현 제공           |
| ViewModel 구조화 | Loadable, Trackable, ErrorHandleable      | ViewModel에 조합 가능한 상태 관리 기능 주입     |
| 유틸리티          | String/Date/Array 확장                      | 확장된 기능 추가로 표현력 증가                 |
| 테스트/모킹        | Mockable, Injectible                      | 테스트 시 Dependency 주입 및 행동 시뮬레이션 가능 |

아래에 코드 예시를 넣어두겠음!!!!


## 코드 예시

```Swift
// 2. Extension 과 프로토콜의 결합

// 추상화 계층
protocol Resettable {
    mutating func reset()
}

// 구현 - extension
extension Resettable {
    mutating func reset() {
        print("Resetting to default values...")
    }
}

// 활용 - 여러 타입이 같이 사용
struct User: Resettable {
    var name = "Default"
}

class Setting: Resettable {
    var level = 1
}

User().reset() // 동일한 기본 동작 수행
Setting().resset() // 동일한 기본 동작 수행

```


```Swift
// 조건부 구현(조합 가능하도록 설계)


protocol Printable {
    func printDescription()
}
protocol Identifiable {
    var id: String { get }
}


// 나는 Printable 프로토콜을 체택 하고서 Identifiable 프로토콜까지 체택한 애한테만 printDescription 를 줄거야. 라는 의미임.
// 프로토콜 두개의 조합인 타입에게만 기능을 줄 수 있음.
extension Printable where Self: Identifiable {
    func printDescription() {
        print("ID: \(id)")
    }
}


struct User: Printable, Identifiable {
    let id: String
    let name: String
}

// User 는 printDescription() 를 쓸 수 있다.

struct Logger: Printable {
    // Identifiable은 아님
}

// Logger 는 printDescription() 를 쓸 수 없다.



// StructA 타입만을 위한 기능도 가능함
// 등호 방식은 좀 더 디테일하게 타겟팅 하는 방식.
extension Printable where Self == StructA { ... }


```



#### 주요기능 활용 사례 예시 코드
```Swift
// 1. UI 공통 스타일 잡기
// View + extension

extension View {
    func roundedCard() -> some View {
        self
            .padding()
            .background(RoundedRectangle(cornerRadius: 12).fill(Color.white))
            .shadow(radius: 4)
    }
}

Text("Hello")
    .roundedCard()

```


```Swift
// 2. APIService 패턴
// 공통 방식 수행

protocol APIRequest {
    associatedtype Response: Decodable
    var url: URL { get }
}

extension APIRequest {
    func send() async throws -> Response {
        let (data, _) = try await URLSession.shared.data(from: url)
        return try JSONDecoder().decode(Response.self, from: data)
    }
}

struct GetUserRequest: APIRequest {
    typealias Response = User
    var url: URL { URL(string: "https://example.com/user")! }
}

let user = try await GetUserRequest().send()

```

```Swift
// 3. ViewModel 공통 동작

protocol Loadable {
    var isLoading: Bool { get set }
    mutating func startLoading()
    mutating func stopLoading()
}

extension Loadable {
    mutating func startLoading() { isLoading = true }
    mutating func stopLoading() { isLoading = false }
}


// 중복 로딩 로직을 통일화
struct LoginViewModel: Loadable {
    var isLoading = false
    ...
}

```



```Swift
// 4. 데이터 모델에 기능 조합
// 내 모델만을 위한 기능 추가 확장
extension Task {
    var isOverdue: Bool {
        dueDate < Date()
    }
}


```


```Swift
// 5. 자주 쓰는 기능, 유틸 확장
// String, Date, Int 에서 반환 기능 추가
extension Date {
    var weekdayString: String {
        let formatter = DateFormatter()
        formatter.dateFormat = "EEEE"
        return formatter.string(from: self)
    }
}

```


```Swift
// 6. ViewModel 공통 동작

// 조건부 구현(조합 가능하도록 설계) 위에 있음
// SwiftUI나 Combine 같은 프레임워크에서 매우 자주 쓰임

```



```Swift
// 7. 테스트 용 유틸리티 
extension LoginViewModel {
    static var previewMock: LoginViewModel {
        var vm = LoginViewModel(service: MockUserService())
        vm.email = "test@example.com"
        vm.password = "password"
        return vm
    }
}
```




## Keywords
+ [[Extension]]
+ [[POP]]
+ [[Dispatch 비교]]
+ [[Protocol]]

## References
- https://docs.swift.org/swift-book/documentation/the-swift-programming-language/extensions/
- https://iosjiho.tistory.com/127
- 코드 및 정리는.. 대부분 내 친구 GPT

## 작성자
- #TAENI 