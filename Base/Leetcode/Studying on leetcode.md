---
tags:
  - index
  - leetcode
---
Здесь можно найти задачи, которые я решал на https://leetcode.com.

## Обозначения

1. `#leetcode` - задачи, взяты из `Leetcode`;
2. `#several-solutions` - задачи, где используются несколько подходов, отличающиеся по времени и/или памяти;
3. `#new-mechanism` - задачи, в которых используется новая механика, способная уменьшить время и/или количество памяти;
4. `#repeat` - задача, которую нужно повторить в решении;
5. `#draft` - задача, к которой нужно вернуться и доработать описание.

```dataview
TABLE level AS "Level"
FROM #leetcode
WHERE level = "super easy"
SORT id ASC
```

```dataview
TABLE level AS "Level"
FROM #leetcode
WHERE level = "easy"
```

```dataview
TABLE level AS "Level"
FROM #leetcode
WHERE level = "middle"
```