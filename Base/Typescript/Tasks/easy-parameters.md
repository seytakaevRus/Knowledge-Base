---
level: easy
tags:
  - "#infer-tricks"
  - typescript
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/03312-easy-parameters/README.md
---
## Subscription

Достать массив параметров из типа функции:

Пример кода:

```typescript
const foo = (arg1: string, arg2: number): void => {}

type FunctionParamsType = MyParameters<typeof foo> // [arg1: string, arg2: number]
```

---
## Solution

```typescript
const foo = (arg1: string, arg2: number): void => {}

type MyParameters<T extends (...args: any) => void> = T extends (...args: infer U) => void ? U : never;

type FunctionParamsType = MyParameters<typeof foo> // [arg1: string, arg2: number]
```

---
## Explanation

infer позволяет достать тип и записать его в переменную (в нашем случае U), а потом просто использовать переменную где угодно