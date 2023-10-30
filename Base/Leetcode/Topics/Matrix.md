---
tags:
  - leetcode
linkToIndex: "[[Studying on leetcode]]"
---
## Description

`Матрица` - это двумерный массив.

Двумерный массив:
![[two-dimensional-array.png]]

В массиве выше есть 5 колонок и 3 столбца, в коде это выглядит так:

```typescript
const matrix = [[1, 4, 7, 3, 6], [2, -5, 0, 15, 10], [8, 9, 11, 12, 20];
```

Чтобы обратиться к элементу используется запись вида `matrix[i][j]`, где `i` - номер строки, а `j` - номер столбца.
## Operations

```typescript
// 1. Создание двумерного массива.
const array = [[1, 2, 3], [7, 5, 1], [4, 5, 7]]

// 2. Получение количество строк в матрице.
console.log(array.length);
// Получение количество колонок в матрице.
console.log(array[0].length);

// 3. Получение первого элемента.
console.log(array[0][0]);

// 4.Перебор двумерного массива.
for (let i = 0; i < array.length; i++) {
	for (let j = 0; j < array[i].length; j++) {
		console.log(array[i][j]);
	}
}

// 5. Модифицирование элемента.
array2[1][2] = 99;
```

## Tasks

```dataviewjs
dv.table(["Task"], dv.pages('#leetcode')
	.filter((entity) => dv.array(entity.topics).includes('matrix'))
	.map((entity) => [entity.file.link]));
```