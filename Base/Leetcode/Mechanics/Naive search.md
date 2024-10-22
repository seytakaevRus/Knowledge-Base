---
tags:
  - leetcode
  - mechanic
---
## Description

Одним из самых простых алгоритмов поиска подстроки в строке является "Наивный алгоритм".

Он состоит из двух вложенных циклов:
1. первый перебирает в строке от `0` до `длина строки - длина подстроки`;
2. второй перебирает от `0` до `длины подстроки`.

Во втором цикле идет сравнение буквы в строке под индексом `i + j`, где `i` - индекс символа в строке, а `j` - в подстроке, с буквой по индексом `j` в подстроке.

Если символы неравны, то цикл прерывается, иначе идет сравнения дальше до тех пор, пока они будут не равны или цикл не прервется. После цикла сравнивается индекс `j` и длина подстроки. При равенстве индекс `i` записывается в `result`.

В конце возвращается `result`.

---
## Advantages

1. `O(1)` по памяти;
2. Простая и понятная реализация;
3. Приемлемое время работы на практике. Благодаря этому алгоритм применяется, например, в браузерах и текстовых редакторах (при использовании `Ctrl + F`), потому что  обычно паттерн, который нужно найти, очень короткий по сравнению с самим текстом.

---
## Disadvantage

1. По времени требует `O((n - m) * m)` операций, поэтому алгоритм работает медленно, когда длина патерна достаточно велика.

---
## Usage

```typescript
const findPatternInString = (string: string, pattern: string): number[] => {
  const result: number[] = [];

  for (let i = 0; i <= string.length - pattern.length; i++) {
    let j;

    for (j = 0; j < pattern.length; j++) {
      if (string[i + j] !== pattern[j]) break;
    }

    if (j === pattern.length) {
      result.push(i);
    }
  }

  return result;
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