2023-09-24 17:57
Tags: #web #react
## Создание компонента
[[First Component]]
## Использование JSX
[[Writing Markup with JSX]]
## Обмен данными между компонентами
[[Passing data in component]]
## Условный рендеринг
[[Conditional rendering]]

## Добавление стилей

Нужно использовать атрибут **className** для присваивания классов.

```js
<img className="avatar" />
```

```css
.avatar {
  border-radius: 50%;
}
```

Либо же можно использовать атрибут `style`. 

```js
<div
	 style={{
          width: user.imageSize,
          height: user.imageSize
    }}
>
</div>
```
### Условный рендеринг

Можно использовать `if/else`.

```js
let content;

if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}

return (
  <div>
    {content}
  </div>
);
```

Или `тернарный оператор`, если нравится более компактный код.

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

Когда в условии не нужен `else`, можно использовать оператор `&&` .

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

## Отрисовка списка

Для этого используется функция `map`.

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

Атрибут `key` используется, чтобы `React` мог отличить элемент среди других. Обычно сюда попадает `id`, который приходит из данных, например, `id` БД.
## Обработка событий

Обработка происходит путём передачи обработчика события.

```js
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

Причем `handleClick` указывается без `()`, потому что нужно передать только ссылку. Обработчик будет вызван, когда нажмут на кнопку. 
## Обновление экрана

Для обновления данных используется хук `useState`. Возвращает массив, где первым элементом идет текущее состояние, а вторым элементом функция, которая может изменить это состояние. При вызове `setCount` происходит ререндер компонента.

```js
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```
## Использование хуков

`Хуки` - функции, которые начинаются с `use`. Например, `useState` один из таких хуков. `Хуки` имеют свои ограничения. Одно из гласит: "Хуки можно вызывать только на верхнем уровне компонента". Также их нельзя использовать в условиях или циклах.
