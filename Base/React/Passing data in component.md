2023-09-24 22:49
Tags: #web #react

`Пропсы` - данные которые передаются компоненты в качестве входящих параметров. С помощью них компоненты общаются.

Например, `className`, `src`, `alt` это некоторые из просов, которые можно передать тегу `<img />`.

```js
function Avatar() {
  return (
    <img
      className="avatar"
      src="https://i.imgur.com/1bX5QH6.jpg"
      alt="Lin Lanying"
    />
  );
}
```

`Пропсы` используются для пользовательских компонентов. Вот так они передаются

```js
<Button color="red" size={50} />
```

`Просы` всегда объект со свойствами, поэтому здесь идет деструктуризация `color` и `size`.

```js
const Button = ({ color, size }) => {
  return (
    <>
      <div>Color: {color}</div>
      <div>Size: {size}</div>
    </>
  );
};

export default Button;

```

Если нужно передать все пропсы от родителя ребенку, можно использовать оператор `...`.

```js
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```

> Синтаксис выше нужно использовать сдержанно. Если это используется в каждом другом компоненте, значит что-то не так. Обычно это означает, что нужно разделить компоненты и передавать `JSX` как ребенка. 

Если вложить контент внутрь `JSX тега`, то компонент родитель получит его как пропс, который зовется `children`.

```js
function App() {
  return (
    <div className="card">
      <Button color="red" size={50}>
        <div>Text</div>
      </Button>
    </div>
  );
}
```

```js
function Button({ children }) {
  console.log(children);

  return <div>Button</div>;
}
```

> Пропсы нельзя изменять, ведь это можно привести к более тяжелому тестированию и, возможно, просто не будет работать. Вместо этого нужно передать функцию изменения стейта.