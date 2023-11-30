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
// 불리언(boolean), 숫자(number), 문자열(string) 생략

// 1. 배열(Array)
const testArray1: string[] = ['test1', 'test2']; // 이게 더 가독성 좋음
const testArray2: Array<string> = ['test1', 'test2'];

~~~
~~~typescript
// 2. 열거(Enum)
// 이넘은 생각보다 중요합니다.
// 기획된 상태 값 표현할 때 많이 사용함 (특히 진행 상태, select 할 수 있는 inputBox 구성 시 사용했었음)
enum day {Monday, Tusday, Wndsday};
console.log(day.Monday); // 0 으로 자동 초기화됨

<select name="building">
  <option value="0-아파트">아파트</option>
  <option value="1-주택/빌라">주택/빌라</option>
</select>

// ----

enum EBuilding {
  Apartment = "0-아파트",
  HouseVilla = "1-주택/빌라"
}

// React 컴포넌트 내부에서 enum 값을 사용하여 옵션 생성
<select name="building">
  {Object.entries(EBuilding).map(([key, value]) => (
    <option key={key} value={value}>{value.split('-')[1]}</option>
  ))}
</select>

// ----

// 만약 value.spit('-')[1] 도 마음에 안든다면!? (나는 맘에 안든다..)
// Building type 생략

// Mapped type 사용해서 enum 앞 ('number-')접두사 제거
type removePrefix<T extends string> = T extends `${infer Prefix}-${infer Rest}` ? Rest : T;

// 빌딩 네임도 enum으로 만들고 싶었는데, type으로 enum 생성은 불가능하다.
type TBuildingNames = {
  [K in keyof typeof EBuilding]: removePrefix<typeof EBuilding[K]>;
}

// BuildingNames 타입을 사용하는 객체
const buildingNames: TBuildingNames = {
  Apartment: "아파트",
  HouseVilla: "주택/빌라"
};

<select name="building">
  {Object.entries(buildingNames).map(([key, value]) => (
    <option key={key} value={EBuilding[key as keyof typeof EBuilding]>{value}</option>
  ))}
</select>
~~~

~~~typescript
// 3. Any
// 타입 검사 패스, 외부 꺼(서드파티) 사용할 때 사용
// 대부분의 eslint 설정 시 기본값이 any 쓰지말라고 하는데 꼭 써야하는경우 해당 문장만 패스하게 만들어 쓴다.
const thirdParty: any;

// 4. void
// 함수 리턴이 없을 때 사용 (유용한 것을 반환하지 않는다는 뜻)
type func1 = () => void;
~~~

~~~typescript
// 5. Never
// 아무 것도 없는 양을 나타낼 때 0을 쓰는 것처럼, 불가능을 나타내는 타입
// -> 타입 추출, default 조건으로 활용 가능
// 네버는 알면 무조건 도움이 된다!

// 언제 사용될까
1. 값을 포함할 수 없는 빈타입
 - 제네릭에서 허용하지 않는 매개변수가 왔을 때
 - 호환되지 않는 교차타입
2. 실행이 끝날 때 까지 호출자에게 제어를 반환하지 않는타입
 ex. Node의 process.exit
3. 절대 도달할 수 없는 else 조건
4. 거부된 프로미스에서 처리된 값
ex. const promise1 = Promise.reject('foo') // const p: Promise<never>

// 활용을 알아보기 전에 never의 특징을 알아보자
// 유니언('|')은 A or B를 나타내고 교차('&')는 모두를 만족하는 새로운 타입을 나타낸다.

// 0 + number = number 유니언('|')에서 never는 없어짐
type resUnion = never | string; // string
// 0 * number = 0 교차('&')에서는 덮어쓴다
type resCross = never & string; // never

// 실 활용
// 1. switch나 if-else 문 모든상황 보장 (절대 도달할 수 없는 else 조건)
function unknownSometihng(animal: never) : never {
  throw new Error("unknownSometihng");
}

type TAnimal = 'dog' | 'cat';

function divideAnimal(animal: TAnimal): string {
  switch (animal) {
    case 'cat':
      return 'it it cat';
    case 'dog':
      return 'it is dog';
    default:
      return unknownSometihng(animal)
  }
}

// 2. 부분 타이핑 허용 안함
type aPerson = {
  a: number;
}

type bPerson = {
  b: number;
}

// 함수 선언, param은 APerson 또는 bPerson으로 유니언('|') 사용
declare function fn(param: aPerson | bPerson) : void;

fn({a: 0, b: 1}); // 오류 안남

// ---
// 구조적 타이핑 비활성화 -> 두 속성 모두 포함하는 객체 전달 불가능
type aPerson1 = {
  a: number;
  b?: never;
}
type bPerson1 = {
  a?: never;
  b: number;
}

declare function fn1(param: aPerson1 | bPerson1) : void;
fn1({a: 1, b:1}); // 오류남

// 유니언 타입에서 멤버 추출할 때 else 문에 사용됨
type aPerson = {
  name: 'aName';
  a: number;
}

type bPerson = {
  name: 'bName';
  b: number;
}

type TAllPerson = aPerson | bPerson;

type extractTypeByName<T, G> = T extends {name: G} ? T : never;

type aType = extractTypeByName<TAllPerson, 'aName'>; // aType = aPerson 타입
~~~

~~~typescript
// 6. 타입 단언(Type assertions)
// 위 예제에서 value를 안만들어주고, value.split('-')[1] 이렇게 쓸 경우
// 우리는 "0-아파트" 나 "1-주택/빌라" 만 올거라고 알지만, 컴퓨터는 모른다.
// 이때는 아래처럼 value는 EBuilding 라고 단언해서 타입 오류를 피할 수 있다.

const value = event.target.value as EBuilding;

// 하지만 앞으론 enum을 활용해서 사용할것이다. - _-+
~~~

## 1. type vs interface 
~~~typescript
// 타입, 인터페이스 둘 다 타입 정의하는데 사용 됨.

// 타입: 간단한 타입 정의 시 사용. 재사용이 적은 경우에 사용
// 유니온(|)이나 교차(&)으로 타입 조합, 타입 조건 사용 가능(위 매핑 타입예제)
// (조건부 타입: T extends U ? X : Y === T가 U에 포함되나요? X : Y)

type sick = {
   headache: booelan;
}

type newSick = sick & { // 같은 이름 사용시 오류나요
  toothache: booelan;
}

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

type Tfunction = (a: number, b:number) => void;

// a, b 타입 지정 안해줘도됨
const firstFunc: Tfunction = (a, b) => { console.log('first'); };

// es5 function 형태로도 쓸 수 있다.(const 변수에 함수를 저장하는 익명함수형태로만)
const secondFunc: Tfunction = function(a, b) { console.log(a+b); };

interface Ifunction {
  (a: number, b:number): void;
}

const iFirstFunc: Ifunction = (a, b) => { console.log('first'); };

const iSecondFunc: Ifunction = function (a, b) { console.log(a+b); };
~~~

## 3. 인덱스 시그니처(index signature)
~~~typescript
// 객체의 인덱스 타입을 정의할 때 사용
// 키 이름은 다를 수 있지만 동일한 값을 리턴할 때 유용

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

// 이력서 (IPerson과 ICarrer는 IPerson, ICareer 모든 조건을 만족해야하니까 교차타입('&')으로 동작
interface IResume extends IPerson, ICareer {
  privacyAgree: boolean; // 개인정보 제공 동의
}
~~~

## 6. 인터페이스 병합
~~~typescript
// 위에서 이미 정리한 내용.
// 유연성과 확장성을 높여주지만, 가독성이 안좋아지고 명확성이 떨어지기때문에 같은이름의 인터페이스를 두는것을 나는 꼭 필요한 경우가 아니라면 비선호한다.
~~~
