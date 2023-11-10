---
level: middle
tags:
  - "#typescript"
  - "#remaping"
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00003-medium-omit/README.md
---
## Subscription

Implement the built-in `Omit<T, K>` generic without using it.

Constructs a type by picking all properties from `T` and then removing `K`

For example:
```typescript
interface Todo {
  title: string
  description: string
  completed: boolean
}

type TodoPreview = MyOmit<Todo, 'description' | 'title'>

const todo: TodoPreview = {
  completed: false,
}
```
## Solution
```typescript
interface Todo {
  title: string
  description: string
  completed: boolean
}

type MyOmit<T, K extends keyof T> = {[Key in keyof T as Key extends K ? never : Key]: T[Key]};

type TodoPreview = MyOmit<Todo, 'description' | 'title'>

const todo: TodoPreview = {
  completed: false,
}
```
## Explanation

Тут мы используем ремапинг. Ремапинг позволяет замапить ключи из типа Todo и, после оператора as, возвращать на каждой итерации мапинга какое-то определенное значение. В нашем случае мы просто возвращает Key. Дальше мы проверяем на каждой итерации: расширяет ли ключ из типа T юнион K. Если расширяет то мы убераем его, возвращая never, иначе - мы просто возвращаем ключ