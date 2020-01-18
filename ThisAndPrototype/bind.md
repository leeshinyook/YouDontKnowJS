# bind

> this 바인딩의 개념을 이해하려면 먼저 호출부, 즉 함수 호출(선언이 아니다.) 코드부터 확인하고 this가
>
> 가리키는 것이 무엇인지 찾아봐야 한다.

​		호출 스택부터 생각하라!

### 기본 바인딩

> 단독 함수 실행에 관한 규칙,
>
> 나머지 규칙에 해당하지 않을 경우 적용되는 this의 기본 규칙이다



```javascript
function foo() {
  console.log(this.a);
}
var a = 2;
foo(); // 2
```

- this.a 는 전역객체 a를 가리킨다.
- 기본바인딩이 적용되여, this는 전역객체를 가리킨다.
  - 하지만, 엄격모드`use strict` 에서는 전역객체가 기본바인딩 대상에서 제외된다.
  - 즉, this 바인딩 규칙은 오로지 호출부에 의해 좌우되지만, 비엄격모드에서 전역 객체만이 기본 바인딩의 유일한 대상이라는 것.

### 암시적 바인딩

> 호출부에 콘텍스트 객체, 즉 객체의 소유 / 포함여부를 확인한다.
>
> 함수전달하여 객체로 실행한다.



```javascript
function f() {
  console.log(this.a);
}
function g() {
  fn();
}
var a = 1;
var b = {
  n : 3,
  f : f
}
b.f(); // 3 --> 암시적 바인딩

var c = o.f();
c(); // 1 ---> 함수 레퍼런스 / 별명
```



### 암시적 소실

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a : 2,
  foo : foo
}
var bar = obj.foo; // 함수 레퍼런스
var a = "엥, 전역이네?"; // 'a' 역시 전역 객체의 프로퍼티
bar(); // "엥, 전역이네?"
```

- bar는 obj의 foo를 참조하는 변수처럼 보이지만, 실은 foo를 직접 가리키는 또 다른 레퍼런스다. 
- 또한, 호출부에서 그냥 평범하게 bar()를 호출하므로, 기본 바인딩이 적용된다.



### 명시적 바인딩

> 암시적 바인딩에서는, 함수 레퍼런스를 객체에 넣기 위해 객체 자신을 변형해야 했고, 함수 레퍼런스 프로퍼티를 이용해 this를 암시적(간접적)으로 바인딩 했다.

> 반대로, 명시적 바인딩은 어떤 객체를 this 바인딩에 이용하겠다는 강력한 의지를 나타낸다.

- call()

  ```javascript
  function foo() {
    console.log(this.a);
  }
  var obj = {
    a: 2
  };
  
  foo.call(obj); // 2
  ```

  - this에 바인딩 할 객체를 첫째 인자로 받아 함수 호출시 이 객체를 this로 세팅한다.

- 하드바인딩 : 재사용 가능한 헬퍼함수

  ```javascript
  function bind(fn, obj) {
    return function() {
      return fn.apply(obj, arguments);
    }
  }
  ```

- new 바인딩

  > 클래스 지향 / 생성자
  >
  > 생성자 : new 연산자가 있을 때 호출되는 일반 함수.

  ***순서***

  1. 새 객체가 만들어진다.
  2. 새로 생성된 객체의 [[Prototype]]이 연결된다.
  3. 새로 생성된 객체는 해당 함수 호출 시 this로 바인딩 된다.
  4. 이 함수가 자신의 또 다른 객체를 반환하지 않는 한,  new로 새로운 객체를 반환한다

  ```javascript
  function f(p1, p2) {
  	this.value = p1 + p2;
  }
  var g = f.bind(null, "p2");
  var h = new g("p1");
  
  console.log(h.value); // p1p2
  
  // 함수 인자를 전부 또는 일부만 미리 세팅한다.
  // bind() 함수는 this바인딩 이후 전달된 인자를 원 함수의 기본인자로 고정한다.
  // currying
  ```

### 바인딩 예외

> call, apply, bind에 null이나 undefined를 넘기면, 기본 바인딩 규칙 적용



### 화살표 함수

```javascript
function foo() {
  return (a) => {
    // 여기서 this는 어휘적으로 foo()에서 상속된다.
    console.log(a);
  }
}

var obj1 = { 
	a: 2
}
var obj2 = {
  a: 3
}
var bar = foo.call(obj1);
bar.call(obj2); // 2, 3이 아니다.
```

> foo()는 obj1에 this가 바인딩 되므로 bar(반환된 화살표 함수를 가리키는 변수)의 this 역시, obj1로 바인딩 된다.

- 필요하면 bind()까지 포함하여 완전한 this 스타일의 코드를 구사하라
- ES6 화살표 함수는 표준 바인딩 규칙을 무시하고, 렉시컬 스코프로 this를 바인딩 한다.





































































