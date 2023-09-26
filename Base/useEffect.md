2023-09-25 21:48
Tags: #web #react

Позволяет синхронизировать компонент с `внешней системой`. Здесь под `внешней системой` подразумевается любой кусок кода, который не контролируется React:
1. Таймер, управляемый `setInterval()` и `clearInterval()`;
2. Событие, подписываемое с помощью `window.addEventListener()` и `window.removeEventListener()`;
3. Сторонняя библиотека анимаций с похожим API `animation.start()` и `animation.reset()`.  

```js
useEffect(() => {
    calculateSalary(income);
  }, [income]);
```

---
### useEffect(setup, dependencies?)
### Параметры

`setup` - функция, с логикой `Эффекта`. Она может возвращать функция `cleanup`. Когда компонент добавится в `DOM`, то функция `setup` будет вызвана. После каждого ререндера с измененными `dependencies`, React сначала вызовет функция `cleanup` (если она была передана) с предыдущими значениями, и только потом `setup` с новыми значениями. После того как компонент удалится из `DOM`, React вызовет функция `cleanup`.

Необязательные `dependencies` - список всех реактивных значений, упомянутых в функции `setup`. Это могут быть пропсы, состояния и все переменные или функции, объявленные внутри тела компонента. Массив зависимостей должен иметь постоянное значения элементов и иметь вид `[dep1, dep2, dep3]`. React сравнивает каждую зависимость с предыдущей при помощи `Object.is`. Если опустить этот список, то `setup` будет вызываться на каждое изменение из `dependencies`. Если указать пустой массив, то будет вызвано один раз при создании компонента.
### Возвращаемое значение

Хук возвращает undefined.
### Предостережения

Как и любой хук его можно вызывать только на верхнем уровне компонента или в собственных хуках. Нельзя вызывать внутри цикла или в условиях.

Иногда useEffect может быть не нужен TODO: https://react.dev/learn/you-might-not-need-an-effect

Когда включен строгий режим, то React вызовет один дополнительный цикл `setup + cleanup` перед вызовом первого реального `setup`. Это стресс-тест, который гарантирует, что логика внутри `cleanup` останавливает ту, что находится в `setup`.

Если зависимостями является объект или функция, то это может вызвать нежелательные ререндеры, от этого можно попробовать избиваться. (Раздел `"Использование"`).

TODO: Написать отличие от useLayoutEffect https://react.dev/reference/react/useLayoutEffect

---
## Использование

### Базовое применение

Здесь `useEffect` применяется для добавления обработчика события. А так же для удаления, когда компонент перестанет существовать.

```js
function App() {
  useEffect(() => {
  	document.addEventListener('click', handleClick);

	return () => document.removeEventListener('click', handleClick);
  }, []);
}
```

Вообще React вызывает функции `setup` и `cleanup`, когда это необходимо.
1. Код внутри `setup` вызывается, когда компонент добавляется на страницу (монтируется);
2. После каждого ререндера компонента, где зависимости были изменены:
	- Сначала вызывается код внутри `cleanup` со старыми пропсами и состоянием;
	- Далее вызывается код внутри `setup` с новыми пропсами и состоянием.
3. Потом вызывается последний раз код внутри `cleanup` после того, как компонент удаляет из страницы (размонтируется).

### Использование в пользовательском хуке

В хуке `useChatRoom` сокрыта логика подсоединения и отключения к комнате. 

```js
export function useChatRoom({ serverUrl, roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId, serverUrl]);
}
```

### Fetching данных с сервера

Запрос на получение данных указывается в `useEffect`.

TODO: прочитать про https://maxrozen.com/race-conditions-fetching-data-react-with-useeffect

```js
export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);

  useEffect(() => {
    let ignore = false;
    
    setBio(null);
    
    fetch(person)
	    .then(result => {
	      if (!ignore) {
	        setBio(result);
	      }
	    });
    
    return () => {
      ignore = true;
    };
  }, [person]);
```

TODO: https://react.dev/reference/react/useEffect#examples-connecting (object and functions)