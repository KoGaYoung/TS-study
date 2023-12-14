## 문제
~~~typescript
// JavaScript의 Array.includes 함수를 타입 시스템에서 구현하세요. 타입은 두 인수를 받고, true 또는 false를 반환해야 합니다.
type isPillarMen = Includes<['Kars', 'Esidisi', 'Wamuu', 'Santana'], 'Dio'> // expected to be `false
~~~

## 풀이
~~~typescipt
// T가 U에 포함되나요 ?  true : false
type Include<T, U> = T extends U ? true : false;

type isPillarMen = Include<['Kars', 'Esidisi', 'Wamuu', 'Santana'], 'Dio'> // expected to be `false`
~~~
