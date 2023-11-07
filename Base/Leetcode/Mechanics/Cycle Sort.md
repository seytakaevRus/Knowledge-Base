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

- не выделяет дополнительную память при сортировке, так как сортирует массив на месте;
- просто в реализации.

---
## Usage

`index` не меняется, пока на значения `index` не встанет число со значением `array[index] + 1` при `[1..n]` и со значением `array[index]` при `[0..n]`. Чтобы это произошло меняются местами элементы с индексом `correctIndex` и `index`.

Сложность по памяти: `O(1)`.
Сложность по времени: `O(n)`.

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

