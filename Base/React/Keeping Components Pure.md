2023-09-30 14:23
Tags: #web #react

Некоторые из JS функция являются `чистыми`. Такие функции только выполняют вычисления и ничего более. Если делать компоненты как `чистые` функции, то можно избежать целый класс багов и непредсказуемого поведения.
## Чистота - формула в написании компонентов

`Чистая функция` - функция, которая следует правилам ниже:
1. Занимается свои делом. Не изменяет никакие объекты или переменные, существовавшие до вызова функции;
2. На те же выходные данные выдавать те же выходные.

Пример чистой функции. Если передать ей `3`, она вернет `6`. Всегда.

```js
function double(number) {
  return 2 * number;
}
```

Компоненты в React строятся на основе этого принципа. То есть возвращать тот же JSX при тех же входных данных. Если передать `drinkers={2}`, то вернется JSX, содержащий `2 cups of water`. Всегда.

```js
function Recipe({ drinkers }) {
  return (
    <ol>    
      <li>Boil {drinkers} cups of water.</li>
      <li>Add {drinkers} spoons of tea and {0.5 * drinkers} spoons of spice.</li>
      <li>Add {0.5 * drinkers} cups of milk to boil and sugar to taste.</li>
    </ol>
  );
}

export default function App() {
  return (
    <section>
      <h1>Spiced Chai Recipe</h1>
      <h2>For two</h2>
      <Recipe drinkers={2} />
      <h2>For a gathering</h2>
      <Recipe drinkers={4} />
    </section>
  );
}
```

## Внешний эффект

Как было сказано выше никаких изменений переменных, которые существовали до исполнения функции. Компонент ниже это нарушает. 

```js
let guest = 0;

function Cup() {
  // Bad: changing a preexisting variable!
  guest = guest + 1;
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup />
      <Cup />
      <Cup />
    </>
  );
}
```

Чтобы сделать компонент чистым, нужно передать `guest` в качестве пропса.

```js
function Cup({ guest }) {
  return <h2>Tea cup for guest #{guest}</h2>;
}

export default function TeaSet() {
  return (
    <>
      <Cup guest={1} />
      <Cup guest={2} />
      <Cup guest={3} />
    </>
  );
}
```

> В React есть 3 вида входных данных, с которых происходит считывание значений во время рендера: `props`, `state`, `context`. Всегда нужно оставлять их только для чтения. Если нужно изменить что-то из них нужно использовать функцию `set`, а не изменения прямо в переменную. А строгий режим поможет в определении нечистой функции.

## Где могут возникнуть побочные эффекты

`Сайд эффекты` (побочные эффекты) - это получение данных, подписка на события и ручное изменение DOM. Обычно вызываются внутри обработчиков событий.  Также сайд эффекты можно вызывать в `useEffect`.