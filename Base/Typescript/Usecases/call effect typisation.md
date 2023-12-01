---
tags:
  - "#typescript"
  - "#ts_usecases"
---


В сагах часто используется эффект call чтобы зафетчить данные из бэкенда. На своем проекте я решил написать обертку для эффекта call (я назвал ее callTs), которая возвращает правильный тип данных, возвращаемых с бэкенда.

Вот как выглядит типизация с обычным эффектом call։

![[typescript-usecases1.png]]

А вот как выглядит функция accessTokenRequest (не обращаем внимание на any :) ):

![[typescript-usecases2.png]]

Вот как выглядит наша обёртка callTs:

```ts
import { CallEffect, SagaReturnType, call } from "redux-saga/effects"

export function* callTs<T extends (...args: any[]) => any>(func: T, ...args: Parameters<T>): Generator<
    CallEffect<SagaReturnType<T>>,
    ReturnType<T> extends Promise<infer U> ? U : any,
    unknown
> {
    return (yield call(func, ...args)) as any;
}
```

Суть обёртки состоит в том, что мы подхватываем возвращаемый тип функции при помощи ```
ReturnType<T>```. Далее при помощи конструкции ```T extends Promise<infer U> ? U: any```
мы проверяем: является ли возвращаемый тип промисом? Если да, то при помощи infer достаем из промиса тип (который является типом данных, возвращаемых с бэка). Если мы имеем дело не с промисом, то просто возвращаем any (то есть в данном случае наш callTs будет работать как обычный call).

Также нужно учесть что при использовании callTs нам нужно использовать не просто ``yield``, а `yield*`. Это связано с тем что мы имеем дело с композицией генераторов, так как callTs - сам является функцией-генератором.

Что мы имеем по итогу:

![[typescript-usecases3.png]]