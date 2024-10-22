---
tags:
  - leetcode
  - mechanic
---
## Description

Можно отсортировать массив `array`, подходящий под ограничения ниже, за `O(n + m)`, где `n` это длина `array`, а `m = Math.max(...array) - Math.min(...array)`. Делается это при помощи двух проходов.

Первый проход, пройтись по массиву и использовать значения в качестве индексов `O(n)` по времени.

Второй проход, удалить из массива все пустоты `O(m)`по времени.

---
## Restrictions 

- `array` должен быть из целочисленных значений;
- значения должны быть уникальными;
- при большой разнице между минимальным и максимальным числом, будет ухудшения по времени.

---
## Advantages

Сортировка методов `sort` занимает в среднем `O(n * log n)`. Метод выше позволяет это сократить до `O(n + m)`.

---
## Usage

```typescript
const arr = [81, 6, 3, 8, 9, 13, 64, 1];
const output = [];

arr.forEach((value) => {
	output[value] = value;
});

console.log(output.filter((value) => value !== null));
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