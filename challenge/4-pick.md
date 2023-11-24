## 문제
~~~
TypeScript에서 내장된 Pick<T, K> 제네릭 타입을 사용하지 않고 MyPick 유틸리티 타입을 구현하는 방법
T에서 속성 K 세트를 선택하여 유형을 구성합니다.

For example:
~~~
~~~typescript
interface Todo {
  title: string
  description: string
  completed: boolean
}

type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
}
~~~

## 해결
~~~typescript
// K(title, completed)는 T(Todo)의 부분집합입니다 (K는 유니온타입)
MyPick<T, K extends keyof T> {
  [key in K]: T[key]; // K의 각 속성들에 대해 반복하며 T에서 선택
}

interface Todo {
  title: string
  completed: boolean
  description: string
}

// T와 K라는 제너릭 매개변수를 가짐
type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
}
~~~
