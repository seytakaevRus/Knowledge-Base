---
level: easy
tags:
  - typescript
  - mapping
linkToTask: https://typehero.dev/challenge/day-12
---
## Subscription

Как бы странно это ни звучало... Санта учился в колледже у кого-то, кто работает в большой сетевой компании Кремниевой долины. Они были друзьями уже много лет. Настолько, что в 2023 году Санта настаивал на совете директоров мастерской, пока они не утвердили бюджет на установку Wi-Fi на территории всего кампуса. Таким образом, Санта сможет просматривать TikTok, переходя от здания к зданию по кампусу.

Но после всех этих думскроллеров Санта понял, что заблудился в ёлочном лесу! Для его поиска была отправлена поисковая группа эльфов, но ему нужно дать им больше информации о том, где он находится среди деревьев.

FindSanta — это тип, который принимает кортеж в качестве единственного аргумента и возвращает индекс местоположения Санта. Давайте поможем Санте вернуться к тому, что у него получается лучше всего: вдохновлять на лидерство.

примечание: никогда не возвращается, если Санту нельзя найти среди деревьев.

Пример кода:

```typescript
type Forest0 = ['🎅🏼', '🎄', '🎄', '🎄'];
type test_0_actual = FindSanta<Forest0>;

type test_0_expected = 0;
type test_0 = Expect<Equal<test_0_expected, test_0_actual>>;

type Forest1 = ['🎄', '🎅🏼', '🎄', '🎄', '🎄', '🎄'];
type test_1_actual = FindSanta<Forest1>;

type test_1_expected = 1;
type test_1 = Expect<Equal<test_1_expected, test_1_actual>>;
```

---
## Solution

```typescript
type FindSanta<T extends Array<any>, K extends 1[] = []> = T extends [infer First, ...infer Rest] ? (
  First extends '🎅🏼' ? K['length'] : FindSanta<Rest, [...K, 1]>
) : never;
```

---
## Explanation

Заводим счетчик K, который является массивом единичек. Каждый раз, когда текущий элемент не является Дедом Морозом, мы вызываем рекурсивно FindSanta с остатком Rest и с тем же массивом K, только с добавлением единички. Нам нужен этот K чтобы потом получить количество этих единичек, которое и будет равзяться индексу нахождения Деда Мороза, когда в нашей рекурсии наступит условие выхода из рекурсии.