>[!question]
>GQ1. Generic은 무엇일까?
>GQ2. Generic은 어떤 경우에 사용하는가?
>GQ3. Generic은 왜 쓰는 것인가?

## Description

###### Generic은 무엇일까?

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

---

######  그렇다면, Generic은 어떤 경우에 유용할까?

 아래 두 Int 값을 Swap 하는 함수가 있다고 가정해보자

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) { 
	let temporaryA = a 
	a = b 
	b = temporaryA 
}
```

인자로 들어오는 a와 b의 값이 모두 Int인 경우에는 이상없이 a와 b의 숫자가 바뀔 것이다.

그렇다면 String과 Double의 값 교환도 가능한가?
-> Swift는 타입 안전 언어로, **다른 타입 변수 간의 교환을 허용하지 않는다**

따라서, 아래와 String, Double을 인자로 받는 아래 두 함수를 별도로 만들어줘야 한다.

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) { 
	let temporaryA = a 
	a = b 
	b = temporaryA 
} 

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) { 
	let temporaryA = a 
	a = b 
	b = temporaryA 
}
```

함수 안의 내용은 동일하지만, 인자가 다르다고 해서,
별도로 두 개의 함수를 다시 만들어야하는 번거로움이 있다. 이를 해결하기 위해 "제네릭"을 사용한다

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) { 
	let temporaryA = a 
	a = b 
	b = temporaryA 
}
```



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