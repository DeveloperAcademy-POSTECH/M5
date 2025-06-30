제출일: 2025-06-29

>[!question]
>GQ1. Swift의 Protocol은 SOLID 원칙과 어떻게 연결될까

## Description
>SO ' L ' ID 
>every **subtype** should be **replaceable** for their **parent.**
>**Consistency** between parent and child types

LSP ~= 다형성
*" parenttype의 행동 규약을 subtype가 위반하면 안된다 "*

근데 이걸 위배하기도 쉽지가 않다
subtype가 잘못된 overriding을 하는 경우
1. **parent의 method signature을 임의로 변경**
``` swift
class Animal {
    func makeSound() -> String {
        return "뭔가 string"
    }
}

class Dog: Animal {
    // return type 을 임의로 변경 → LSP 위배
    override func makeSound() -> Int {
        return 123
    }
}```
2. parent type의 method를 다른 의도로 overriding 
	``` swift
class Bird {
    func fly() {
        print("I can fly!")
    }
}

class Penguin: Bird {
    override func fly() {
        // 논리적으로 잘못된 상속을 받는 케이스
        // error는 아닌데 틀린거지
        print("I can fly!") 
    }
}```

## GQ1. Swift의 Protocol은 LSP와 어떻게 연결될까


## Keywords
+ 파생된 키워드들을 작성

## References
- 참고한 레퍼런스를 작성 (예 : Apple의 공식 문서)

## 작성자
- 작성한 사람의 닉네임 (예: # Air )
- 