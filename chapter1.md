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

* WeakReference에 의해 참조된 객체는 GC가 발생하기 전까지는 객체에 대한 참조를 유지하지만 **GC가 발생하면 무조건 수거됨 \(GC의 실행 주기와 일치\)**
* 짧은 시간동안 자주 쓰일 수 있는 객체를 Cache 할 때 유용하게 이용됨

```
...    
WeakReference<String> wr;    
public String getFileContent(String filename) {
    String fileContent = (wr != null) ? wr.get() : null;  // WeakReference에 의해 파일 내용이 보존되어 있는지 체크.
    if (fileContent == null) {
        fileContent = readFileToString(filename);
        wr = new WeakReference<String>(fileContent); // 글 내용을 WeakReference에 저장.
    }
    return fileContent;
}
...
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
public class PhantomReference<T> extends Reference<T> {
    ...
    public T get() {
        return null;    
    }
}
```

###### 참고

###### - [http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110095553797](http://blog.naver.com/PostView.nhn?blogId=huewu&logNo=110095553797)

###### - [http://egloos.zum.com/skyswim42/v/3677479](http://egloos.zum.com/skyswim42/v/3677479)



