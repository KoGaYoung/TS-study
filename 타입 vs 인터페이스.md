# 목차 (12/09(토) study)
~~~
1. 차이, 각각 언제 사용하는지
2. 호출 시그니처(call signature)
3. 인덱스 시그니처(index signature)
4. 중첩 인터페이스
5. 다중 인터페이스 확장(상속은 객체지향프로그래밍쪽에서 학습)
6. 인터페이스 병합
~~~

## 기본 타입들을 훑어보자
~~~typescript
// 불리언(boolean), 숫자(number), 문자열(string) 생략

// 1. 배열(Array)
const testArray1: string[] = ['test1', 'test2']; // 이게 더 가독성 좋음
const testArray2: Array<string> = ['test1', 'test2'];

// 2. 열거(Enum)
enum day {Monday, Tusday, Wndsday};

// 3. Any

// 4. void

// 5. Never

// 6. 타입 단언(Type assertions)
// input 데이터 select 했을 때 타입값 어썰션 해줬던 기억이 난다...
~~~

## 1. type vs interface 
~~~
타입: 타입 정의 시 사용. 유니온(|)이나 인터섹션(&)으로 타입 조합함
인터페이스: 객체 구조 정의, 확장(상속) 가능 -> 클래스가 상속받아 구현하도록 강제, 같은 이름 선언 시 병합도 됨

둘 다 타입 정의하는데 사용 됨.
차이:
각 언제사용?

~~~
