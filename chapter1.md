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

* WeakReference&lt;T&gt; : 약참조 객체

* SoftReference&lt;T&gt; : 메모리 요구에 응해 가비지 컬렉터의 판단으로 클리어 되는 소프트 참조 객체

* PhantomReference&lt;T&gt; : 팬텀 참조 객체

#### WeakReference

 - 

#### SoftReference

* 힙 메모리 상의 특정 객체를 참조 \(as late as possible\)
* JVM이 관리하는 메모리의 공간이 부족할 경우\(OutOfMemoryError\), SoftReference 참조만 갖고 있는 객체는 GC의 수거 대상이 됨
* Cache 구현 등됨에 사용

#### PhantomReference

: softReference, weakReference 보다 엄청 약함. GC가 돌기 전\(즉, GC 대상이라고 결정할 때, finalize\(\) 호출 후\) 메모리에서 정리됨. GC 되기 전에 cleanup 해야 할 때 사용됨. 내부적으로는 유지하고 있지만, 객체를 다시 꺼내오면 null이 됨.

* 아주 특수한 경우에 사용됨

###### 참고

###### - [http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110095553797](http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110095553797)



