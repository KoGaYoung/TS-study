# 목차
~~~
1. 선언적 프로그래밍 vs 명령적 프로그래밍
2. 라이브러리 도입 전 검토(라이브러리 용량, 사용에 미치는 영향이 있는지)
3. ts-pattern 장단점
4. 프로젝트에 ts-pattern 적용해보기
 - 패턴 매칭, 가드, 안전한 타입 처리 기능
~~~

~~~
타입스크립트 라이브러리로, 선언적 프로그래밍을 하는데 도움을 준다고합니다.

명령형 프로그래밍은 문제를 어떻게 해결해야하는지 컴퓨터에게 명시적으로 명령을 내리는 방법을 의미하고,

선언형 프로그래밍은 무엇을 해결할 것인지에보다 
집중하여 어떻게 문제를 해결하는지에 대해서는컴퓨터에게 위임하는 방법이다

ex. 
명령형 접근(HOW): "저기 Gone Fishin' 이라고 적힌 표지판 아래에 있는 테이블이 비어있네요.
우리는 저기로 걸어가서 저 테이블에 앉도록 하겠습니다."
선언형 접근(WHAT): "2명 자리 주세요."

=> 선언형으로 가기위해서는 '어떻게 접근하는가'에 관한 내용이 명령형에서 추상화 되어있어야 한다 

선언형이 좋다는데! 왜좋지? 문제를 더욱 가독성있게 효율적으로 전달한다!
~~~
![image](https://github.com/KoGaYoung/JS-study/assets/36693355/01d12adf-3769-43ca-a893-47289d210993)

~~~javascript
// 명령형
function addOne (arr) {
    let results = [];
    for(let i=0; i<arr.length; i+=1){
        results.push(arr[i]+1);
    }
    return results;
}
~~~
~~~javascript
// 선언형
// 무엇(WHAT)이 일어날지에만 자바스크립트 내장함수 map 내부에 집중함
function addOne (arr) {
    return arr.map((i) => i+1);
}
~~~

## 2. 라이브러리 도입 전 검토(라이브러리 용량, 사용에 미치는 영향이 있는지)
~~~
라이브러리 용량: 2kB 정도
사용자한테 영향을 미치는지?: 런타임(사용자환경)에서도 패턴매칭 코드가 동작해야 하기 때문에 
dev dependacy가 아닌 일반 depandancy로 추가해야함
용량이 2kb정도임을 감안했을때 빌드, 배포시에도 사용자경험에 영향을 미치지 않을 수준이라고 판단.
~~~

## 3. ts-pattern 장단점

~~~
장점 : 복잡할 수록 가독성좋은 패턴매칭 구현 가능 (조건부 로직 예쁘게 구현 가능, 패턴 매칭, 가드와 매처)
복잡한 조건문이 많은 프로젝트에 효과적

단점 : 약간의 러닝커브.. 단점은 아니지만 타입스크립트에 종속적임
~~~

## 4. 프로젝트에 ts-pattern 적용해보기

~~~typescript
type Data = { type: 'text'; content: string } | { type: 'id'; id: number };

const data: Data = { type: 'text', content: 'hello' };

let result: string;

switch (data.type) {
  case 'text':
    result = `Text: ${data.content}`;
    break;
  case 'id':
    if (typeof data.id === 'number') {
      result = `ID: ${data.id}`;
    }
    break;
  default:
    throw new Error('Invalid type');
}

console.log(result); // 'Text: hello'

~~~


~~~typescript
import { match, P } from 'ts-pattern';

type Data = { type: 'text'; content: string } | { type: 'id'; id: number };

const data: Data = { type: 'text', content: 'hello' };

const result = match(data)
  .with({ type: 'text' }, (x) => `Text: ${x.content}`)
  .with({ type: 'id', id: P.number }, (x) => `ID: ${x.id}`)
  .exhaustive();

console.log(result); // 'Text: hello'

~~~