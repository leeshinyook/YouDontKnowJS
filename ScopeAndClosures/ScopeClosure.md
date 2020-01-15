# 스코프 클로저

    정의 : 클로저는 함수를 렉시컬 스코프 밖에서 호출해도 함수는 자신의 렉시컬 스코프를 기억하고 접근할 수 있는 특성을 의미한다.

- 클로저는 렉시컬 스코프에 의존해 코드를 작성한 결과로 발생한다.
- 모든 코드에서 클로저는 생성되고 사용된다.

## 클로저란?

- 렉시컬 스코프 검색 규칙을 통해 바깥 스코프의 변수에 접근할 수 있다.
- 여전히 `사용중` 이므로 해제되지 않는다.
  - 예를 들어, 어떤 함수가 실행된 뒤, 자바스크립트엔진이 `가비지 콜렉터(Garbage Collector)`를 고용해 더는 사용하지 않는 메모리를 해제시킨다.
  - 하지만, 클로저는 이를 내버려두지 않는다.
- 렉시컬 스코프 클로저를 가지고, 나중에 참조할 수 있도록 스코프를 살려둔다.(즉, 여전히 해당 스코프의 참조를 가진다.)
- 어떤 방식이든 함수를 값으로 넘겨 다른 위치에서 호출되는 행위
- 콜백함수를 넘기면 클로저를 사용할 준비가 된 것이다.
- `IIFE(즉시 실행 함수)`: 스코프를 생성하지만, 함수는 자신의 렉시컬 스코프 밖에서 실행된 것이 아니다. 이 자체는 클로저의 사례는 아니지만, 확실히 클로저를 생성하고 사용할 수 있는 스코프를 만드는 가장 흔한 도구이다.

  가장 대표적인 스코프의 예

```javascript
function foo() {
  var a = 2;
  function bar() {
    console.log(a);
  }
  return bar;
}
var baz = foo();
baz(); // 2 !! <- Closure was obeserved!!
```

## 모듈

```javascript
function coolModule() {
  var something = "cool";
  var another = [1, 2, 3];

  function doSomething() {
    console.log(somthing);
  }
  function doAnother() {
    console.log(another.join(" ! "));
  }
  return {
    doSomething,
    doAnother
  };
}
var foo = coolModule();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

- 모듈패턴의 조건
  1. 하나의 최외곽 함수가 존재하고, 이 함수가 최소 한 번은 호출되어야 한다.(호출할 때마다 새로운 모듈 인스턴스가 생성된다.)
  2. 최외곽 함수는 최소 한 번은 하나의 내부 함수를 반환해야 한다. 그래야 해당 내부 함수가 비공개 스코프에 대한 클로저를 가져 비공개 상태에 접근하고 수정할 수 있다.

## 싱글톤(Singleton)만 생성하는 모듈 : 모듈 + IIFE

```javascript
var singleton = (function Module() {
  var v = "var";

  function _get() {
    return v;
  }
  function _set(value) {
    v = value;
  }
  var publicAPI = {
    get: _get,
    set: _set
  };
  return publicAPI;
})();

singleton.get();
singleton.set();
```

## 정리

1. 클로저는 함수를 렉시컬 스코프밖에서 호출해도 함수는 자신의 렉시컬 스코프를 기억하고 접근할 수 있는 특성을 의마한다.
2. 클로저는 다양한 형태의 모듈 패턴을 가능하게 하는 매우 효과적인 도구이다.
