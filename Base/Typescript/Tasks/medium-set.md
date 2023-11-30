---
level: middle
tags:
  - "#infer-tricks"
  - typescript
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md
---
## Subscription

Необходимо получить на вход тупл и вернуть тупл без повторений.

Пример кода:

```typescript
type Res = Unique<[1, 1, 2, 2, 3, 3]>; // expected to be [1, 2, 3]
type Res1 = Unique<[1, 2, 3, 4, 4, 5, 6, 7]>; // expected to be [1, 2, 3, 4, 5, 6, 7]
type Res2 = Unique<[1, "a", 2, "b", 2, "a"]>; // expected to be [1, "a", 2, "b"]
type Res3 = Unique<[string, number, 1, "a", 1, string, 2, "b", 2, number]>; // expected to be [string, number, 1, "a", 2, "b"]
type Res4 = Unique<[unknown, unknown, any, any, never, never]>; // expected to be [unknown, any, never]
```

---
## Solution

```typescript
type IsEqual<T, K> = T extends K ? K extends T ? true : false : false;

type IsInclude<T, K> = K extends [infer F, ...infer Rest] ? IsEqual<F, T> extends true ? true : IsEnclude<T, Rest> : false;

type Unique<T, K extends any[] = []> = T extends [infer First, ...infer Rest] ? IsInclude<First, K> extends true ? Unique<Rest, [...K]> : Unique<Rest, [...K, First]> : K;

type Res = Unique<[1, 1, 2, 2, 3, 3]>; // expected to be [1, 2, 3]
type Res1 = Unique<[1, 2, 3, 4, 4, 5, 6, 7]>; // expected to be [1, 2, 3, 4, 5, 6, 7]
type Res2 = Unique<[1, "a", 2, "b", 2, "a"]>; // expected to be [1, "a", 2, "b"]
type Res3 = Unique<[string, number, 1, "a", 1, string, 2, "b", 2, number]>; // expected to be [string, number, 1, "a", 2, "b"]
type Res4 = Unique<[unknown, unknown, any, any, never, never]>; // expected to be [unknown, any, never]
```

---
## Explanation

Сначала мы сделали вспомогательный тип IsEqual, который возвращает true или false в зависимости от того эквивалентны ли типы T и K либо нет.

Затем мы сделали вспомогательный тип IsInclude, который проверяет, Входит ли K в тупл T или нет.

Потом мы делаем тип Unique, который дженериком принимает T и K (причем K мы используем как накопитель. Пользователю не нужно задавать этот тип, он по умолчанию равен []). При помощи infer достаем первый элемент First и остаток Rest. Проверяем есть ли уже наш First в накопителе K при помощи IsInclude. Если элемент есть в счетчике, то рекурсивно вызываем Unique, но уже без First (мы добиваемся этого, передавая Rest). Иначе мы также рекурсивно вызываем Unique передавая Rest, и добавляя в накопитель First (```Unique<Rest, [...K, First]>```). При выходе из рекурсии у нас возвращается накопитель K, который и содержит тупл без повторений.