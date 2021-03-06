# 강제변환

## 값변환

> 명시적 : 타입캐스팅(컴파일시점)  
> 암시적 : 강제변환(런타임시점)

> 강제변환을 하면 문자열, 숫자, 불리언 등 (스칼라 원시 값) 중 하나가 된다.

- 사람마다 명시적, 암시적 이러한 용어들을 나와 같은 느낌으로 받아들이진 않는다.

## toString()

- `[[Class]]`를 반환한다. ex)[object Object]

## JSON 문자열화

- JSON.stringify()
  - JSON 안전 값(JSON-Safe Value)
  - 안전하지 않는 값:`undefine`, `function`, `symbol`, 환영참조
  - 위 값들은 자동 누락
  - 배열에 포함되어있으면 `null`처리

## ToNumber()

> `valueOf()`를 쓸 수 있고, 반환 값이 원시 값이면, 그대로 강제반환한다.  
> 그렇지않으면, `toString()`을 이용하여 강제 반환한다.

## Falsy값

> 명세가 정의한 `falsy`

- undefined
- null
- false
- +0, -0, NaN
- ""

**위 내용을 외워라. 위에 없는 정의값은 전부 truthy값으로 귀결된다.**

## 명시적 강제변환

## 문자열 <-> 숫자

```javascript
var a = 42;
var b = a.toString();

var c = "3.14";
var d = +c;

b; // "42"
d; // 3.14
```

- `+c`가 명시적 강제변환이다.

> 평소 코딩테스트에서도 나는 String을 number로 변환하고자, 그 값이 \*1을 하곤 했다.

## ~(틸드)

```javascript
~42; // -(42 + 1) ==> -43
```

> 문자열에 하위 문자열이 포함되는지 확인하기 위해 평소 이런 코드를 작성하곤 했다.

```javascript
var a = "Hello world";

if (a.indexOf("lo") >= 0) {
  // found it!!
}
```

_indexOf()메서드는 특정 문자를 검색하고 발견하면 0, 그렇지 않은경우 -1을 반환한다._

    이 책에서는 위 같은 코드를 `Leaky Abstraction` 이라 정의한다. (경계값 -1을 '실패'로 정의한 것)

- 위 예제를 ~(틸드)를 이용하면 적절한 Boolean값으로 강제변환 가능하다.

```javascript
var a = "Hello world";

if (~a.indexOf("lo")) {
  // 못 찾음.
}
```

## parseInt()

> 문자열에 쓰는 함수이다.  
> 좌 -> 우 방향으로 파싱하다가 숫자 같지않은 문자를 만나면 그 즉시 멈춘다.

- 내부 로직
  - 인자 값을 강제로 문자열로 바꾼다음(toString()) 파싱을 실행한다.

```javascript
var a = {
  num: 21,
  toString: function() {
    return String(this.num * 2);
  }
};
parseInt(a); // 42
```

## Boolean강제변환

- 명시적 강제변환
  1. `!!`이중부정 연산자
  2. `Boolean()`

> 위 방법을 사용하지 않는다면, truthy, falsy까지 뒤바뀐다.

## 암시적 강제변환의 안전한 사용법

1. 피연산자 중 하나가 true/false일 가능성이 있으면 절대 == 연산자를 사용하지 않는다.
2. 피연산자 중 하나가 [], " ", 0이 될 가능성이 있으면 가급적 == 연산자를 쓰지 말자.

> 전반적으로 암시적 강제변환이 정말로 위험한 경우는 흔지 않는다. 이러한 경우도 ===로 대체하여 안전하게 가면 그만.

### 4장을 읽고,

전반적으로 당황스러웠다. 평소 내가 당연하다 여기면서 사용하는 변환에 너무 많은 자바스크립트 개발자들의 찬반토론과, 변환과정이 숨겨져있었다. 암시적 강제변환이 나쁘다라고 말하여도 숨겨진 로직에 의한 부수 효과를 잘 알고 사용한다면, 오히려 코드 가독성을 얻는다는 저자의 말에 깊은 생각을 가지게 된다.
