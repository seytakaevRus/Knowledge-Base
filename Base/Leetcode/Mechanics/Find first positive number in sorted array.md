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
4. Если срединный элемент больше, чем `0`, сдвигаем правую границу;
5. Если срединный элемент меньше или равен `0`, сдвигаем левую границу;
6. После завершения цикла `left` будет больше, чем `right` на `1`, поэтому можем проверить, либо `right + 1`, либо `left`;
7. Если оно положительное, то возвращаем `right + 1` или `left`;
8. Если нет, то `-1`.

Нахождение последнего положительного числа в отсортированном массиве не рассматривается, так как этому условию соответствует последний элемент в массиве.

---
## Restrictions 

- Массив должен быть отсортирован.

---
## Advantages

- В отличие от линейного поиска, занимающего `O(n)`, этот поиск выполняется за `O(log n)`.

---
## Usage

```typescript
const findIndexFistPositiveNumber = (nums: number[]) => {
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    const middle = Math.floor((left + right) / 2);

    if (nums[middle] > 0) {
      right = middle - 1;
    } else if (nums[middle] <= 0) {
      left = middle + 1;
    }
  }

  if (nums[right + 1] > 0) {
		return right + 1;
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