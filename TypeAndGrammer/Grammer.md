# 문법

> 분해 할당 (Destructuring Assignments)(ES6)

```javascript
var {a, b} = ...;
// {a, b}는 {a: a, b: b}를 간결하게 쓴 형태의 간편 구문이다.
```

## if-else

> 자바스크립트에는 if else-if는 존재하지 않는다. if와 else과 단일 문 블록의 결과물이다.

```javascript
if(a) {
    ...
} else if(b) {
    ...
} else {

}
```

    위 처럼의 구문은 실제로 아래와 같은 코드로 파싱된다.

```javascript
if(a) {
    ...
} else {
    if(b) {
        ...
    } else {
        ...
    }
}
```

## 연산자 우선순위(Operator Precedence)

```javascript
(false && true) || true; // true

// &&는 언제나 || 보다 먼저 평가되고, 좌측 -> 우측
// ||는 (a ? b : c) 보다 먼저 평가 되고, 우측 -> 좌측
```

## 단락 평가

> &&, || 연산자는 좌측 피연산자의 평가 결과만으로도 전체 결과가 이미 결졍될 경우 우측 피연산자의 평가를 건너뛴다.

> 예를들어  
> &&, 좌측이 false라면 바로 평가끝  
> ||, 좌측이 true라면 바로 평가끝

## 결합성(Grouping)

> 처리순서를 정할 때, 반드시 필요한 부분만 ( )로 감싸주자. 연산자의 우선순위/결합성을 고려하여 적절하게 사용하는 것이 깔끔한 코드를 낳는다.

## ASI(Automatic Semicolon Insertion)

> 자바스크립트엔진은 `;`을 생략하여도 자동으로 이를 삽입한다.

    그렇다면, 굳이 개발자가 ;를 작성할 이유가 있나?

- ASI본질

  - 명세에 따르면, ASI는 에러를 정정하는 루틴이라 표기되어있다. 즉, 세미콜론을 생략하는 것은 에러이다. 반드시 넣어주자.

## Error

1. 올바르지 않은 정규식은 에러를 던진다.
2. 할당 대상은 반드시 식별자다
3. 엄격모드에서 함수 인자명은 중복될 수 없다.

## TDZ(Temporal Dead Zone)

> 아직 초기화를 하지 않아 변수를 참조할 수 없는 코드영역을 일컫는다.

## Try ... finally

- finally절에 예외가 던져지면, 이전 실행결과는 모두 무시한다.
- finally의 return은 try, catch절의 return을 덮어쓰는 능력이 있다.

## switch

- case표현식의 결과는 엄격하게 매칭한다.
- True, False로 떨어지게 `case !!(변수):`로 작성하도록한다.
