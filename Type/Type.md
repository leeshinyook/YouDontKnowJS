# 타입

내장타입
----
- null
- boolean
- undefined
- number
- string
- object
- symbol(ES6부터 추가)

        object를 제외한 내장타입을 원시타입이라고 부른다. 

값은 타입을 가진다
-----
typeof -> 이 변수의 타입은 무엇이니?

            typeof의 반환값은 언제나 문자열이다.

typeof의 안전가드
-----
```javascript
if(typeof atob === "undefined") {
    atob = function() {...}
}
```
- 이 변수의 선언여부를 체크할 때, typeof의 안전가드가 유용하다.
  

undefined와 undeclared
-----
- undefined는 선언된 변수에 할당할 수 있는 값.
- undeclared는 변수자체가 선언된 적이 없음.

        위 차이를 자바스크립트는 이 두 용어를 대충 합쳐 undefined로 통합시켜버린다.
        이런 상황에서 typeof안전가드를 이용하면 아주 좋다.



