# 스코프와 렉시컬 this

> 동적 스코프는 자바스크립트의 또 다른 메커니즈(this)과 가까운 친척관계다.

## 동적 스코프

- 런타임때에 동적으로 결정한다.
- 동적 스코프 체인은 코드 내 스코프의 중첩이 아니라 `콜-스택(Call-Stack)`과 관련있다.
- 자바스크립트는 동적 스코프를 사용하지 않고 렉시컬 스코프만 사용한다.
  - 렉시컬 스코프는 작성할 때, 동적 스코프는(this)는 런타임에 결정된다.
  - 렉시컬 스코프는 어디서 함수가 선언됐는지와 관련있지만, 동적 스코프는 어디서 함수가 호출되었는지와 관련있다.

## 폴리필링 블록 스코프

> ES6에서 키워드 let을 지원하면서 자바스크립트도 완전히 제한 없는 블록 스코핑을 지원하게 되었다.

```javascript
{
  let n = 1;
  console.log(n); // 1
}
console.log(n); // ReferenceError
```

```javascript
{
    try(throw 2)catch(n) {
        console.log(n); //2
    }
}
console.log(n); // ReferenceError
```

## 렉시컬 this

- var self = this...
- bind()
