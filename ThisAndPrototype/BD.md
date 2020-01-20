# 작동 위임 (Behavior Delegation)

> [[Prototype]] 체계는 한 객체가 다른 객체를 참조하기 위한 내부 링크다.
>
> [[Prototype]] 링크에 연결된 객체를 타고 이동한다. 이렇게 수색한다.
>
> 자바스크립트의 무한한 원동력은 `객체를 다른 객체와 연결하는 것`에서 비롯된다.



## 위임 지향 디자인으로 가자

- 클래스/상속 디자인패턴에서 벗어나라
  - 자바스크립트는 작동 위임 디자인 패턴이다.

### 위임 이론

```javascript
Task = {
  setID : function(ID) { this.id = ID; },
  outputID : function(ID) { console.log(this.id()}
}
  
// 'XYZ'가 'Task'에 위임한다.
XYZ = Object.create(Task);
XYZ.prepareTask = function(ID, Label) {
  this.setID(ID);
  this.label = Label;
}
XYZ.outputTaskDetails = function() {
  this.outputID();
  console.log(this.label);
}
```

> 이러한 스타일을 저자는 `OLOO(Object Linked to Other Object)` 라 부른다.

- OLOO 패턴에서는 클래스 디자인 패턴에서 쓰이는 다형성, 오버라이딩을 피해야한다.
- 작동 위임 패턴에서는 오버라이드보다 각 객체의 작동 방식을 잘 설명하는 서술적인 명칭이 필요하다.
  - 메서드의 이름만 보고도 어떠한 작동을 하는지, 그 의미가 분명 => 유지보수, 코드의 가독성 상승

> `작동 위임` 이란, 찾으려는 프로퍼티/메서드 레퍼런스가 객체에 없으면, 다른 객체로 수색 작업을 `위임` 하는 것을 의미한다.

- 수직적 클래스는 필요없다. 서로 수평적으로 배열된 상태에서 위임 링크가 체결된 모습을 그려라



## 인스트로펙션

> 클래스지향 프로그래밍에서의 정의 : 인스턴스를 조사해 객체 유형을 거꾸로 유추하는 타입 인스트로펙션

- instanceOf - 클래스와 착각을 줄 수 있기에, 사용을 잘 하지 않는다.
- OLOO 방식에서는 `isPrototypeOf()` 를 이용하여, 타입 인스트로펙션을 간단하게 사용할 수 있다.