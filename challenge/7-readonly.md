// in keyof 명령어로 key-value 받아올 수 있다.
type MyReadonly<T> = {
    readonly[P in keyof T]: T[P];
}

interface Todo {
  title: string
  description: string
}

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}
