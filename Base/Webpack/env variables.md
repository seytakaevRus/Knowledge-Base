---
tags:
  - webpack
---

Самый простой способ получить значения переменных среды:

```js
module.exports = (env) => {
    const { mode } = env; //Достаем env переменные
    return {
        context: path.resolve(__dirname, 'src'),
        mode,//режим
        entry: {
            main: './index.js',
        },
        output: {
            filename: '[name].[contenthash].js',
            path: path.resolve(__dirname, 'dist'),
        },
   }
 }
```

Мы можем вернуть в webpack.config.js файле конфигурацию не в виде объекта, а в виде функции, которая принимает env как параметр.

Теперь мы можем создать скрипт в package.json:

```js
...
  "scripts": {
	  ...
    "build": "webpack --mode production",
    ...
  },
...
```
