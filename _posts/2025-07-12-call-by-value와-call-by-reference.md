---
layout: post
title: Call by Value와 Call by Reference
date: 2025-07-12 02:04 +0900
categories: [ Java ]
---

#  Call by Value와 Call by Reference는 무엇일까?
---
각각의 프로그래밍 언어들은 메소드 **매개변수(parameter)를 호출하는 방법**이 있는데, 대표적으로 <u><strong>Call by Value</strong></u>와 <u><strong>Call by Reference</strong></u>가 있다. 

Call by Value는 함수에 인자를 전달할 때 '원본 값의 복사본'을 전달하는 방식이다. <br />
Call by Reference는 함수에 인자를 전달할 때 '참조(주소 값)를' 전달하여 메소드 안에서 원본을 조작할 수도 있는 방식이다.

<strong>자바는 Call by Value 방식만을 사용</strong>하는데, 자바가 갖는 자료형의 특성으로 인해 이해하는 데 있어 헷갈릴 수 있으니 한 번 짚고 넘어가 보자.


## 자바의 자료형
자바의 자료형은 크게 두 가지로 나뉜다.
- 기본형(primitive type) - short, int, long, float, double, boolean, char
- 참조형(reference type) - Class Type, Array Type, Interface Type 등 기본형을 제외한 타입

여기서 참조형은 Stack 영역에서 실제 데이터가 있는 Heap 영역의 주소 값을 가지고 있기에 Call by Reference 방식을 사용한다고 생각할 수 있는데, 자바는 Call by Value만 사용한다.  <br />
기본형부터 차근차근 살펴보자.

## Call by Value (기본형)
---
기본형은 다음과 같이 값을 복사 및 전달한다.
```java

public class Main {
    
    public static void main(String[] args) {

        int count = 0;
        increase(count);
        System.out.println(count);  // 출력: 0 => 값의 변화가 없음
    }

    static void increase(int count) {
        count += 10;
    }
}
```
코드의 흐름은 다음과 같다.

1. 프로그램 실행 시, Main 클래스가 로딩되면서 Stack 영역에 main 메소드의 스택 프레임이 적재된다.
2. main 스택 프레임에는 main 메서드의 지역 변수인 `count`가 0의 값으로 생성된다.
3. increase 메소드가 호출됨에 따라 Stack 영역에 increase 스택 프레임이 적재되고 매개 변수 `count`가 0의 값을 가지며 생성이 된다. <br />이 때, 메서드 내의 코드가 실행되며 increase 메서드의 count 값이 10이 된다.
4. main 메소드의 지역 변수인 count 변수의 값은 변하지 않고 그대로 유지되어 0이 출력된다.

예제 코드를 보면 `count`의 값을 증가시켜서 10이 출력되는게 맞지 않나? 라는 생각을 할 수가 있는데, 이를 자바의 메모리 관점에서 보면 쉽게 이해할 수 있다. <br />

![스텍프레임(기본형)](/assets/img/stack-frame//stack-frame(primitive).png)

이렇듯, 메모리의 관점에서 보면 main 메소드의 `count` 변수는 increase 메소드에 의한 영향을 받지 않는다. 단지, increase 메서드에는 `count` 값의 복사본을 전달할 뿐이다. 

> [ 스택프레임 ] <br />
> JVM은 메소드와 메소드의 안의 정보(매개 변수 및 지역 변수 등)를 하나로 묶어 Stack 영역에 적재하는데 이를 스택 프레임(Stack frame)이라고 한다.
{: .prompt-tip } 

## Call by Value (참조형)

참조형은 다음과 같이 값을 전달한다.
```java
public class Main {

    public static void main(String[] args) {

        int[] arr = { 7 };
        increase(arr);
        System.out.println(arr[0]); // 출력: 77 => 같은 주소를 참조하는 변수가 값을 변경
        
    }

    void increase(int[] arr) {
        arr[0] += 70;
    }
}
```

예제 코드 결과로는 77이 출력되었다. 그럼, 주소 값 참조를 통해 값을 변경했으니 Call by Reference가 아닌가? 라는 생각이 들 수도 있지만, 자바는 오직 Call by Value로만 동작한다는 것을 상기해야 한다. <br /> 
코드의 흐름은 다음과 같다.

1. 프로그램 실행 시, Main 클래스가 로딩되면서 Stack 영역에 main 스택 프레임이 적재된다.
2. `int[] arr = { 7 }`이 실행됨에 따라, 배열 객체는 Heap영역에 생성되고 변수 arr은 배열 객체의 주소 값을 가진다.
3. increase 메소드가 실행되면서 Stack 영역에 increase 스택 프레임이 적재되고, 매개변수 `arr`은 main 메소드의 변수 `arr`의 <strong>참조 값의 복사본</strong> 을 통해 동일한 배열 객체를 참조한다. <br />
그리고 코드에 따라 `arr[0]`의 값에 70을 더한다.
4. main 메소드의 `arr`변수가 참조하는 배열의 값이 변경되었으므로, 77을 출력한다.

![스택프레임(참조형)](/assets/img/stack-frame//stack-frame(reference).png)

이처럼, increase 메소드의 매개변수 `arr`은 main 메서드 변수 `arr`이 참조하는 주소 값의 복사본 (Call by Value)을 통해 같은 객체를 참조할 뿐, 원본 객체에 대한 직접적인 영향을 끼칠 수는 없다.


사실 나는 여기까지 공부를 했어도 내가 정확히 이해했는지를 확신할 수 없었다. Call by Reference는 어떻게 동작하는건지 모르니, Call by Value와 Call by Reference의 차이를 명확하게 이해할 수 없었기 떄문이다.
그렇다면 Call by Reference는 어떻게 동작하는지 한 번 알아보자.

## Call by Reference
---
Call by Reference를 사용하는 대표적인 언어가 바로 C++언어 이다. C++에서 Call by Reference가 어떻게 동작하는지 살펴보자.

```cpp
#include <iostream>
#include <string>

class Person {
public:
    std::string name;
    int age;

    Person(const std::string& name, int age)
      : name(name), age(age) {}
};

void changeRef(Person*& p) {
    p = new Person("조조", 40);
}

int main(void) {

    Person p = new Person("관우", 30);
    changeRef(p);
    std::cout << "이름: " << p->name << std::endl;
    std::cout << "나이: " << p->age << std::endl;

    // 출력 결과: 이름: 조조, 나이: 40
    return 0;

}
```

코드의 흐름을 그림과 함께 살펴보자.

1. 프로그램이 실행되면 메인 메소드에서 `Person p(관우, 30)`가 생성된다.
2. `changeRef` 메소드를 통해 원본 객체의 주소 값이 새로운 객체(Person)의 주소 값으로 바뀌게 된다.
3. 출력 시 `changeRef`를 통해 생성된 객체의 값이 출력된다.

그림을 보면 다음과 같다.
![Call by Reference](/assets/img/stack-frame/call-by-reference.png)


`chanceRef()` 메소드는 원본 객체의 참조 값을 이용한 참조 객체의 변경이 아닌 원본 객체의 참조 값 자체를 변경시켰다. 이러한 방식을 Call by Reference라 하며 Call by Value와는 다르게 동작함을 알 수 있다.

## 정리
---

Call by Value 
-  원본 변수의 값을 복사해서 메소드를 호출하는 방식(참조형의 경우 참조 주소 값 복사)이다.
-  변수의 내부 상태를 변경하면 기본형의 경우에는 원본 값에 영향이 아예 없고, 참조형의 경우에는 값이 바뀔 수 있다. <br /> => 이 경우를 구분하기 위해 Call by Value와 Call by Address라고 구분짓기도 한다.
- Call by Value 방식의 가장 중요한 점은 **값을 복사해서 전달** 한다는 것이다.

Call by Reference 
- 원본 객체의 참조 주소 값을 직접 전달하는 방식이다.
- 메소드의 내부 상태 변화나 원본 객체의 참조 자체를 변경해버릴 수 있다.

---               
> 🔍 [ 참고 자료 ] <br />
> 🔗 [https://inpa.tistory.com/entry/JAVA-☕-자바는-Call-by-reference-개념이-없다-❓](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94%EB%8A%94-Call-by-reference-%EA%B0%9C%EB%85%90%EC%9D%B4-%EC%97%86%EB%8B%A4-%E2%9D%93) <br />
> 🔗 [https://velog.io/@ahnick/Java-Call-by-Value-Call-by-Reference](https://velog.io/@ahnick/Java-Call-by-Value-Call-by-Reference)
{: .prompt-info }
