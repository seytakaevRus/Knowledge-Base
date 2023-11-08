---
tags:
  - leetcode
  - mechanic
  - draft
---

TODO: Решить задачи из этого списка
https://leetcode.com/discuss/study-guide/2958275/cyclic-sort-important-pattern

## Description

Циклическая сортировка - алгоритм, который сортирует массив целых чисел на месте. Подходит для массивов, чьи элементы расположены в диапазоне `[0..n]` или `[1..n]`, в последнем случае для вычисления `correntIndex` добавляется `-1`.

Идея состоит в том, что бы по значению элемента определять, на какой индекс нужно ставить элемент.

---
## Restrictions 

- массив со значениями должен быть в диапазоне `[1..n]`;
- элементы в массиве должны быть уникальными;
- важно сохранение порядка для элементов с одинаковыми значениями.

---
## Advantages

- не выделяет дополнительную память при сортировке, так как сортирует массив на месте, поэтому сложность по памяти `O(1)`;
- сложность по времени составляет `O(n)`.

---
## Usage

`index` не меняется, пока на значения `index` не встанет число со значением `array[index] + 1` при `[1..n]` и со значением `array[index]` при `[0..n]`. Чтобы это произошло меняются местами элементы с индексом `correctIndex` и `index`.

```typescript
const swap = (array: number[], index1: number, index2: number) => {
  const temp = array[index1];

  array[index1] = array[index2];
  array[index2] = temp;
}

const cyclicSort = (array: number[]) => {
  let index = 0;

  while (index < array.length) {
	  // при [1..n] добавляем -1
	  // при [0..n] -1 не нужен
    const correctIndex = array[index] - 1;

    if (correctIndex !== index) {
      swap(array, correctIndex, index)
    } else {
      index += 1;
    }
  }
};
```

В ограничениях написано, что элементы должны быть уникальные, но это не совсем так. Даже при наличии дублей, можно использовать сортировку, но она будет больше использована для поиска дубликатов, а не для сортировки.

Но перед испытанием нужно изменить условие в `if`, а именно `correctIndex !== index`, так как будут дубликаты, то полагаться на индексы больше нельзя, иначе цикл будет бесконечным, поэтому сравнивать нужно значения у элементов `array[correctIndex] !== array[index]`.

```typescript
const cyclicSort = (array: number[]) => {
  let index = 0;

  while (index < array.length) {
	  // при [1..n] добавляем -1
	  // при [0..n] -1 не нужен
    const correctIndex = array[index] - 1;

    if (array[correctIndex] !== array[index]) {
      swap(array, correctIndex, index)
    } else {
      index += 1;
    }
  }
};
```

Есть массив с дубликатами ниже.

```typescript
const array = [1, 1, 1, 2, 3, 4, 5];
```

Если его прогнать через функцию `cyclicSort`, то массив станет таким:

```typescript
[1, 2, 3, 4, 5, 1, 1]
```

Уникальные элементы встали на свои места, а дубликаты просто сместились.

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
