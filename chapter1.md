# 2주차

### 1\) 객체 지향 개념

* 클래스
* 객체, 인스턴스

  ```
  Animal cat = new Animal();
  ```

* 필드

* 생성자

* 메소드

### 2\) java.lang.ref package

##### : 가비지 컬렉터와의 제한부의 대화를 지원하는, 참조 객체 클래스를 제공

* PhantomReference&lt;T&gt; : 팬텀 참조 객체

* SoftReference&lt;T&gt; : 메모리 요구에 응해 가비지 컬렉터의 판단으로 클리어 되는 소프트 참조 객체

* WeakReference&lt;T&gt; : 약참조 객체



#### WeakReference

 : 객체가 사용중이기는 하지만, 더 이상 사용하지 않을 것 같은 경우 GC를 실행하면 메모리에서 정리됨

#### SoftReference

 : GC가 실행 후 JVM에서 메모리 공간이 없으면 SoftReference 인스턴스를 GC 대상으로 함 \(as late as possible\)

#### PhantomReference

 : softReference, weakReference 보다 엄청 약함. GC가 돌기 전\(즉, GC 대상이라고 결정할 때, finalize\(\) 호출 후\) 메모리에서 정리됨. GC 되기 전에 cleanup 해야 할 때 사용됨. 내부적으로는 유지하고 있지만, 객체를 다시 꺼내오면 null이 됨.



