---
level: easy
tags:
  - typescript
  - mapping
linkToTask: https://typehero.dev/challenge/day-4
---
## Subscription

*Разговор на кухне Северного полюса утром 4 декабря между Сантой и главой эльфа Бернардом*

*Бернард* Это ерунда, Крис. Я возглавляю эльфов более 200 лет. Вы не верите, что я знаю, о чем говорю?? НАМ НУЖНО РАСТИТЬ КОМАНДУ. У нас здесь штатная команда.
*Санта* Помните, мы здесь как семья; мы все приносим жертвы! Мы все еще стартап!
*Бернар* Клянусь тебе. Думаю, если я услышу от тебя еще одну шутку о культуре суеты... Думаю, моя маленькая эльфийская голова взорвется.
*Санта* Если ты сможешь придерживаться этого сейчас и помочь нам прожить еще один год, в будущем обязательно будут награды.
*Бернар* Не знаю, зачем я вообще спросил...
Очевидно, Бернард немного недоволен. Можете ли вы винить его? Увы, впереди еще много работы. Бернард уходит и бормочет про себя:
*Бернард* Думаю, пришло время решить еще одну бессмысленную задачу, связанную с TypeScript, не имеющую практического применения.

Бедный Бернард. Давайте поможем ему.

Сегодняшняя задача — создать тип (PresentDeliveryList), который принимает тип объекта в качестве входных данных, а затем заменяет значения каждого свойства на адрес. Мы заранее не знаем, какие свойства будут предоставлены, поэтому это должен быть универсальный тип. В противном случае Бернард, вероятно, просто скопировал бы это, чтобы прожить день.

Пример кода:

```typescript
type MixedBehaviorList = {
  john: { behavior: 'good' };
  jimmy: { behavior: 'bad' };
  sara: { behavior: 'good' };
  suzy: { behavior: 'good' };
  chris: { behavior: 'good' };
  penny: { behavior: 'bad' };
};

type test_MixedBehaviorTest_actual = PresentDeliveryList<MixedBehaviorList>;

type test_MixedBehaviorTest_expected = {
  john: Address;
  jimmy: Address;
  sara: Address;
  suzy: Address;
  chris: Address;
  penny: Address;
};
```

---
## Solution

```typescript
type Address = { address: string; city: string };
type PresentDeliveryList<T> = {
  [K in keyof T]: Address;
};
```

---
## Explanation

Мы перебираем все ключи в типе T и задаем в качестве ключа Address