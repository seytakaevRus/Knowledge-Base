---
level: easy
tags:
  - "#typescript"
  - "#mapping"
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md
---
## Subscription

Реализовать`Pick<T, K>` без использования утилок ts.

Нужно создать тип с использованием дженерик типов `K` от T.

For example:
```typescript
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
```
## Solution
```typescript
interface Todo {
  title: string
  description: string
  completed: boolean
}

type MyPick<T, K extends keyof T> = {[Key in K]: T[K]};

type TodoPreview = MyPick<Todo, 'title' | 'completed'>

const todo: TodoPreview = {
    title: 'Clean room',
    completed: false,
}
```
## Explanation

Typescript поддерживает маппинг типов. В данном случае мы просто проходимся по элементам юниона при помощи оператора in