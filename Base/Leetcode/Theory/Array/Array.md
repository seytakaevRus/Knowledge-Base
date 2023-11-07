---
tags:
  - leetcode
  - index
---
## Описание

`Массив` - базовая структура для последовательного хранения элементов. Элементы в памяти хранятся рядом с ячейках памяти. Доступ к элементам возможен по индексам.

Массив может одномерным или многомерным.

Одномерный массив:
![[one-dimensional-array.png]]

В массиве `A` выше есть 6 элементов. Он имеет длину 6, чтобы обратиться к первому элементу используется запись `A[0]`, к последнему `A[5]` или `A[A.length - 1]`.

По расширяемости есть:
1. `Статический массив` - массив, у которого размер не может быть увеличен позже;
2. `Динамический массив` -  массив, у которого размер может быть изменен.

Но в `JavaScript` массивы динамические, да и размер не нужно указывать заранее.
## Операции с массивом

### Создание

```typescript
const array = [1, 6, 3, 10, 5];
```
### Получение длины

`O(1)` по времени.

```typescript
console.log(array.length);
```
### Получение элемента по индексу

`O(1)` по времени.

```typescript
console.log(array[0]);
console.log(array[2]);
console.log(array[5]);
```

### Поиск элемента в массиве

`O(n)` по времени, так как нужно пройтись по всему массиву.
### Удаление/добавление элемента с конца/в конец

`O(1)` по времени.

```typescript
array.pop();
array.push(5)
```

### Удаление/добавление элемента с начала/в начало

`O(n)` по времени, так как после удаления/добавления первого элемента, оставшиеся элементы сдвигаются.

```typescript
array.shift();
array.unshift(5)
```
### Удаление/добавление элементов с произвольного места/в произвольное место

`O(n)` по времени, так как после удаления/добавления с произвольного места/в произвольное место элементы справа будут сдвигаться.

```typescript
array.splice(index, 1);
array.splice(index, 0, 5);
``` 
## Задачи

```dataviewjs
const mainTopic = dv.current().file.name.toLowerCase();

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