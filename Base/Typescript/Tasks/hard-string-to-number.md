---
level: hard
tags:
  - "#infer-tricks"
  - typescript
  - template-literal
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00300-hard-string-to-number/README.md
---
## Subscription

Необходимо получить на вход строку и превратить ее в целое число как это делает parseInt.

Пример кода:
```typescript
type Test1 = ParseInt<'74,2'> //74
type Test2 = ParseInt<'3f'> //3
type Test3 = ParseInt<'hello'> //never
```

---
## Solution

```typescript
type ParseInt<T extends string, Temp extends string = ''> = T extends `${infer First}${infer Rest}` ? (
First extends `${infer N extends number}` ? (
N extends number ? ParseInt<Rest, `${Temp}${N}`> : Temp
) : TStringToNumber<Temp>
) : TStringToNumber<Temp>;

type TStringToNumber<T> = T extends `${infer N extends number}` ? N : never;

type Test1 = ParseInt<'74,2'> //74
type Test2 = ParseInt<'3f'> //3
type Test3 = ParseInt<'hello'> //never
```

---
## Explanation

Создаем тип ParseInt, который принимает дженериком нашу строку как T и Temp, который имеет значение по умолчанию как '' и служит просто как накопитель.

Далее мы при помощи шаблонных строки и infer достаем первый символ и остаток (First и Rest). Если символ First расширяет тип number, то мы рекурсивно вызываем нашу утилку ParseInt, и передаем туда уже Rest (чтобы убрать первый символ из текущей строки) и новое значение накопителя Temp (добавляем в конец First при помощи шаблонных строк: `${Temp}${N}`).

Также мы создаем вспомогательный тип TStringToNumber чтобы уже при выходе из рекурсии перевести строковое значение числа в численный вид.