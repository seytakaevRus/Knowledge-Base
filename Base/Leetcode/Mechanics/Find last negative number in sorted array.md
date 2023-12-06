---
tags:
  - leetcode
  - mechanic
---
## Description

Алгоритм дает возможность находить первое положительное число в отсортированном массиве. В алгоритме используется [[About binary search|бинарный поиск]].

1. Заводим переменную `left` и `right`;
2. Пока `left <= right` выполняется цикл;
3. Вычисляем индекс срединного элемента;
4. Если срединный элемент больше или равен `0`, сдвигаем правую границу;
5. Если срединный элемент меньше, чем `0`, сдвигаем левую границу;
6. После завершения цикла `left` будет больше, чем `right` на `1`, поэтому можем проверить, либо `right`, либо `left - 1`;
7. Если оно положительное, то возвращаем `right` или `left - 1`;
8. Если нет, то `-1`.

Нахождение первого отрицательного числа в отсортированном массиве не рассматривается, так как этому условию соответствует первый элемент в массиве.

---
## Restrictions 

- Массив должен быть отсортирован.

---
## Advantages

- В отличие от линейного поиска, занимающего `O(n)`, этот поиск выполняется за `O(log n)`.

---
## Usage

```typescript
const findIndexLastNegativeNumber = (nums: number[]) => {
	let left = 0;
	let right = nums.length - 1;

	while (left <= right) {
		const middle = Math.floor((left + right) / 2);

		if (nums[middle] >= 0) {
			right = middle - 1;
		} else if (nums[middle] < 0) {
			left = middle + 1;
		}
	}

	if (nums[left - 1] < 0) {
		return left - 1;
	}

	return -1;
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