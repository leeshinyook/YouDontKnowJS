# 객체



### 구문

- 선언적 (리터럴 형식)
- 생성자 형식

```JAVASCRIPT
// 리터럴 형식
var myObj = {
	key : value
  //..
}

// 생성자 형식
var myObj = new Object();
myObj.key = value;		
```

- 리터럴 형식은 한 번의 선언으로 다수의 키 / 값 쌍을 프로퍼티로 추가할 수 있다.
- 생성자 형식은  한 번에 한 프로퍼티만 추가할 수 있다.

 

### 타입

> 단순원시타입 `stirng, number, null, boolean, undefined` 는 객체가 아니다.

- 자바스크립트의 모든 것이 객체라는 말은 오류다!)
  - `typeof null === "object"` 로 인한 언어 자체의 버그
- 복합원시타입 독특한 객체 하위 타입이 있다. (function, Array...)



### 내용

```javascript
var myObj = {
  a: 2
}
myObj.a; // 2 ..프로퍼티 접근
myobj["a"]; // 2 ..키 접근	
```

> 같은 조회 방법이지만, 프로퍼티 접근 방식을 선호, 사용하라.
>
> 문자열 이외의 다른 원시 값을 쓰면 우선 문자열로 변환된다.
>
> 배열 인덱스로 사용하는 숫자도 마찬가지 이므로 공연히 객체와 배열 사이에 숫자를 써서 헷갈리는 코드를 짜지 마라!



- 계산된 프로퍼티명

```JAVASCRIPT
var prefix = "foo";
var myObj = {
  [prefix + "bar"]: "hello",
  [prefix + "baz"]: "world"
}

myObj["foobar"]; // hello;
myObj["foobaz"]; // world;
```



- 프로퍼티와 메서드

> 객체에 참조된 함수를 메서드라 할 수 없다.
>
> 개별 레퍼런스일 뿐 소유한 함수라는 의미가 아니기 때문이다. (소유의 의미가 중요하다.)



- 객체 복사

```javascript
function f() {};
var o = {
  c: true
};

var j = [];
var myObj = {
	a : 2,
  b : o, // 사본이 아닌 레퍼런스다.
  d : j, // 역시 레퍼런스다
  d : f
}
```

```javascript
// JSON 안전한 객체여야 한다.
// 참조된 함수 프로퍼티는 복사되지 않는다.
var newObject = JSON.parse(JSON.stringify(someObj));
```

- 얕은 복사는 ES6부터 Object.assign() 메서드 제공	
  - 순수하게 = 할당에 의해서만 복사하므로 writable같은 특수한 프로퍼티의 속성은 타깃에 객체에 복사되지 않는다.



### 프로퍼티 서술자

- 쓰기가능(Writable)

```javascript
var o = {};
Object.defindProperty(o, "n", {
  value: 1,
  writable: false, // 쓰기 금지
  configurable: true, 
  enumberable: true
})
```

- 설정가능(Configurable)
  - defineProperty로 프로퍼티 서술자를 변경할 수 있다.
  - configurable이 false가 되면 다시 돌아올 수 없다.

- 열거기능(enumerable)

> 객체 프로퍼티 순회 리스트에 포함 `for ... in` 연산자는 어떤 프로퍼티가 해당 객체에 존재하는지 아니면 객체의
>
> `[[ProtoType]]` 연쇄를 따라 갔을 때 상위 단계에 존재하는지 확인한다.
>
> `HasOwnProperty()` 는 단지 프로퍼티가 객체에 있는지만 확인하고 위 연쇄에서는 찾지 



### [[GET]]

> 주어진 프로퍼티 값을 어떻게 해도 찾을 수 없으면, [[GET]]은 undefined를 반환한다.
>
> 렉시컬 스코프내에 없는 변수를 참조하면 객체 프로퍼티처럼 undefined가 반환되지 않고, ReferenceError가 발생한다.



### [[PUT]]

1. 프로퍼티가 접근 서술자인가? 맞으면 세터 호출
2. 프로퍼티가 writable: false인가? 맞으면 실패 
3. 이외에는 프로퍼티에 해당 값을 세팅한다.



### 순회

- forEach() : 콜백 반환 값 무시
- every() : 콜백 반환 값이 false이면 중단
- some() : 콜백 반환 값이 true이면 중단
- for...of
  - 순회할 원소의 순회자 객체 `Iteration Object`
  - 순회자 객체의 next() 메서드를 호출하여 연속적으로 반환 값을 순회한다.
  - 배열은 `@@iterator`가 내장되어 있다.



```javascript
var a = [1, 2, 3];
var i = a[Symbol.iterator]();

i.next(); // {value: 1, done: false}
i.next(); // {value: 2, done: false}
i.next(); // {value: 3, done: false}
i.next(); // {done: true}
```

``````javascript
var randoms = {
  [Symbol.iterator]: function() {
    return {
      next: function() {
        return { value: Math.random() };
      }
		}
  }
}

var randoms_pool = [];

for(var n of randoms) {
  randoms_pool.push(n);
  if(random_pool.length === 100) break;
}

// for ... of 와 커스텀 순회자는 사용자 객체의 조작하는데 탁월한 구문도구.
``````

> ES6 부터는 for...of 구문에서 한 번에 하나씩 다음 데이터값으로 이동하는 next() 메서드를 가진 
>
> 내장/커스텀 @@iterator 객체를 통해 자료 구조(배열, 객체 등)에서 여러 값을 순회할 수 있다.



























