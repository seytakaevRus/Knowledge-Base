2023-09-29 23:17
Tags: #web #react

Часто нужно отобразить множество похожих элементов из коллекции с данными. Можно использовать `JS` методы для манипуляции с массивами данных. Например, `filter`, `map`.
## Отображение данных из массива

Это можно сделать с помощью метода `map`. Благодаря этому методу создастся массив их `li` элементов.

```js
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  
  return <ul>{listItems}</ul>;
}
```
## Фильтрация массива с элементами

Для фильтрации используется соответственно метод `filter`.

```js
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  
  const listItems = chemists.map(person =>
    <li>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  
  return <ul>{listItems}</ul>;
}
```
## О атрибуте key

Если запустить код выше, то консоль будет выдавать ошибку "каждый ребенок в массиве должен иметь уникальный атрибут `key`". Поэтому нужно дать каждому элементу массива его, обычно это строка или число, которое уникально идентифицирует элемент среди других.

```js
<li key={person.id}>...</li>
```

>JSX элементы внутри `map` всегда нуждаются в key.

Атрибут говорит React, какой элемент массива к какому компоненту принадлежит, поэтому React сможет их сопоставить в будущем. Это становится важно, если массив элементов может двигаться (сортировка, например), или в него можно добавить, удалить что-то. Хорошо выбранный `key` позволяет React понять что произошло и правильно применить изменения к `DOM` дереву.

>Если нужно, чтобы сразу несколько элементов были дочерними узлами для каждого значения в массиве, то можно использовать `Fragment`. Именно его, а не запись `<></>`, так как `Fragment` можно указать `key`.
## Где достать ключ

Разные источники данных предоставляют разные источники `key`:
1. Данных с Базы Данных, если данные пришли с БД, то можно использовать главный ключ, который уникальный по своей природе;
2. Локально сгенерированные данные, можно использовать инкремент, `crypto.randomUUID` или пакет `uuid` при создании элементов.
## Правила атрибута `key`

1. Ключи должны быть уникальны среди своих соседей. Однако, все в порядке, если одни и тот же ключ используется для JSX в разных массивах;
2. Ключи не должны меняться, это противоречит их цели. Не создавайте их во время рендеринга.

> Не нужно в качестве key использовать Math.random и Date.now().
## Почему React вообще нужны ключи

Каждый ключ это идентификатор элемента/компонента. Между рендерами React смотрит на них и понимает, что нужно создать, а что удалить.

```js
const array = [
  { id: 1, value: 1 },
  { id: 2, value: 2 },
  { id: 3, value: 3 },
  { id: 4, value: 4 },
];

const Item = memo(({ id, handleDelete }) => {
  const handleClick = () => {
    handleDelete(id);
  };

  useEffect(() => {
    console.log(`Create component with id: ${id}`);

    return () => {
      console.log(`Delete component with id: ${id}`);
    };
  });

  return (
    <div>
      <a onClick={handleClick}>{id}</a>
    </div>
  );
});

class Item2 extends PureComponent {
  componentDidMount() {
    console.log('DID MOUNT ', this.props.id);
  }

  componentDidUpdate(prevProps) {
    console.log('DID UPDATE ', prevProps.id, ' -> ', this.props.id);
  }

  componentWillUnmount() {
    console.log('WILL UNMOUNT ', this.props.id);
  }

  handleClick() {
    this.props.handleDelete(this.props.id);
  }

  render() {
    return (
      <li>
        <span onClick={this.handleClick.bind(this)}>{this.props.id}</span>
      </li>
    );
  }
}

export default function App() {
  const [items, setItems] = useState(array);

  const handleDelete = useCallback((idToRemove: number) => {
    setItems(i => i.filter(({ id }) => id !== idToRemove));
  }, []);

  return items.map(({ id }, index) => (
    <Item key={index} id={id} handleDelete={handleDelete} />
  ));

  return items.map(({ id }, index) => (
    <Item2 key={index} id={id} handleDelete={handleDelete} />
  ));
}
```

В примере выше, сначала вызовутся сообщения:
`Create component with id 1`
`Create component with id 2`
`Create component with id 3`
`Create component with id 4`

Если удалить элемент, с `id = 4`, то будет сообщение `Delete component with id: 4`.

Если же удалить, например, с `id = 2`, то будет:
`Delete component with id: 4`
`Delete component with id: 2`
`Delete component with id: 3`
`Create component with id: 3`
`Create component with id: 4`

Это произошло потому что, при удаление элемента с `id = 2`, `key` последующих элементов изменился, так как он зависел от индекса элемента в массиве, а он сдвинулся. Поэтому все элементы после `id = 2` были удалены и заново созданы.

> Что интересно это поведение свойственно при использование хуков. В классовых же компонентах ситуация другая. Ключи `1, 2` остались, но у элементов, к которым они привязаны изменились значения, поэтому вызовется `Did Update`, для ключа `3` теперь нет места, так как элементов всего 3, поэтому элемент, к которому он привязан был, удалится и вызовется `Will Unmount`.