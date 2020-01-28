# 제너레이터

> 지금까지 You don't know JS를 읽어오면서, 나 스스로 제일 어려웠던 부분이다.

> 비동기 흐름 제어를 순차적 / 동기적 모습으로 나타내는 것 ES6에 등장한 제너레이터!!

- 선점형 멀티스레드 언어라면 일반적으로 두 문 사이의 특정 시점에 어떤 함수가 끼어들어 실행되게 할 수 있다.
- 하지만, 자바스크립트는 선점형`Context Change`, 멀티스레드 언어가 아니다.
  - 자바스크립트에서 이러한 부분이 가능해진다면, 끼어들기(`Interrupt`)를 협동적 형태로 나타낼 수 있다.

~~~Javascript
var x = 1;

function *foo() {
  x++;
  yield; // STOP
  console.log(x);
}
function bar() {
	x++;
}

///////
// 이터레이터 it을 선언하여 제너레이터를 제어한다.
var it = foo();

// 'foo'는 여기서 시작된다.!
it.next(); // yield에서 멈춘다.
x; // 2
bar();
x; // 3
it.next(); // 3
~~~

- `it = foo()`의 할당으로 *foo()제너레이터가 실행되는게 아니다. 실행을 제어한 Iterator만 마련한다.

- 이후 next()에서 yield에서 멈추게된다. 
- 그리고 bar()를 실행하여 x의 증가를 실행하고
- 출력을 한다.

> 즉, 함수의 진행상황에 중간에 멈춰!를 해두고 흐름을 제어하게 되었다.



### 인터리빙

> 자바스크립트 함수는 `완전-실행` 된다. 즉, 함수를 개별로 인터리빙하여 실행하는 건 불가능하다.
>
> 하지만, 제너레이터는 문 사이에서도 인터리빙 할 수 있다.

~~~javascript
var a = 1;
var b = 2;

function *foo() {
  a++;
  yield;
  b = b * a;
  a = (yield b) + 3;
}
function *bar() {
  b--;
  yield;
  a = (yield 8) + b;
  b = a * (yield 2);
}
~~~



 ## 값을 제너레이팅

> ES6는 `for~of` 루프를 지원하여 표준 이터레이터를 자동으로 기존 루프 구문 형태로 쓸 수 있다.

- for~of 루프는 매번 자동으로 next()를 호출하다가 done:true를 받으면 그 자리에서 멈춘다.
- 배열과 같은 자바스크립트 내장 자료 구조 대부분에는 기본 이터레이터가 장착되어있다.
  - 일반 객체에는 없다. Object.key()를 사용하라





## 제너레이터를 비동기적으로 순회

~~~javascript
function foo(x, y) {
  ajax(
 			"http://some.url/?x= " + x+ "?y=" + y,
    	function(err, data) {
        if(err) {
          // *main()으로 에러를 던진다.
          it.throw(err);
				}
        else {
          // 수신한 'data'로 '*main()'을 재개한다.
          it.next(data);
        }
     }
  )
}

function *main() {
  try {
    var text = yield foo(11, 31);
    console.log(text);
  } catch (err) {
    console.error(err);
  }
}
var it = main();
// 모두 시작하라.
it.next();
~~~

> 비동기성을 우리 두뇌가 허락하는 순차적/동기적 방향으로 표현할 수 없었던, 콜백의 단점을 거의 완벽하게
>
> 그것도, 한번에 보완한 솔루션이다.





## 제너레이터 + 프라미스

~~~javascript
function foo(x, y) {
  return request(
  	"http://some.url.1/?" + x + y;
  );
}

function *main() {
	try {
    var text = yield foo(11, 31);
    console.log(text);
  }
  catch(err) {
    console.error(err);
  } 
}

var it = main();

var p = it.next().value;

// p 프라미스가 귀결될 때까지 기다린다.
p.then(
	function(text) {
    it.next(text);
  },
  function(err) {
    it.throw(err);
  }
)
~~~



## 제너레이터 위임

> 한 이터레이터(1)를 실행할 때, 그 과정 속에 다른 이터레이터(2)가 있다면,
>
> (2)이터레이터를 만나면 (1)의 수행을 잠시 멈추고, (2) 이터레이터를 모두 훑고나면
>
> 그 제어권이 다시 (1)로 넘어온다.

~~~javascript
function *foo() {
  console.log("'*foo()'시작");
  yield 3;
  yield 4;
  console.log("'*foo()'끝");
}
function *bar() {
 	yield 1;
  yield 2;
  yield *foo(); // yield 위임
  yield 5;
}

var it = bar();

it.next().value; // 1
it.next().value; // 2
it.next().value; // 'foo()' 시작
								 // 3
it.next().value; // 4
it.next().value; // 'foo()' 끝
								 // 5
~~~



> 중간의 수 많은 위임에 관한 내용을 생략했다. 이 파트는 책을 보면서 공부하는것이 좋겠다.



## 정리

- 제너레이터는 ES6부터 도입된 새로운 유형의 함수. 완전-실행이 아닌, 멈추고 그 지점에서 시작할 수 있다.
- 멈춤/재개가 번갈아 일어나므로 선점적이다라기 보다 협동적인 툴이다.
- 제너레이터는 비동기 코디의 순차/동기/중단적 패턴을 유지하게 한다.







































