# 호이스팅

> 선언문이 대입문보다 먼저다.  
> 모든 선언문은 어디서 나타나든 실행 전 먼저 처리된다.

    선언문을 끌어올리는 동작을 `호이스팅(Hoisting)`이라고 한다.

```javascript
console.log(a); // undefined
var a = 2;

// 위의 예제는 아래처럼 동작한다.
// 선언문과 대입문을 쪼개어
var a; // 선언문을 호이스팅
console.log(a); // 당연히, undefined
a = 2; // 대입문.
```

    위 처럼 선언문은 컴파일레이션단계에서 진행된다.

## 호이스팅은 스코프별로 작동한다.

```javascript
foo();

function foo() {
  console.log(a); // undefined
  var a = 2;
}
// 아래처럼 동작한다.
function foo() {
  var a;
  console.log(a);
  a = 2;
}
foo();
```

## 함수가 먼저다.

```javascript
f(); // function2
var f;

function f() {
  console.log("function");
}

f = function() {
  console.log("var");
};

function f() {
  console.log("function2");
}
```

    즉, 선언이 대입 우선이다.

> 블록 내 함수 선언은 자양하는 것이 가장 좋다.

> 많은 중복 변수 선언문은 사실상 무시되지만, 중복 함수 선언문은 앞선다.

> 선언문 자체는 옮겨지지만, 모든 대입문은 끌어올려 지지 않는다.  
> 중복선언을 조심하라
