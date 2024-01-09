---
level: easy
tags:
  - typescript
  - mapping
  - infer-tricks
linkToTask: https://typehero.dev/challenge/day-8
---
## Subscription

И снова Санта обратился с просьбой изменить код фильтрации детей. На этот раз он просто отправил электронное письмо всей команде инженеров (что совершенно не является процессом, но поскольку с Сантой иногда бывает сложно общаться, ни у кого еще не хватило смелости сказать ему об этом). Вот содержание письма.

```
POST /sendmail HTTP/1.1
Host: mail.hohoholdings.com
Content-Type: text/plain; charset=utf-8
Content-Length: 420

From: kris.kringle@hohoholdings.com
To: engineering@hohoholdings.com
Subject: Code Changes Needed

Привет, любимая команда!

Похоже, нам снова нужны изменения в коде!

1. иногда в одном списке бывают непослушные дети
1. оказывается, мне вообще-то не обязательно видеть в списке хороших детей
1. сегодня утром моя игра в гольф затянулась... так что, поскольку два других изменения были реализованы быстро, я уверен, что и это будет так же быстро, верно?!

- Крис Крингл
   «в мастерской Санты мы ценим лояльность превыше всего»
```

Пример кода:

```typescript
type SantasList = {
  naughty_tom: { address: '1 candy cane lane' };
  good_timmy: { address: '43 chocolate dr' };
  naughty_trash: { address: '637 starlight way' };
  naughty_candace: { address: '12 aurora' };
};
type test_wellBehaved_actual = RemoveNaughtyChildren<SantasList>;
type test_wellBehaved_expected = {
  good_timmy: { address: '43 chocolate dr' };
};
```

---
## Solution

```typescript
type RemoveNaughtyChildren<T> = {
  [K in keyof T as K extends `naughty_${infer U}` ? never : K]: T[K];
};
```

---
## Explanation

Мы перебираем все ключи в типе T и проверяем: Расширяет ли ключ из T шаблон `naughty_${infer U}`. Плюс тут мы применяем infer так как мы точно не знаем всю строку, а знаем только шаблон с префиксом naughty_.