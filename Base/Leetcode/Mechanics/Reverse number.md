---
tags:
  - leetcode
  - mechanic
---
## Description

Алгоритм заключается в "разворачивании" числа без перевода его в строку.

Идея заключается в том, чтобы создать цикл, который будет выполняться, пока число больше, чем `0`.

На каждой итерации будет происходить следующее:
1. Получать последнюю цифра числа;
2. Наращивать число по формуле `value = value * 10 + lastDigit`;
3. Делить число на `10`, отбрасывая при этом дробную часть.

Рассмотрим на примере шаг `2`.

```typescript
let number = 123;
let value = 0;
// value = 0 * 10 + 3; number = 123;
// value = 3 * 10 + 2; number = 12;
// value = 32 * 10 + 1; number = 1;
```

---
## Usage

```typescript
const getReversedNumber = (x: number): boolean => {
  let temp = Math.abs(x);
  let value = 0;

  while (temp > 0) {
    const lastDigit = temp % 10;

    value = value * 10 + lastDigit;

    temp = Math.trunc(temp / 10);
  }

  if (x < 0) {
    return value * -1;
  } else {
    return value;
  }
};
```

---
## Application examples

```dataviewjs
const currentFileName = dv.current().file.name;

dv.table(["Task"], dv.pages('#leetcode')
	.filter(entity => {
		const linkArray = dv.array(entity.file.outlinks.values);
		return linkArray.some(link => link.path.includes(currentFileName));
	})
	.map(entity => {
		return [entity.file.link];
	}));
```