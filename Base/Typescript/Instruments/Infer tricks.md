#typescript
infer позволяет достать тип из выражения и записать его в переменную для дальнейшего использования. Пример с написанием кастомной утилитой MyAwaited:

```typescript
type MyAwaited<T> = T extends Promise<infer U> ? U : never;

type Test = MyAwaited<Promise<string>>; //Вернет string
```

Благодаря infer и рекурсии можно проворачивать разные трюки с перебором туплов. как, например в задаче [[medium-set]].

Более простой пример с infer и туплом:
```typescript
type FirstElement<T extends any[]> = T extends [infer First, ... infer Rest] ? First : never;
type Test = FirstElement<[1,2,3]>;//Вернет 1
```