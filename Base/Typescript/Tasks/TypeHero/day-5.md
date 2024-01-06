---
level: easy
tags:
  - typescript
linkToTask: https://typehero.dev/challenge/day-5
---
## Subscription

*Эльфы идут на работу рано утром 5 декабря. Над входом в цех висит табличка с надписью «Нас интересует страсть, а не только зарплата».*

Для мастерской Санты это был тяжелый год. Эльфы немного отстают от графика получения Сантой списка. Санте очень нравится видеть полный список имен задолго до сочельника, когда он доставляет подарки.

Обычно эльфы получают такие списки:

```ts
const badList = ["Tommy", "Trash", "Queen Blattaria", /* ... many more ... */]; const goodList = ["Jon", "David", "Captain Spectacular", /* ... many more
```

И они копируют все значения в тип TypeScript, чтобы предоставить Санте вот так։

```ts
type SantasList = [ "Tommy", "Trash", "Queen Blattaria", /* ... many more ... */ "Jon", "David", "Captain Spectacular", /* ... many more ... */ ];
```

Но есть проблема... В команде есть один эльф, Фримаген, который постоянно напоминает остальным, насколько невероятны его навыки Vim'а. Так он всегда делал это в прошлые годы. Однако в этом году Фримаген получил один из этих MacBook Pro без клавиши Escape, и скорость его работы с Vim резко снизилась. Нам нужно найти лучший способ передать Санте его список.

Давайте реализуем SantasList так, чтобы ему можно было передавать типы badList и GoodList, и он возвращал кортеж TypeScript со значениями обоих списков вместе.

Пример кода:

```typescript
const bads = ['tommy', 'trash'] as const;
const goods = ['bash', 'tru'] as const;

type test_0_actual = SantasList<typeof bads, typeof goods>;

type test_0_expected = ['tommy', 'trash', 'bash', 'tru'];
```

---
## Solution

```typescript
type SantasList<B extends readonly any[], G extends readonly any[]> = [...B, ...G];
```

---
## Explanation

Мы просто раскрываем список детей при помощи спред оператора