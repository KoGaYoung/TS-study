## 간단하게 TS test 할 수 있는 사이트
[typescriptlang](https://www.typescriptlang.org/play)

---
## 타입스크립트 딥다이브
[1.	타입 애너테이션 (12/09 완료)](./타입_애너테이션.md)
~~~
0. 타입스페이스, 벨류스페이스
1. 기본타입
2. 읽기 전용 속성
3. 선택적 속성
~~~

[2.	타입 vs 인터페이스 (12/09 완료)](./타입_vs_인터페이스.md)
~~~
1. 차이, 각각 언제 사용하는지
2. 호출 시그니처(call signature)
3. 인덱스 시그니처(index signature)
4. 중첩 인터페이스
5. 다중 인터페이스 확장(상속은 객체지향프로그래밍쪽에서 학습)
6. 인터페이스 병합
~~~

[3.	함수 (12/09 완료)](./함수.md)
~~~
0. 함수 리턴타입, 파라미터, Await 활용(실전)
1. this와 화살표함수
2. 나머지 매개변수 (Rest Parameters)
3. 콜백에서 this 매개변수
4. 오버로드
~~~
[4.	유니언과 교차타입 (12/16 완료)](./유니언과_교차타입.md)
~~~
1. 유니언 타입과 교차 타입의 사용 사례
2. 유니언 타입에서의 타입 가드
3. 교차 타입의 장점과 사용법
~~~
[5.	클래스 (11/25 완료)](./클래스와_인터페이스.md)
~~~
1. 객체지향 프로그래밍
2. 클래스/인터페이스
3. 캡슐화
4. 다형성
5. 추상화
6. 제어의역전 + 의존성주입
~~~
[6.	열거형 (선택사항)] ()
~~~
~~~
[7.	제너릭 (12/22 완료)](./제너릭.md)
~~~
1. 제너릭을 사용하는 이유
2. 기본 제너릭 타입 정의
3. 제너릭 제약조건
4. 제너릭 유틸리티 타입
~~~
[8. 연산자](./연산자.md)
~~~
1. keyof, typeof 연산자
2. indexd type(인덱스 접근 유형)
3. 조건부 타입
4. mapped 타입 ( + 맵드 타입(Mapped Types)에서 속성을 제거하는 데 사용 하는 (-) 연산자 )
5. 템플릿 리터럴
7. 선택적 속성(?), readonly 연산자 - 1에서 함 (생략)
8. Non-null 단언 연산자(!)
~~~
[9.	타입 가드와 타입 어설션(as)] ()
~~~
1. 타입 강제 변환: 타입 강제 변환(캐스팅)과 관련된 주의점
2. 사용자 정의 타입 가드
~~~
[10. generic 심화 + 타입 추론(infer)] ()
~~~
~~~

[11. ts-pattern 활용] ()
~~~
~~~

[12. 타입호환]
~~~
1. 타입 호환성, 타입 호환성의 기준(타입이 다른 변수 간 호환성 판단 기준)
~~~

---

2. 
## type challenges 
[type-challenges](https://github.com/type-challenges/type-challenges) 공부하면서 풀기


|no|난이도|링크|키워드|
|---|:---|:---:|---:|
|1|<img src="https://img.shields.io/badge/warm--up-1-teal" alt="1"/> | [13.Hello World](./challenge/13-hello-world.md) | - |
|2|<img src="https://img.shields.io/badge/easy-4-7aad0c" alt="13"/>|[4.pick](./challenge/4-pick.md)|부분집합 검사|
|3|<img src="https://img.shields.io/badge/easy-7-7aad0c" alt="13"/>|[7.readonly](./challenge/7-readonly.md)|readonly key-value|
|4|<img src="https://img.shields.io/badge/easy-43-7aad0c" alt="13"/>|[43.exclude](./challenge/43-exclude.md)|Union, Exclude|
|5|<img src="https://img.shields.io/badge/easy-898-7aad0c" alt="13"/>|[43.include](./challenge/898-include.md)|Union, Include|

## practice 
함수 리턴 타입, 파라미터, Awaited의 활용 [문제, 풀이](./challenge/함수_리턴_타입,_파라미터,_Awaited의_활용.md) <br/>
Union_추출, 제외 [문제, 풀이](./challenge/Union_extract_excloude.md) <br/>
제네릭 [문제, 풀이](./challenge/제네릭.md) <br/> 
