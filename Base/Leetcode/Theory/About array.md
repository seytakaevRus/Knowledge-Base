---
tags:
  - leetcode
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

---
## Operations with array

### Creating

```typescript
const array = [1, 6, 3, 10, 5];
```
### Getting length

`O(1)` по времени.

```typescript
console.log(array.length);
```
### Getting by index

`O(1)` по времени.

```typescript
console.log(array[0]);
console.log(array[2]);
console.log(array[5]);
```

### Searching element by value

`O(n)` по времени, так как нужно пройтись по всему массиву.
### Deleting from the end / Adding element to the end

`O(1)` по времени.

```typescript
array.pop();
array.push(5)
```

### Deleting from the start / Adding element to the start

`O(n)` по времени, так как после удаления/добавления первого элемента, оставшиеся элементы сдвигаются.

```typescript
array.shift();
array.unshift(5)
```
### Deleting from the random / Adding element to the random

`O(n)` по времени, так как после удаления/добавления с произвольного места/в произвольное место элементы справа будут сдвигаться.

```typescript
array.splice(index, 1);
array.splice(index, 0, 5);

