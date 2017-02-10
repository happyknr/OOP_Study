# 2주차

### 1\) 객체 지향 개념

#### 클래스

* 객체지향 프로그래밍\(Object-Oriented Programming\)에서 로직과 데이터를 묶어둔 하나의 단위
* 현실 객체를 명세\(표현\)하기 위한 일종의 틀
* 특징

  * 각자 따로 보관하고 싶어하는 데이터 혹은 속성들을 정의\(attribute\)

  * 데이터에 관계없이 제공되는 같은 기능이나 로직\(method\)

```
public class TestClass {
    // 클래스 내용
}
```

#### 객체

* 데이터\(특성\)와 그 데이터에 관련되는 동작\(절차, 방법, 기능-로직\)이 결합된 것
* new 연산자를 통해 객체를 저장할 메모리를 할당함

> 객체 생성 과정
>
> 1. new 연산자가 객체\(Animal\)가 저장될 메모리 공간 할당
>
> 2. 생성자가 객체\(Animal\)를 초기화 후 종료
>
> 3. new 연산자가 새로 생성된 객체의 주소\(reference\)를 변수\(cat\)에 저장
>
> 4. 변수\(cat\)를 통해 해당 객체에 접근 가능

```
Animal cat = new Animal();
```

#### 필드\(field\)

* 클래스 안에서 선언되는 멤버 변수
* 단, main\(\) 메서드 안에서 정의한 변수는 필드라고 하지 않음

```
class A {
    int a;
    String b;
}
```

#### 생성자

* 객체 생성시 객체를 초기화하고 heap에 저장하는 메소드
* 생성자는 객체가 생성될 때 실행됨
* 생성자는 클래스명과 동일하며 종료할 때 값을 반환하지 않음\(즉, return 값이 존재하지 않음\)

> 생성자 호출 규칙
>
> * 생성자 내에서 다른 생성자를 호출할 때에는 항상 생성자 코드의 첫번째에서 실행되어야 함
> * 한 생성자 내에서 다른 생성자를 호출 할 때에는 반드시 this\(\)로만 호출할 수 있음
> * 생성자는 오직 다른 생성자만이 호출할 수 있음
> * this\(\)를 일반 메소드로 명시할 수 없음

```
class Image {
    Image() {
        System.out.println("Image() called");
    }
}

// Image img = new Image(); 실행 시 생성자가 실행됨
// 출력 결과
Image called
```

#### 메소드\(method\)

* 어떤 작업을 수행하기 위한 명령문의 집합
* 주로 어떤 값을 입력받아 처리하고 그 결과를 돌려줌. 경우에 따라 입력받는 값이 없을 수도 있고 결과를 반환하지 않을 수도 있음
* 반복적으로 사용되는 코드를 줄이기 위해 사용됨 ! -&gt; 코드의 양도 줄이고 관리하기 편하므로 유지보수 편리

### 2\) java.lang.ref package

##### : 가비지 컬렉터와의 제한부의 대화를 지원하는, 참조 객체 클래스를 제공

* WeakReference&lt;T&gt; : 약참조 객체

* SoftReference&lt;T&gt; : 메모리 요구에 응해 가비지 컬렉터의 판단으로 클리어 되는 소프트 참조 객체

* PhantomReference&lt;T&gt; : 팬텀 참조 객체

#### WeakReference

* WeakReference에 의해 참조된 객체는 GC가 발생하기 전까지는 객체에 대한 참조를 유지하지만 **GC가 발생하면 무조건 수거됨 \(GC의 실행 주기와 일치\)**
* 짧은 시간동안 자주 쓰일 수 있는 객체를 Cache 할 때 유용하게 이용됨

```
import java.lang.ref.WeakReference;

public class ReferenceTest { 
    public static void main(String[] args) throws InterruptedException { 
        Student s1 = new Student(1); 
        System.out.println("1: " + s1); 
        WeakReference<Student> ws = new WeakReference<Student>(s1); 
        System.out.println("2: " + ws.get()); 
        s1 = null; 
        System.gc(); 
        Thread.sleep(1000); 
        System.out.println("3: " + ws.get()); 
    } 
}

class Student { 
    int id; 
    public Student(int id) { 
        this.id = id; 
    } 
    public String toString() { 
        return "[id=" + id + "]"; 
    } 
} 

// 출력 결과
1: [id=1] 
2: [id=1] 
3: null 
```

#### SoftReference

* 힙 메모리 상의 특정 객체를 참조 \(as late as possible\)

* JVM이 관리하는 **메모리의 공간이 부족할 경우\(OutOfMemoryError\), SoftReference 참조만 갖고 있는 객체는 GC의 수거 대상이 됨**

* Cache 구현 등됨에 사용

```
package org.apache.commons.pool.impl;

import java.lang.ref.SoftReference; 
import java.lang.ref.ReferenceQueue; 
import java.lang.ref.Reference; 
import java.util.ArrayList; 
import java.util.Iterator; 
import java.util.List; 
import java.util.NoSuchElementException;

import org.apache.commons.pool.BaseObjectPool; 
import org.apache.commons.pool.ObjectPool; 
import org.apache.commons.pool.PoolableObjectFactory; 
import org.apache.commons.pool.PoolUtils;


public class SoftReferenceObjectPool extends BaseObjectPool implements ObjectPool { 
….



public synchronized void addObject() throws Exception { 
        assertOpen(); 
        if (_factory == null) { 
            throw new IllegalStateException("Cannot add objects without a factory."); 
        } 
        Object obj = _factory.makeObject();

        boolean success = true; 
        if(!_factory.validateObject(obj)) { 
            success = false; 
        } else { 
            _factory.passivateObject(obj); 
        }

        boolean shouldDestroy = !success; 
        if(success) { 
           _pool.add(new SoftReference(obj, refQueue)); 
            notifyAll(); // _numActive has changed 
        }

        if(shouldDestroy) { 
            try { 
                _factory.destroyObject(obj); 
            } catch(Exception e) { 
                // ignored 
            } 
        } 
    } 
```

#### PhantomReference

* finalize\(\)가 호출된 이후 그 객체와 관련된 사후 작업을 수행할 필요가 있을 때 사용됨
* **다시는 객체에 참조할 수 없다**는 특이점이 있음! phantomReference는 참조되는 객체를 내부적으로 유지하고 있지만 그 객체를 다시 꺼내오는 get\(\) method에서 **무조건 null을 반환**하도록 되어 있는데 이는 phantomReference가 참조하는 객체는 다시 활용될 수 없고 GC가 수행되면서 그 객체가 사라질 때 관련된 사후 작업만을 할 수 있다는 의미 ! \(null이 나와 객체가 사라졌다고 생각할 수 있으나 실제로는 사라진 것이 아님\)

* 때문에 PhantomReference는 명시적으로 객체를 소멸시켜주는** clear\(\)**를 반.드.시. 호출해야 함

* 아주 특수한 경우에 사용됨

```
package snippet;

import java.lang.ref.PhantomReference; 
import java.lang.ref.Reference; 
import java.lang.ref.ReferenceQueue; 
import java.util.ArrayList;

public class ReferenceTest { 
    public static void main(String[] args) throws InterruptedException { 
        test1(); 
        test2(); 
    } 

    public static void test1() { 
        ReferenceQueue<Foo> queue = new ReferenceQueue<Foo>(); 
        ArrayList<PhantomReference<Foo>> list = new ArrayList<PhantomReference<Foo>>();

        for (int i = 0; i < 10; i++) { 
            Foo o = new Foo(Integer.toOctalString(i)); 
            list.add(new PhantomReference<Foo>(o, queue)); 
        }

        // make sure the garbage collector does it’s magic 
        System.gc();

        // lets see what we’ve got 
        Reference<? extends Foo> referenceFromQueue;

        for (PhantomReference<Foo> reference : list) { 
            System.out.println("x: " + reference.isEnqueued()); 
        } 
        while ((referenceFromQueue = queue.poll()) != null) { 
            System.out.println("y: " + referenceFromQueue.get()); 
            referenceFromQueue.clear(); 
        } 
    }

    public static void test2() { 
        // initialize 
        ReferenceQueue<Foo> queue = new ReferenceQueue<Foo>(); 
        ArrayList< FinalizeStuff<Foo>> list = new ArrayList<FinalizeStuff<Foo>>(); 
        ArrayList<Foo> foobar = new ArrayList<Foo>(); 

        for ( int i = 0; i < 10; i++) { 
            Foo o = new Foo( Integer.toOctalString( i)); 
            foobar.add(o); 
            list.add(new FinalizeStuff<Foo>(o, queue)); 
        } 

        // release all references to Foo and make sure the garbage collector does it’s magic 
        foobar = null; 
        System.gc(); 

        // should be enqueued 
        Reference<? extends Foo> referenceFromQueue; 
        for ( PhantomReference<Foo> reference : list) { 
            System.out.println(reference.isEnqueued()); 
        } 

        // now we can call bar to do what ever it is we need done 
        while ( (referenceFromQueue = queue.poll()) != null) { 
            ((FinalizeStuff)referenceFromQueue).bar(); 
            referenceFromQueue.clear(); 
        } 
    } 
}


class Foo { 
    private String bar;

    public Foo(String bar) { 
        this.bar = bar; 
    }

    public String get() { 
        return bar; 
    } 
}


class FinalizeStuff<Foo> extends PhantomReference<Foo> { 
    public FinalizeStuff(Foo foo, ReferenceQueue<? super Foo> queue) { 
        super(foo, queue); 
    } 

    public void bar() { 
        System.out.println("foobar is finalizing resources"); 
    } 
}

// 출력 결과
x: true 
x: true 
x: true 
x: true 
x: true 
x: true 
x: true 
x: true 
x: true 
x: true 
y: null 
y: null 
y: null 
y: null 
y: null 
y: null 
y: null 
y: null 
y: null 
y: null 
true 
true 
true 
true 
true 
true 
true 
true 
true 
true 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
foobar is finalizing resources 
```

###### 참고

###### - [http://nukestorm.tistory.com/](http://nukestorm.tistory.com/)

###### - [http://rank01.tistory.com](http://rank01.tistory.com)

###### - [http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110095553797](http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110095553797)

###### - [http://egloos.zum.com/skyswim42/v/3677479](http://egloos.zum.com/skyswim42/v/3677479)

###### - [http://knight76.tistory.com/entry/SofreReference-WeakReference-PhantomReference](http://knight76.tistory.com/entry/SofreReference-WeakReference-PhantomReference)



