2023-09-29 21:12
Tags: #web #react

Зачастую в компонентах нужно отображать элементы, основывая на каком-нибудь условии. В React это можно рендерить JSX по условию, используя `if` инструкции, `&&` и `? :` операторы.
## Условный рендеринг

`Условный рендеринг` - рендеринг в зависимости от какого-то условия. В примере ниже, основываясь на `isPacked`нужно помечать или нет `Item`.

```js
function Item({ name, isPacked }) {
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}

```

Сделать это можно несколькими способами.

## Условие при помощи if

Если `isPacked` это `true`, то рендерится компонент с маркой, иначе рендерится без неё. Количество условий может быть больше, например, и использованием `else, else if`.

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }

  return <li className="item">{name}</li>;
}
```

## Возвращение null

Если нужно ничего не возвращать, то можно использовать `null`.

```js
function Item({ name, isPacked }) {
  if (isPacked) {
    return null;
  }
  return <li className="item">{name}</li>;
}
```
## Рендеринг при помощи тернарного оператора

Можно использовать конструкцию `? :` для более компактного условного рендеринга.  Вместо использования `if`, можно писать так.

> Этот подход нужно использовать с умом. Так как если в компонент много условий при использовании тернарного оператора будет нечитаемая каша.

```js
function Item({ name, isPacked }) {
  return <li className="item">{isPacked ? name + ' ✔' : name}</li>;
}
```

## Рендеринг при помощи &&

Этот подход можно использовать, когда в условии не нужна ветка `else`. Взять пример с маркировкой. Если `isPacked = true`, то мы показываем галочку, если нет, то не отображаем её, поэтому код может быть переписан таким образом.

Это `Логическое И`, он возвращает первое `falthy` (значение, которое при преобразовании в `boolean` дает `false`), иначе возвращает последнее значение. Если `isPacked = true`, то он возвращает марку, иначе возвращает `false`, и не отображает его, как и `undefined` и `null`.

```js
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✔'}
    </li>
  );
}
```

> Не нужно оставлять числа в левой стороне от &&. Так как React отрендерит 0. Поэтому не стоит делать выражения по типу `arrayLength && something`. А нужно сделать `arrayLength !== 0 && something`.
## Использование let и if

Можно использовать эту комбинацию для более разнообразных рендеров по условию.

```js
function Item({ name, isPacked }) {
  let itemContent = name;
  
  if (isPacked) {
    itemContent = name + " ✔";
  }
  
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}
```
``