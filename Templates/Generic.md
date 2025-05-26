>[!question]
>GQ1. Generic은 무엇일까?
>GQ2. Generic은 어떤 경우에 사용하는가?
>GQ3. Generic은 왜 쓰는 것인가?

## Description

Swift.org 원문에 따르면 제네릭이란
**Write code that works for multiple types and specify requirements for those types.**
("여러 자료형에서 작동할 수 있는 코드를 작성하되, 그 자료형들이 충족해야 할 조건(요구사항)을 명확히 해라.")

이를 더 쉽게 풀어서 설명해보자면
"타입에 의존하지 않는 유연한 범용 코드를 작성할 때 사용할 수 있으며, 이를 통해 중복을 피할 수 있다" 이다.

애플 원문에 따르면 Swift에서 가장 강력한 기능 중 하나로, 
Swift 표준 라이브러리의 많은 부분이 제네릭으로 작성돼있다고 한다. 

우리가 흔히 사용하는 1) Array 2) Dictionary 타입도 모두 제네릭 컬렉션으로 이루어져 있다.
생각해보면 우리는 어떤 타입의 배열과 딕셔너리를 아무렇지 않게 정의하며 쓰고 있었는데
이는 모두 타입에 제한을 두지 않는 제네릭으로 작성돼있기 때문이다

--------------------------------

그렇다면, Generic은 어떤 경우에 유용할까?


## 코드 예시
- 타입에 따라 정의하는 Array의 예시
```swift

let intArray: [Int] = [1, 2, 3, 4]
let stringArray: [String] = ["apple", "banana", "cherry"]
let doubleArray: [Double] = [3.14, 2.71, 1.62]

```

- Array 정의 원문
```swift
struct Array<Element> {
    // 내부 구현은 생략
}
```

## Keywords
+ 파생된 키워드들을 작성

## References
- 참고한 레퍼런스를 작성 (예 : Apple의 공식 문서)