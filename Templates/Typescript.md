---
level: easy
tags:
  - "#typescript"
linkToTask: https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md
---
## Subscription

Implement some type

Пример кода:
```typescript
type MyType = any;
```

---
## Solution

```typescript
type MyType<T> = T;
```

---
## Explanation

Краткое описание