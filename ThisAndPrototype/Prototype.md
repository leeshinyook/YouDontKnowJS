# 프로토타입

## 프로토타입 연쇄

- 객체의 연쇄를 전부 샅샅이 뒤진다. 위로 올라간다.
- 최상위 지점은 `Object.prototype` 이다.
- 연쇄과정중, 상위 수준 두 곳에서 동시에 발견될 때 가려짐(Shadowing) 발생

```javascript
var another = {
  a: 2
}
var myObject = Object.create( anotherObject );
anotherObject.a; // 2
myObject.a; // 2

anotherObject.hasOwnProperty("a"); // true
myObject.hasOwnProperty("a"); // false

myObject++; // 암시적 가려짐 발생

anotherObject.a; // 2
myObject.a; // 3

myObject.hasOwnProperty("a"); // true
```

> [[Prototype]]을 경유하여 [[Get]]을 먼저 찾고, anotherObject.a에서 현재 값 2를 얻은 후,
>
> 1만큼 증가시킨뒤, 그 결과값 3을 다시 [[Put]]으로 myObject에 새로운 가려짐 프로퍼티 a를 생성한 뒤 할당.



## 클래스

> 일종의 클래스 같은 독특한 작동은 모든 함수가 기본으로 프로토타입이라는 공용 / 열거불가 프로퍼티를
>
> 가진다는 특성에 기인한다.

```javascript
function F() {
  //...
}
F.prototype; // {}
var o = new F(); // new로 새로운 객체 o가 만들어지고, F.prototype 객체와 내부적으로
								 // [[Prototype]]과 연결이 맺어진다.
Object.getPrototypeOf(o) === F.prototype; // 2
```

> 자바스크립트에서 객체들은 서로 완전히 떨어져 분리되는 것이 아닌, 끈끈하게 연결된다.
>
> 이를 프로토타입 상속이라고 한다.



## 프로토타입 상속

```javascript
function f() {
  //...
}
function g() {
  //...
}
// f 프로토타입과 연결된 새로운 g프로토타입을 만들라.
g.prototype = Object.create(f.prototype);

// f 생성자 호출로 인한, 부수효과가 발생할 수 있다.
g.prototype = new f();
```

> Object.create() 를 사용하라!



##  클래스 관계

- ES6, setPrototypeOf, getPrototypeOf를 사용하라.

> instance of만으로는 두 객체가 서로 [[Prototype]] 연쇄를 통해 연결되어있는지 알 수 없다.





## 객체 링크

- Object.create()

```JAVASCRIPT
// ES5 이전에 사용되었던 폴리필이다.
if(!Object.create) {
  Object.create = function(o) {
    function F() {}
    F.prototype = o;
    return new F();
  };
}
```

- 위임 디자인 패턴(`Delegation Design Pattern`) : 구현 상세를 겉으로 노출하지 않고 내부에 감추는 식으로 위임
  - 자바스크립트 객체 간의 관계는 복사되는 것이 아니라, 위임 연결이 맺어진 것. 

> ES5 부터는 , Object.create()이 사용가능하다.



## 자바스크립트 체계

- 전통적인 클래스 지향 언의의 `클래스 인스턴스화 및 클래스 상속` 과 유사해 보인다.
  - 하지만, 자바스크립트에서는 복사가 일어나지 않는다.
  - 객체는 결국 다른 객체와 내부 `[[Prototype]]` 연쇄를 통해 연결된다.







































