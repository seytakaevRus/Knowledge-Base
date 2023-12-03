---
tags:
  - leetcode
---
## Описание

`Бинарный поиск` - тип поискового алгоритма, который последовательно делит пополам заранее отсортированный массив данных, чтобы обнаружить нужный элемент. Его сложность по времени составляет `O(log n)`, где `n` - количество элементов в массиве.

Поиск работает только в упорядоченном списке, тогда он эффективнее линейного, сложность по времени которого составляет `O(n)`.

Если массив не упорядочен, то сначала его нужно отсортировать, если использовать `быструю сортировку`, то это займет `O(n log n)`, поэтому для коротких списков выгоднее использовать линейный поиск.

---
## Алгоритм

1. Если массив неотсортированный, то сортируем, направление сортировки (`возрастающее`, `убывающее`) не имеет значения;
2. Обозначаем левую и правую границу;
3. Делим его пополам и находим срединный индекс;
4. Сравниваем элемент посередине с заданным элементом:
	- Если они совпадают, то возвращаем срединный индекс;
	- Если заданный элемент больше, чем срединный, то левая граница равняется срединному индексу + `1`, повторяем пункт 3;
	- Если заданный элемент меньше, чем срединный, то правая граница равняется срединному индексу - `1`, повторяем пункт 3.

---
## Реализация

### Итеративная

```typescript
const search = (nums: number[], target: number): number => {
	let leftPointer = 0;
	let rightPointer = nums.length - 1;

	while (leftPointer <= rightPointer) {
		const middlePointer = Math.floor((leftPointer + rightPointer) / 2);

		if (nums[middlePointer] === target) {
			return middlePointer;
		}

		if (nums[middlePointer] < target) {
			leftPointer = middlePointer + 1;
		} else if (target < nums[middlePointer]) {
			rightPointer = middlePointer - 1;
		}
	}

	return -1;
};
```

### Рекурсивная

```typescript
const search = (nums: number[], target: number, leftIndex: number = 0, rightIndex: number = nums.length - 1) => {
  if (leftIndex > rightIndex) {
    return -1;
  }

  const middleIndex = Math.floor((leftIndex + rightIndex) / 2);

  if (nums[middleIndex] === target) {
    return middleIndex;
  }

  if (nums[middleIndex] < target) {
    return search(nums, target, middleIndex + 1, rightIndex);
  } else if (target < nums[middleIndex]) {
    return search(nums, target, leftIndex, middleIndex - 1);
  }
}
```