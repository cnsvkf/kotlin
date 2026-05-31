## 개념

- 객체의 타입을 __다른 타입처럼 다루는 것__

## 예시

```kotlin
open class Animal

class Dog : Animal()
```

: Dog 객체가 있을 때, 이 _____객체를 Animal 타입으로 다루거나 다시 Dog 타입으로 돌려서 사용_____ 하는 것이 타입 캐스팅

-> 관계

    Animal
    ↑
    Dog

    Dog는 Animal을 상속받았기 때문에 Dog는 Animal이다