2023-09-24 22:40
Tags: #web #react 
## JSX

`JSX` - расширение, представляющее собой комбинацию `JS` и `XML` разметки. Является синтаксическим сахаром над `React.createElement`. Расширение строже, чем `HTML`. Ниже перечислены конкретные ограничения.
## 1. Возвращать один элемент

Чтобы вернуть несколько элементов из компонента, их нужно обернуть в тег. Ниже этим тегом выступает `div`, также можно использовать `<></>`, так как этот тег не будет вставлен в разметку `HTML`. Этот тег зовется `Fragment`.

>Оборачивать нужно, так как `JSX` превращается в объекты под капотом, а вернуть несколько объектов сразу нельзя, поэтому нужно обернуть либо в другой тег, либо в `Fragment`

```html
<h1>Hedy Lamarr's Todos</h1>
<ul>  
	...  
</ul>
```

```js
<div>
	<h1>Hedy Lamarr's Todos</h1>
	<ul>  
		...  
	</ul>
</div>
```

## 2. Закрывать все теги

Одиночные теги в `JSX` закрываются.

```js
<img />
```

Парные теги так и остаются.

```js
<li>Element</li>
```
## 3. Большинство атрибутов пишется в camelCase

Так как `JSX` это комбинация, то появилось больше ключевых слов, например, в `HTML` использовался атрибут `class`, теперь же нужно использовать `className`, так как `class` это слово в `JS`.
## Отображение данных

Нужно использовать `{}` для отображения динамических данных.
Они могут использоваться в двух случаях:
1. Как текст прямо в `JSX теге`;
2. Как атрибут после `=`.

```js
return (
  <h1 className={user.class}>
    {user.name}
  </h1>
);
```

Также можно передать объект с помощью `{}`. 

```js
<div
	 style={{
          width: user.imageSize,
          height: user.imageSize
    }}
>
</div>
```

В остальных случаях для отображения или передачи могут быть использованы обычные кавычки `""` или `''`.

```js
<img src="https://..." alt="Avatar" />
```