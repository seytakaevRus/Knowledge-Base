---
level: easy
tags:
  - typescript
  - mapping
linkToTask: https://typehero.dev/challenge/day-7
---
## Subscription

*расшифровка вялого разговора в 23:23 между Сантой и Чиппером (одним из эльфов, который вчера работал над кодом фильтрации)*

*Санта* У нас большая проблема. Тот код, который вы мне дали вчера, не работает!

*Чиппер* что здесь не работает?

*Санта* Оказывается, мне нужны данные, отформатированные совсем по-другому! Все входы и выходы должны быть разными.

*Чиппер* ОК, похоже, требования изменились. ты спросил моего менеджера? уже поздно, и я расслабляюсь. Я играю в League of Legends.

*Санта* Это как RuneScape? Ну, в любом случае, не мог бы ты мне помочь в крайнем случае? Воспринимайте это как оплату своих взносов за лучшее положение позже!

*Чиппер* ок. Наверное.

*Санта* Замечательно! Вот входы и выходы! Этого должно быть достаточно для тебя! Хорошо, мне нужно немного отдохнуть перед завтрашней игрой в гольф. Отписываюсь!

Пример кода:

```typescript
type WellBehavedList = {
  tom: { address: '1 candy cane lane' };
  timmy: { address: '43 chocolate dr' };
  trash: { address: '637 starlight way' };
  candace: { address: '12 aurora' };
};

type test_wellBehaved_actual = AppendGood<WellBehavedList>;
type test_wellBehaved_expected = {
  good_tom: { address: '1 candy cane lane' };
  good_timmy: { address: '43 chocolate dr' };
  good_trash: { address: '637 starlight way' };
  good_candace: { address: '12 aurora' };
};
```

---
## Solution

```typescript
type AppendGood<T> = {[K in keyof T as `good_${string & K}`]: T[K]};
```

---
## Explanation

Мы перебираем все ключи в типе T и, используя шаблонные строки, добавляем в имя ключа префикс good_.