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

~~~
~~~typescript
// 2. 열거(Enum)
// 이넘은 생각보다 중요합니다.
// 기획된 상태 값 표현할 때 많이 사용함 (특히 select 할 수 있는 inputBox 구성 시 사용)
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
// 타입 검사 패스, 외부 꺼(서드파티) 사용할 때 유용
// 대부분의 eslint 설정 시 기본값이 any 쓰지말라고 하는데 꼭 써야하는경우 해당 문장만 패스하게 만들어 쓴다.

// 4. void
// 함수 리턴이 없을 때 사용 (유용한 것을 반환하지 않는다는 뜻)
~~~

~~~typescript
// 5. Never
// 아무 것도 없는 양을 나타낼 때 0을 쓰는 것처럼, 불가능을 나타내는 타입
1. 값을 포함할 수 없는 빈타입
 - 제네릭에서 허용하지 않는 매개변수가 왔을 때
 - 호환되지 않는 교차타입
2. 실행이 끝날 때 까지 호출자에게 제어를 반환하지 않는타입
ex. Node의 process.exit
3. 절대 도달할 수 없는 조건
4. 거부된 프로미스에서 처리된 값
ex. const p = Promise.reject('foo') // const p: Promise<never>
https://ui.toast.com/posts/ko_20220323

// 6. 타입 단언(Type assertions)
// input 데이터 select 했을 때 타입값 어썰션 해줬던 기억이 난다...
// 하지만 앞으로는 위 enum 처럼 선언해서 사용할 것이다..
// 위 예제에서 value를 안만들어주고, value.split('-')[1] 이렇게 쓸 경우,
// 우리는 "0-아파트" 나 "1-주택/빌라" 만 올거라고 알지만, 컴퓨터는 모른다.
// 이때는 아래처럼 value는 EBuilding 라고 단언하는 것이다
const value = event.target.value as EBuilding;
~~~

## 1. type vs interface 
~~~typescript
둘 다 타입 정의하는데 사용 됨.

타입: 타입 정의 시 사용. 유니온(|)이나 인터섹션(&)으로 타입 조합, 타입 조건 사용 가능(위 매핑 타입예제)
(조건부 타입: T extends U ? X : Y, T가 U에 포함될 경우 경우 X, 아닌경우 Y)

type sick = {
   headache: booelan;
}

type sicks = sick & {
  toothache: booelan;
}

인터페이스: 객체 구조 정의 시 사용. 확장(상속) 가능 -> 클래스가 상속받아 구현하도록 강제, 같은 이름 선언 시 병합도 됨
interface sick {
  headache: boolean;
}

interface sick {
  toothache: boolean;
}

// sick { headache: boolean; toothache: boolean; } 자동 병합 됨

~~~

## 2. 호출 시그니처(call signature) 
