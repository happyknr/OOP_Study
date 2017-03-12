# 6주차

### 팩토리 패턴

* 인스턴스를 만드는 방법을 상위 클래스 측에서 결정하지만 구체적인 클래스 이름까지는 결정하지 않음. 구체적인 내용은 모두 하위 클래스에서 수행함 → 인스턴스 생성을 위한 골격\(framework\)과 실제의 인스턴스 생성의 클래스를 분리해서 생각할 수 있음

![](/assets/factory_pattern_uml)

#### Product\(제품\)의 역할

* framework에 포함됨
* 이 패턴에서 생성되는 인스턴스가 가져야 할 인터페이스\(API\)를 결정하는 것. 추상클래스로 작성됨.
* 구체적인 내용은 하위 클래스인 ConcreteProduct에서 작성됨.

#### Creator\(생성자\)의 역할

* Product 역할을 생성하는 추상클래스
* 구체적인 내용은 하위 클래스인 ConcreteCreator에서 작성됨
* Creator가 가지고 있는 정보는 Product 역할과 인스턴스 생성 메소드를 호출하면 Product가 생성된다는 것 뿐임
* **new를 사용해서 실제의 인스턴스를 생성하는 대신에, 인스턴스 생성을 위한 메소드를 호출해서 구체적인 클래스 이름에 의한 속박에서 상위 클래스를 자유롭게 만듦!**

#### ConcreteProduct\(구체적인 제품\)의 역할

* 구체적인 제품을 결정

#### ConcreteCreator\(구체적인 작성자\)의 역할

* 구체적인 제품을 만드는 클래스를 결정

### 인스턴스 생성 - 메소드의 구현 방법

* Creator 클래스의 factoryMethod 메소드는 추상 메소드이며, 하위 클래스에서 구현하게 됨. 

#### 1\) 추상 메소드

* factoryMethod 메소드를 기술할 때 추상메소드로 기술함
* 추상메소드로 기술하면 하위 클래스에서 반드시 이 메소드를 구현해야 함. 구현되어 있지 않을 경우 컴파일할 때 검출됨!

```
abstract class Factory {
    public abstract Product createProduct(String name);
    ...
}
```



