---
level: middle
tags:
  - "#infer-tricks"
  - typescript
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00015-medium-last/README.md
---
## Subscription

Необходимо достать хвост (последний элемент) из тупла:

For example:
```typescript
type arr1 = ['a', 'b', 'c']
type arr2 = [3, 2, 1]

type tail1 = Last<arr1> // expected to be 'c'
type tail2 = Last<arr2> // expected to be 1
```

---
## Solution

(мое не оптимальное решение, но довольно интересное через infer и рекурсию):
```typescript
type arr1 = ['a', 'b', 'c']
type arr2 = [3, 2, 1]

type IsEqual<T, K> = T extends K ? K extends T ? true : false : false;//Вспомогательный тип для проверки на эквивалентность

type Last<T extends any[]> = T extends [infer First, ...infer Rest] ? (
    IsEqual<Rest['length'], 1> extends true ? (//Проверяю что в остатке всего лишь один элемент
            Rest extends [infer Element] ? Element : never //Если элемент в остатке остался один, то достаю его при помощи infer (по сути это условия выхода из рекурсии)
        ) : Last<Rest>
) : never;

type tail1 = Last<arr1> // expected to be 'c'
type tail2 = Last<arr2> // expected to be 1
```

Более короткое решение от какого-то китайца из гитхаба:
```typescript
type EaserLast<T extends any[]> = [any, ...T][T["length"]];
```
## Explanation

Попытался все расписать в комментариях в решении