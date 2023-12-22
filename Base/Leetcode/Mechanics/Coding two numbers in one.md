---
tags:
  - "#leetcode"
  - mechanic
---
## Description

Предположим, есть 2 числа `a` и `b` и их нужно закодировать в одно число. Самый простой способ это сделать через формулу `encoded = a + n * b`, где `n` - произвольное число, которое должно быть больше `a` и `b`. После кодировки первоначальные числа можно получить по формуле:
1. `a = encoded % n`;
2. `b = Math.floor(encoded / n)` (целочисленное деление).

---
## Restrictions 

- `a` и `b` должны быть одного знака, либо оба положительные, либо оба отрицательные;
- `n` должны быть больше `a` и `b`.

---
## Advantages

Благодаря такому кодированию, можно хранить 2 числа в одном, а значит модифицировать массив, который подается на вход, как пример, сократить сложно по времени с `O(n)` до `O(1)`.

---
## Usage

Каждое число кодируется при помощи следующего. Последнее число кодируется через первое.

`% N` нужно, чтобы из модифицированного значения получить изначальное.

```typescript
const array = [1, 2, 3, 4, 5];
const N = 10;

// Coding
for (let i = 0; i < array.length; i++) {
    array[i] = array[i] + N * (array[(i + 1) % array.length] % N);
}

// Decoding
for (let i = 0; i < array.length; i++) {
    array[i] = Math.floor(array[i] / N);
}
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

