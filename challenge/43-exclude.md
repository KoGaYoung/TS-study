## 문제
~~~typescript
// Exclude 함수 사용하지 않고 MyExclude 타입 구현
type Result = MyExclude<'a' | 'b' | 'c', 'a'> // 'b' | 'c'
~~~

## 풀이 
~~~typescript
// Exclude 함수 사용 안하고 Exclude 기능 구현해보기
type MyExclude<T, U> = T extends U ? T : never;

type Result = MyExclude<'a' | 'b' | 'c', 'a'> // 'b' | 'c'

// type Result = Exclude<'a' | 'b' | 'c', 'a'>; // 'b' | 'c'
~~~
