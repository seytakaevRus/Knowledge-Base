---
level: middle
tags:
  - "#typescript"
  - "#remaping"
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/02852-medium-omitbytype/README.md
---
## Subscription

Убрать из типа T все ключи, которые имеют примитивный тип K

Пример кода:

```typescript
type OmitBoolean = OmitByType<{
  name: string
  count: number
  isReadonly: boolean
  isEnable: boolean
}, boolean> // { name: string; count: number }
```

---
## Solution

```typescript
type OmitByType<T, K> = {
     [Key in keyof T as T[Key] extends K ? never : Key]: T[Key]
}

type OmitBoolean = OmitByType<{
   name: string
   count: number
   isReadonly: boolean
   isEnable: boolean
}, boolean> // { name: string; count: number }
```

---
## Explanation

Тут мы используем ремапинг. Это делается при помощи оператора as. Мапим ключи из типа T и на каждой итерации берем T[Key]. Если T[Key] расширяет наш примитивный тип K, то убераем его, возвращая never, иначе - просто возвращаем наш ключ