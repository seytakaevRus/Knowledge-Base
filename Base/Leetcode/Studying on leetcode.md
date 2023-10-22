---
tags:
  - index
  - leetcode
---
Здесь можно найти задачи, которые я решал на https://leetcode.com.

## Обозначение
1. `#leetcode` - задачи, взяты из `Leetcode`;
2. `#several-solutions` - задачи, где используются несколько подходов, отличающиеся по времени и/или памяти.

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