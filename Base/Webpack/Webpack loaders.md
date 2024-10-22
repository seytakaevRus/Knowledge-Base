---
tags:
  - webpack
---

В Webpack одним из способов расширения функционала служат загрузчики. Отличие лоудеров от плагинов заключается в том, что лоудер, по сути, это просто функция, которая принимает на вход содержимое файла, проводит какие-либо манипуляции с ним в теле функции, и возвращает новое содержимое, которое передается по цепочке следующему лоудеру.

Лоудеры добавляются в конфигурационном файле следующим способом:

```js
module.exports = {
    return {
        ...
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: ['style-loader', 'css-loader']
                },
                {
                    test: /\.s[ac]ss$/,
                    use: ['css-loader', 'sass-loader']
                },
            ]
        },
        ...
    }
}
```

==Обратите внимание что лоудеры выполняются в порядке с права на лево! То есть, сначала файл попадет на обработку в css-loader, а потом style-loader==

Мы можем создать свой кастомный лоудер. Webpack берет имена из массива поля use и ищет лоадеры с такими именами в папке node_modules. Мы можем создавать лоудер как отдельный проект и прилинковать его, но можно поступить проще и подсказать Webpack'у где искать лоудеры:

```js
module.exports = {
 return {
	   resolveLoader: {
          modules: ['node_modules', path.resolve(__dirname, 'loaders')],
     },
 }
}
```

Теперь Webpack будет искать лоудеры еще и в нашей папке loaders. Создадим в loaders папку test-loaders и в файле index.js опишем функционал нашего простого лоудера:

```js
module.exports = function loader(source) {
  // Custom loader logic
  return source + "; alert('hello from loader')";
}
```

Теперь подключим его для всех файлов с расширением js։

```js
module.exports = {
    return {
        ...
        module: {
            rules: [
                ...
                {
                    test: /\.js$/,
                    use: ['test-loader']
                },
            ]
        },
        ...
    }
}
```

После сборки можем проверить содержимое js файлов в сборке:

![[webpack-loaders.png]]

Теперь в контент js файлов добавляется наш alert!

---
## Резюме

Webpack позволяет строить пайплайн обработки любых файлов (.js, .css и так далее) при помощи лоудеров. Лоудеры - это простые функции, которые принимают на вход содержимое файла и возвращают обработанные данные, которые передаются по цепочке следующему лоудеру. Лоудер может использовать внутри себя другие зависимости, так например sass-loader принимает на вход файл со стилями в формате sass и, при помощи node-sass, конвертирует содержимое файла в обычный css и возвращает его. Также есть ts-loader для конвертации файлов .ts в файлы .js.