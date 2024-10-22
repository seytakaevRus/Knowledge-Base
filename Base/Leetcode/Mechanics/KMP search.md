---
tags:
  - leetcode
  - mechanic
---
## Description

Алгоритм поиска Кнута-Мориса-Пратта это алгоритм поиска подстроки в строке.

Состоит из двух этапов:
1. Формирование массива `π`, используемого при сдвиге паттерна вдоль строки;
2. Поиск паттерна в строке.

Чтобы создать массив `π` нужно `префикс-функция`. 

`Префикс-функция` от строки `s` (строка паттерн)  эта функция, которая возвращает массив `π`, где `π[i]` равен длине максимального префикса строки `s[0..i]`, совпадающего с её суффиксом.

`Префикс` - символы, которые стоят в начале паттерна.
`Суффикс` - символы, которые стоят в конце.

В алгоритме будут интересовать `префиксы` и `суффиксы` одинаковой длины. И нет смысла рассматривать `префиксы` и `суффиксы`, которые равны длина паттерна, тогда в алгоритме нет смысла. Они также могут пересекаться.

Рассмотрим пример создание массива `π` на основе паттерна ниже.

![[KMP-search-image1.png]]

Для первого символа любого паттерна функция вернет 0, `π[0] = 0`.

Для второго символа рассматриваются префикс `a` и суффикс `b`, так как они должны быть одинаковой длины, так как префикс и суффикс не совпадают, то функция возвращает 0, `π[1] = 0`.

Для третьего символа сначала рассматриваются префикс и суффикс, которые равны длине 1, то есть `a` и `c`, они не совпадают, поэтому рассматриваются, у которых длина равна 2, `ab` и `bc` (префикс и суффикс могут пересекаться и рассматриваются в диапазоне от `[0..i]`), они также не равны, поэтому `π[2] = 0`.

Для четвертого символа, сначала длина 1, `a` под индексом 0 и `a` под индексом 3, они совпадают, потом префикс и суффикс длины 2, `ab` и `ca`, они не совпадают, далее длиной 3, `abc` и `bca` они также не совпадают, поэтому максимальная длина равна 1, `π[3] = 1`.

Для пятого символа префикс и суффикс длиной 1 (`a` и `b`) не совпадают, а вот длиной 2, `ab` и `ab` совпадают, длиной 3, `abc` и `cab` не совпадают и длиной 4, `abca` и `bcab` не совпадают, поэтому максимальная длина это 2, `π[4] = 2`.

Остальные символы подвержены такой же логики

В конце массив `π` будет равен `[0, 0, 0, 1, 2, 3, 0]`.

Есть две реализации префиксной функции. Первая это "наивная".

```typescript
const prefixFunction = (pattern: string) => {
  const result: number[] = [0];

  for (let i = 1; i < pattern.length; i += 1) {
    let coefficient = 0;

    let prefix = "";
    let suffix = "";

    for (let j = 0; j < i; j += 1) {
      prefix += pattern[j];
      suffix = pattern[i - j] + suffix;

      if (prefix === suffix) {
        coefficient = Math.max(coefficient, prefix.length)
      }
    }

    result[i] = coefficient;
  }

  return result;
}
```

Этот алгоритм пока что работает за `O(n^3)`, где первый цикл за проходит по `pattern`, второй цикл проходит с `0` до `j`, а третий это сравнение двух строк.

Вторая реализация оптимальнее первой, благодаря следующим наблюдениям:
1. p<sub>i+1</sub> максимум на единицу превосходит p<sub>i</sub>;
2. p<sub>i + 1</sub> = p<sub>i</sub> + 1 в том и только в том случае, когда s<sub>p<sub>i</sub></sub> = s<sub>i + 1</sub>, в таком случае можно просто обновить p<sub>i + 1</sub> и пойти дальше.

Например, в строке `aabaataabaa` при `i = 10` максимальный префикс будет равен `5`, если следующим символом будет `t`, то p<sub>11</sub> = p<sub>10</sub> + 1 = 6.

---
## Advantages

---
## Usage

```js
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
