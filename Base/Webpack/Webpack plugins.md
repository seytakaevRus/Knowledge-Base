---
tags:
  - webpack
---

Webpack поддерживает расширение при помощи плагинов. Плагины можно подключить следующим способом:

```js
module.exports = {
    return {
        ...
        plugins: [
            new HTMLWebpackPlugin({// Плагин для копирования html файла в папку dist
                title: "Webpack project",
                template: './index.html'
            }),
            new CleanWebpackPlugin(),// Плагин для очистки папки dist
        ],
        ...
    }
}
```

Плагины имеют доступ к хукам жизненного цикла сборки и выполнять действия на том или ином жизненном цикле. Мы можем написать свой кастомный плагин, который выводит информацию в консоль о стадии выполнения сборки:

```js
class MyCustomPlugin {
  apply(compiler) {
    console.log('Начало...');
    compiler.hooks.environment.tap('My Plugin', (stats) => console.log('Инициализация окружения...'));
    compiler.hooks.afterEnvironment.tap('My Plugin', (stats) => console.log('Инициализация окружения [done]'));
    compiler.hooks.entryOption.tap('My Plugin', (stats) => console.log('Загрузка плагинов...'));
    compiler.hooks.afterPlugins.tap('My Plugin', (stats) => console.log('Загрузка плагинов [done]'));
    compiler.hooks.compilation.tap('My Plugin', (stats) => console.log('Создание новой компиляции...'));
    compiler.hooks.afterCompile.tap('My Plugin', (stats) => console.log('Создание новой компиляции [done]'));
    compiler.hooks.emit.tap('My Plugin', (stats) => console.log('Генерация ресурсов...'));
    compiler.hooks.afterEmit.tap('My Plugin', (stats) => console.log('Генерация ресурсов [done]'));
    compiler.hooks.make.tap('My Plugin', (stats) => console.log('Запуск новой компиляции...'));
    compiler.hooks.done.tap(
      'My Plugin',
      (
        stats /* stats is passed as an argument when done hook is tapped.  */
      ) => {
        console.log('Компиляция завершена!');
      }
    );
  }
}

module.exports = MyCustomPlugin;
```

Чтобы подключить плагин к проекту, нам нужно передать экземпляр нашего класса MyCustomPlugin, чтобы Webpack вызвал метод apply при инициализации плагина:

```js
const MyCustomPlugin = require('./plugins/MyCustomPlugin/MyCustomPlugin');

module.exports = {
    return {
        ...
        plugins: [
            ...
            new MyCustomPlugin(),// Плагин для очистки папки dist
        ],
        ...
    }
}
```

В итоге при каждой новой сборке мы будем видеть сообщения в консоли:

![[webpack-plugins.png]]

---
## Резюме

Плагины позволяют расширять возможности webpack. Плагины могут использоваться для выполнения каких либо задач на той или иной стадии компиляции бандла. Например: плагин CleanWebpackPlugin очищает папку с бандлом до того как начнется компиляция всех файлов бандла и они будут перемещены в эту папку. Это может быть нужно чтобы отчистить папку от файлов предыдущей сборки.