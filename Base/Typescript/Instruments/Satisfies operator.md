---
tags:
  - "#typescript"
---

Посмотрим на этот код:

```typescript
type ErrorData = {
	errorOrCode: number | string;
};

const errorData: ErrorData = {
	errorOrCode: 'TIMEOUTERROR',
};

errorData.errorOrCode.toLowerCase();// Property 'toLowerCase' does not exist on type 'string | number'. Property 'toLowerCase' does not exist on type 'number'
```

Как видно, возникает ошибка. Она возникает потому что errorOrCode может быть типа number, а у number нет метода toLowerCase. Но при этом мы хотим и рыбку съесть и косточкой не подавиться (и типизацию сохранить и от ошибки избавиться).

В этом нам поможет оператор satisfies:

```typescript
type ErrorData = {
	errorOrCode: number | string;
};

const errorData = {
	errorOrCode: 'TIMEOUTERROR',
} satisfies ErrorData;

errorData.errorOrCode.toLowerCase();// Ok
```

Теперь typescript учитывает то что метод toLowerCase имеется у типа string, а строковый тип удовлетворяет нашей типизации.