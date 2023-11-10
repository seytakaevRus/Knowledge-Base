---
level: easy
tags:
  - "#typescript"
  - "#mapping"
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00007-easy-readonly/README.md
---
## Subscription

Реализовать собственный `Readonly<T>` дженерик без использования утилок ts.

Утилка должна принимать тип T и вернуть новый тип, который имеет все те же поля что и тип T, только с модификаторами readonly.

For example:
```typescript
interface Todo {
  title: string
  description: string
}

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}

todo.title = "Hello" // Error: cannot reassign a readonly property
todo.description = "barFoo" // Error: cannot reassign a readonly property
```
## Solution
```typescript
interface Todo {
  title: string
  description: string
}

type MyReadonly<T> = {readonly [Key in keyof T]: T[Key]};

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}
```
## Explanation

В данной задаче используется оператор keyof, возвращающий юнион ключей типа (в нашем случае Todo). При помощи оператора in мы просто мапим все ключи типа Todo и добавляем модификатор readonly. Если у нас обратная задача (удалить модификатор), то это моно сделать, добавив знак `-` перед модификатором.

Например:
```typescript
interface Todo {
  readonly title: string
  readonly description: string
}

type MyOptional<T> = {-readonly [Key in keyof T]: T[Key]};

const todo: MyReadonly<Todo> = {
  title: "Hey",
  description: "foobar"
}
todo.title = "Hello" // Все будет нормально, так как readonly удалился
```