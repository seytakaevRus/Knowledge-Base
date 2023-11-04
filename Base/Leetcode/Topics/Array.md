---
tags:
  - leetcode
  - index
---
## Description

`Массив` - базовая структура для последовательного хранения элементов. Элементы в памяти хранятся рядом с ячейках памяти. Доступ к элементам возможен по индексам.

Массив может одномерным или многомерным.

Одномерный массив:
![[one-dimensional-array.png]]

В массиве `A` выше есть 6 элементов. Он имеет длину 6, чтобы обратиться к первому элементу используется запись `A[0]`, к последнему `A[5]` или `A[A.length - 1]`.

По расширяемости есть:
1. `Статический массив` - массив, у которого размер не может быть увеличен позже;
2. `Динамический массив` -  массив, у которого размер может быть изменен.

Но в `JavaScript` массивы динамические, да и размер не нужно указывать заранее.

## Operations

```typescript
// 1. Создание одномерного массива.
const array = [1, 6, 3, 10, 5];

// 2. Получение длины.
console.log(array.length);

// 3. Получение первого элемента.
console.log(array[0]);

// 4. Перебор одномерного массива.
for (let i = 0; i < array.length; i++) {
	console.log(array[i]);
}

// 5. Модифицирование элемента.
array[3] = 99;

// 6. Сортировка массива.
array.sort((a, b) => a - b);
```

## Tasks

```dataviewjs
const mainTopic = "array";

const LEVELS_CODES = {
	'elementary': 0,
	'easy': 1,
	'middle': 2,
	'hard': 3,
};

dv.table(["Task", "Additional topics", "Level"], dv.pages('#leetcode')
	.sort(entity => LEVELS_CODES[entity.level])
	.filter(entity => {
		return dv
			.array(entity.topics)
			.includes('array');
	})
	.map(entity => {
		const topics = entity.topics.filter((topic) => topic !== mainTopic);

		return [entity.file.link, topics, entity.level];
	}));
```