# 프로그램 성능

> 이 장에서도 역시, 조금 더 고급적인 내용을 다루고있다. 이해하기 어려웠다.
>
> 컴퓨터사이언스지식을 더욱 쌓고 본다면, 좋은 내용이 될 것 같다.



## 웹 워커

> 자바스크립트가 멀티스레드로 돌아갔으면 하는 상상.
>
> 자바스크립트는 '현재까지' 스레드 실행을 지원하지 않는다.

- 브라우저 같은 환경은 다수의 자바스크립트 엔진인스턴스를 쉽게 내어줄 수 있다. 그리고 그 인스턴스마다 개별 스레드를

  배정하여 실행 할 수도 있다.

  - 이러한, 독립적인 스레드 조각을 `웹 워커(Web Worker)`라고 한다.
  - 프로그램을 덩이로 나누어 병렬 실행하는 `작업 병행성(Task Parallerlism)` 을 추구한다.

~~~javascript
var w1 = new Worker("http://some.url.1/coolworker.js");
// url로 생성한 워커를 전용워커라 한다.
~~~

- 브라우저에서 다수의 페이지가 같은 URL로부터 워커를 생성하려고 하면, 각 페이지는 완전히 별개의 워커로 움직인다.



## SIMD(`Single Instruction Multiple Data`)

> 데이터 병행성을 나타내는 형식으로, 웹 워커의 작업 병행성과는 대조되는 개념이다.
>
> 여러 데이터 비트를 병렬로 처리하는 일이다.

- SIMD는 스레드 병행성을 제공하지 않는다.
- 현대의 CPU는 숫자 `VECTOR` 와 모든 숫자에 병렬 연산이 가능한 명령어 세트를 이용하여 SIMD기능을 제공
- 데이터 집약적 애플리케이션은 자바스크립트에 SIMD가 지원된다면 성능 향상을 기대할 수 있다.



## asm.js

> asm.js는 자바스크립트 언어에서 고도로 최적화 가능한 부분 집합을 뜻한다.

- 최적화 하기 어려운 특정한 체계와 패턴을 방지하여 아주 공격적으로 저수준 최적화를 시행한다.



## 정리

> 비동기 특성 또한 근본적으로는 단일 이벤트 루프 스레드에 묶여있다. 즉, 한계가 있다.

1. 웹 워커
   - 자바스크립트 파일을 개별 스레드 단위로 실행하게 해준다. 
   - 메인 UI 스레드의 응답성을 높이면서도 소요 시간이 길거나 자원을 집중적으로 소모하는 작업을 다른 스레드에 분산
2. SIMD
   - CPU 수준의 병렬 수학 연산을 고성능 병렬 데이터 연산에 특화된 자바스크립트 API로 연결짓는다
3. asm.js
   - 최적화 하기 어려운 영역(`GC, 강제 변환`)을 피해서 엔진이 최적화를 유도하도록 하는 자바스크립트의 부분집합



## 느낀점

전체적으로 아직 나와 닿기 힘든 부분이었다. 평소 비동기 패턴이 가져오는 좋은 성능에 감탄하지만, 자바스크립트 성능의

시야를 넓힐 수 있었던 장이었다.