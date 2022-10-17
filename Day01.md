

## 면접을 위한 CS 스터디 #1


### 💡 라이브러리 ?

공통으로 사용될 수 있는 특정 기능들을 모듈화한 것

폴더명, 파일명 등에 대한 규칙이 없고 프레임워크에 비해 자유로움

무언가를 자를 때 ‘도구’인 ‘가위’를 사용해서 ‘내가’ 직접 컨트롤하는데, 라이브러리가 이와 비슷하다.

### 💡 프레임워크 ?

공통으로 사용될 수 있는 특정 기능들을 모듈화한 것

폴더명, 파일명 등에 대한 규칙이 있고 라이브러리에 비해 더 엄격함

다른 곳으로 이동할 때 ‘도구’인 비행기를 타고 이동하지만 ‘비행기’가 컨트롤하고 나는 가만히 있어야한다.프레임워크는 이와 비슷하다

### 💡 디자인 패턴이란?

프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 ‘규약’ 형태로 만들어 놓은 것

1.1 디자인 패턴
디자인 패턴이란 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 '규약'형태로 만들어 놓은 것을 의미합니다.



1.1.1 싱글톤 패턴

싱글톤 패턴은 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴입니다. 보통 데이터베이스 연결 모듈에 많이 사용합니다.
객체를 생성할 때마다 메모리 영역을 할당받아야 하지만, 하나의 인스턴스만 생성했기 때문에 메모리 낭비를 방지 할 수 있고, 다른 클래스의 인스턴스들이 데이터를 공유할 수 있는 장점이 있습니다.
자바스크립트에서는 리터럴{} 또는 new Object로 객체를 생성하게 되면 다른 어떤 객체와도 같지 않기 때문에 자체만으로 싱글톤 패턴을 구현할 수 있습니다.
생성자가 여러번 호출되도, 실제로 생성되는 객체는 하나이며 최초로 생성된 이후에 호출된 생성자는 이미 생성한 객체를 반환시키도록 만드는 것이다
(java에서는 생성자를 private으로 선언해 다른 곳에서 생성하지 못하도록 만들고, getInstance() 메소드를 통해 받아서 사용하도록 구현한다)


```javascript

class Singleton {
constructor() {
if (!Singleton.instance) {
Singleton.instance = this
}
return Singleton.instance
}
getInstance() {
return this
}
}
const a = new Singleton()
const b = new Singleton()
console.log(a === b) // true
java

class Singleton {
private static class singleInstanceHolder {
private static final Singleton INSTANCE = new Singleton();
}
public static Singleton getInstance() {
return singleInstanceHolder.INSTANCE;
}
}

public class HelloWorld{
public static void main(String []args){
Singleton a = Singleton.getInstance();
Singleton b = Singleton.getInstance();
System.out.println(a.hashCode());
System.out.println(b.hashCode());  
if (a == b){
System.out.println(true);
}
}
}
/*
705927765
705927765
true
1. 클래스안에 클래스(Holder), static이며 중첩된 클래스인 singleInstanceHolder를
   기반으로 객체를 선언했기 때문에 한 번만 로드되므로 싱글톤 클래스의 인스턴스는 애플리케이션 당 하나만 존재하며
   클래스가 두 번 로드되지 않기 때문에 두 스레드가 동일한 JVM에서 2개의 인스턴스를 생성할 수 없습니다.
   그렇기 때문에 동기화, 즉 synchronized를 신경쓰지 않아도 됩니다.
2. final 키워드를 통해서 read only 즉, 다시 값이 할당되지 않도록 했습니다.
3. 중첩클래스 Holder로 만들었기 때문에 싱글톤 클래스가 로드될 때 클래스가 메모리에 로드되지 않고
   어떠한 모듈에서 getInstance()메서드가 호출할 때 싱글톤 객체를 최초로 생성 및 리턴하게 됩니다.
   */
```

싱글톤 패턴은 Node.js에서 MongoDB 데이터베이스를 연결할 때 mongoose모듈을 사용.
싱글톤 패턴은 Node.js에서 MySQL 데이터베이스를 연결할 때도 사용.

싱글톤 패턴은 각 테스트마다 '독립적인' 인스턴스를 만들기가 어려워 TDD(Test Driven Development)를 할 때 걸림돌이 되는 단점이 있습니다. 또한 사용하기가 쉽고 실용적이지만 모듈간의 결합을 강하게 만들 수 있다는 단점이 있습니다.이 때 의존성 주입을 통해 모듈간의 결합을 조금 더 느슨하게 만들어 해결할 수 있습니다.

### 의존성 주입

의존성이란 종속성이라고도 하며 A가 B에 의존성이 있다는 것은 B의 변경 사항에 대해 A 또한 변해야 한다는 것을 의미합니다. 메인모듈이 '직접'다른 하위 모듈에 대한 의존성을 주기보다는 중간에 의존성 주입자가 이 부분을 가로채 메인 모듈이 '간접'적으로 의존성을 주입하는 방식입니다.
![](https://velog.velcdn.com/images/guddyd6761/post/da1749be-13c9-4a42-9a23-20a2825b64e7/image.png)
```
장점: 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽고 마이그레이션하기도 수월하고, 애플리케이션 의존성 방향이 일관되어지고 모듈간의 관계들이 조금 더 명확해집니다.
단점: 클래스 수가 늘어나 복잡성이 증가될 수 있으며 약간의 린타임 패널티가 생깁니다.
```
### 1.1.2 팩토리 패턴

팩토리 패턴은 객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고,
하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정하는 패턴입니다.
상위 클래스와 하위 클래스가 분리되기 때문에 더 많은 유연성을 갖게 되고,
객체 생성 로직이 따로 떼어져 있기 때문에 유지 보수성도 증가됩니다.

``` javascript

class Latte {
constructor() {
this.name = "latte"
}
}
class Espresso {
constructor() {
this.name = "Espresso"
}
}

class LatteFactory {
static createCoffee() {
return new Latte()
}
}
class EspressoFactory {
static createCoffee() {
return new Espresso()
}
}
const factoryList = { LatteFactory, EspressoFactory }

class CoffeeFactory {
static createCoffee(type) {
const factory = factoryList[type]
return factory.createCoffee()
}
}   
const main = () => {
// 라떼 커피를 주문한다.  
const coffee = CoffeeFactory.createCoffee("LatteFactory")  
// 커피 이름을 부른다.  
console.log(coffee.name) // latte
}
main()
/*
CoffeeFactory라는 상위 클래스가 중요한 뼈대를 결정하고 하위 클래스인 LatteFactory
가 구체적인 내용을 결정하고 있습니다.
참고로 이는 의존성 주입이라고도 볼 수 있습니다.
CoffeeFactory에서 LatteFactory의 인스턴스를 생성하는 것이 아닌
LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있기 때문이죠.
또한, CoffeeFactory를 보면 static으로 createCoffee() 정적 메서드를 정의한 것을 알
수 있는데, 정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능하여 메모리를 절약
할 수 있고 개별 인스턴스에 묶이지 않으며 클래스 내의 함수를 정의할 수 있는 장점이 있
습니다.
*/
```


```java

abstract class Coffee {
public abstract int getPrice();

    @Override
    public String toString(){
        return "Hi this coffee is "+ this.getPrice();
    }
}

class CoffeeFactory {
public static Coffee getCoffee(String type, int price){
if("Latte".equalsIgnoreCase(type)) return new Latte(price);
else if("Americano".equalsIgnoreCase(type)) return new Americano(price);
else{
return new DefaultCoffee();
}
}
}
class DefaultCoffee extends Coffee {
private int price;

    public DefaultCoffee() {
        this.price = -1;
    }

    @Override
    public int getPrice() {
        return this.price;
    }
}
class Latte extends Coffee {
private int price;

    public Latte(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
class Americano extends Coffee {
private int price;

    public Americano(int price){
        this.price=price; 
    }
    @Override
    public int getPrice() {
        return this.price;
    } 
}
public class HelloWorld{
public static void main(String []args){
Coffee latte = CoffeeFactory.getCoffee("Latte", 4000);
Coffee ame = CoffeeFactory.getCoffee("Americano",3000);
System.out.println("Factory latte ::"+latte);
System.out.println("Factory ame ::"+ame);
}
}
/*
Factory latte ::Hi this coffee is 4000
Factory ame ::Hi this coffee is 3000
*/
```


### ✔ 전략 패턴

전략 패턴(Strategy pattern)은 정책 패턴(policy pattern)이라고도 함.

객체의 행위를 바꾸고 싶은 경우 ‘직접’ 수정하지 않고 전략이라 부르는 ‘캡슐화한 알고리즘’을 컨텍스트 안에서 바꿔주며 상호 교체가 가능하게 만드는 패턴

![](https://velog.velcdn.com/images/guddyd6761/post/31bf79d7-80c8-4949-af11-0ef7a64c2a03/image.png)

패턴코드 예제(접기형식으로 넣을 예정)

### 💡 컨텍스트란?

프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미하여 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.

### Passport의 전략패턴

전략패턴을 활용한 라이브러리는 passport가 있다

passport 는 Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리로,여러가지 ‘전략’을 기반으로 인증할 수 있게 한다.

### ✔ 옵저버 패턴

주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴

![](https://velog.velcdn.com/images/guddyd6761/post/c3d79a05-b927-4873-ac59-822006d0d997/image.png)

(객체와 주체가 분리되어 있는 옵저버 패턴)

주체 : 객체의 상태 변화를 보고 있는 관찰자

옵저버 : 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 ‘추가 변화 사항’이 생기는 객체들을 의미

![](https://velog.velcdn.com/images/guddyd6761/post/7355e407-ac08-4629-9f10-d2987e5d0ece/image.png)
(객체와 주체가 합쳐진 옵저버 패턴)

위 그림들처럼 주체 객체를 따로 두지 않고 상태가 변경되는 객체를 중심으로 구축하기도 한다

옵저버 패턴을 활용한 서비스로는 트위터가 있습니다.

![](https://velog.velcdn.com/images/guddyd6761/post/34fac65c-78b1-4ea1-87ee-92431372b091/image.png)

팔로우한 사람이 게시글을 올리면, 옵저버들에게 알림이 감

![](https://velog.velcdn.com/images/guddyd6761/post/2c756d8d-10d2-4c33-89b9-6319179963ea/image.png)

옵저버 패턴은 주로 이벤트 기반 시스템에 사용하며 MVC(Model-View-Controller)패턴에 사용된다.

모델에 변경사항→update()메서드로  뷰에 알림 → 컨트롤러 등 작동

자바 : 상속과 구현

implements 등 자바의 상속과 구현의 특징과 차이

**상속  ⇒ 일반클래스**

상속(extends)은 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용하며 자식 클래스에서 추가 및 확장을 할 수 있는 것을 의미. 재사용성,중복성의 최소화가 이뤄짐

**구현 ⇒ abstract 클래스를 기반으로 구현**

구현(implements)은 부모 인터페이스(interface)를 자식 클래스에서 재정의하여 구현하는 것

상속과는 달리 반드시 부모 클래스의 메서드를 재정의해 구현해야한다

**자바스크립트에서의 옵저버 패턴**

자바스크립트에서의 옵저버 패턴은 프록시 객체를 통해 구현

프록시(proxy) 객체는 어떠한 대상의 기본적인 동작(속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체를 뜻하며, 자바스크립트에서 프록시 객체는 두 개의 매개변수를 가진다.

- target : 프록시할 대상
- handler : 프록시 객체의 target 동작을 가로채서 정의할 동작들이 정해져 있는 함수