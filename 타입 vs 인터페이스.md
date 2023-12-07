# 목차 (12/09(토) study)
~~~
1. 차이, 각각 언제 사용하는지
2. 호출 시그니처(call signature)
3. 인덱스 시그니처(index signature)
4. 중첩 인터페이스
5. 다중 인터페이스 확장(상속은 객체지향프로그래밍쪽에서 학습)
6. 인터페이스 병합
~~~

## 1. type vs interface 
~~~typescript
// 타입, 인터페이스 둘 다 타입 정의하는데 사용 됨.
~~~
~~~typescript
// 타입: 간단한 타입 정의 시 사용. 재사용이 적은 경우에 사용
// 유니온(|)이나 교차(&)으로 타입 조합, 타입 조건 사용 가능(위 매핑 타입예제)
// (조건부 타입: T extends U ? X : Y === T가 U에 포함되나요? X : Y)

type sick = {
   headache: booelan;
}

type newSick = sick & { // 같은 이름 사용시 오류나요
  toothache: booelan;
}
~~~
~~~typescript
// 인터페이스: 복잡한 객체 구조 정의 시 사용. 확장(상속) 가능 -> 클래스가 상속받아 구현하도록 강제
// 같은 이름 선언 시 병합도 됨
// 상속을 해야하는 객체의 경우 extends 키워드 사용시 캐싱하여 성능상 유리하다
// 대고객 서비스에서는 인터페이스를 기본적으로 사용하는것을 선호한다.
interface sick {
  headache: boolean;
}

interface sick {
  toothache: boolean;
}

// sick { headache: boolean; toothache: boolean; } 자동 병합 됨
~~~

## 2. 호출 시그니처(call signature) 
~~~typescript
// 함수에 커서를 가져다대면 나타내는 파라미터와 리턴값의 타입을 호출시그니처라고 한다. (나름의 다형성을 구현함)
~~~
~~~typescript
// type
type Tfunction = (a: number, b:number) => void;

// a, b 타입 지정 안해줘도됨
const firstFunc: Tfunction = (a, b) => { console.log('first'); };

// es5 function 형태로도 쓸 수 있다.(const 변수에 함수를 저장하는 익명함수(함수 표현식) 형태로만 사용가능)
const secondFunc: Tfunction = function(a, b) { console.log(a+b); };
~~~
~~~typescript
// interface
interface Ifunction {
  (a: number, b:number): void;
}

const iFirstFunc: Ifunction = (a, b) => { console.log('first'); };

const iSecondFunc: Ifunction = function (a, b) { console.log(a+b); };
~~~
~~~typescript
// 선택적 파라미터가 있다면 이렇게도 쓸 수 있어요
type Tfunction = {
  (a: number, b:number) :  void;
  (a: number, b:number, c: number ) : void;
}

const typeFunction: Tfunction = (a, b, c?: number) => {
  if (c) {
    console.log(a + b + c);
  }
  else {
    console.log(a+b);
  }
}
~~~

## 3. 인덱스 시그니처(index signature)
~~~typescript
// 객체의 인덱스 타입을 정의할 때 사용
// 키 이름은 다를 수 있지만 동일한 값을 리턴할 때 활용

type TResponse = {
  [key: string] : string;
}

interface IResponse {
  [key: string] : string;
}

const apiResponse: IResponse = {
  name: 'gayoung',
  id: '1',
  address: 'guro'
} 
~~~

## 4. 중첩 인터페이스
~~~typescript
// 인터페이스 안에 인터페이스 사용하는것
// 필요에 따라 적절하게 나누면 구조적으로 가독성, 재사용성, 유지보수성 좋아짐

interface IAddress {
  street: string;
  zipcode: string;
}

interface IWorker {
  name: string;
  phone: string;
  homeAddress: IAddress; // 집주소
  workAddress: IAddress; // 회사주소
}
~~~

## 5. 다중 인터페이스 확장
~~~typescript
// extends 뒤에 여러개가 올 수 있다.
// 사람정보
interface IPerson {
  name: number; // 이름
  address: IAddress; //주소
  phone: string; // 전화번호
}

// 경력정보
interface ICareer {
  workYear: number; // 경력
  workSpace: string; // 근무지
}

// 이력서 (IResume는 IPerson, ICareer 모든 조건을 만족해야하니까 교차타입('&')으로 동작한다고 할 수 있음
interface IResume extends IPerson, ICareer {
  privacyAgree: boolean; // 개인정보 제공 동의
}
~~~

## 6. 인터페이스 병합
~~~typescript
// 위에서 이미 정리한 내용.
// 유연성과 확장성을 높여주지만, 가독성이 안좋아지고 명확성이 떨어지기때문에 같은이름의 인터페이스를 두는것을 나는 꼭 필요한 경우가 아니라면 비선호한다.
~~~
