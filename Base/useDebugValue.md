2023-09-28 14:56
Tags: #web #react
## Для чего

Позволяет добавлять пометки к кастомным хукам в `React DevTools` (расширение браузера для "инспектирования" приложения React).

```js
useDebugValue(value, format?)
```

---
### useDebugValue(value, format?)
### Параметры

`value` - значение, которое будет отображаться в React DevTools. Оно может иметь любой тип.

Необязательный `format` - форматирующая функция. Когда компонент "инспектируется", то React DevTools вызовет функцию форматирования с `value` как аргумент и отобразит возращаемое форматированное значение. Если не указать, то `value` будет отображаться без изменений.
### Возвращаемое значение

Хук ничего не возвращает.

---
## Использование

### Добавление пометки к кастомному хуку

```js
function App() {
	const [count, setCount] = useState(0);
	
	useEffect(() => {
		setInterval(() => {
			setCount((c) => c + 1);
		}, 2000);
	}, []);

	const useCustomHook = (count: number) => {
		useDebugValue(count % 2 === 0 ? 'even' : 'odd');
	};
	
	useCustomHook(count);
	
	return <div>{count}</div>;
}```

В зависимости от `count` в React DevTools будет выведено: 
1.   CustomHook2: "odd";
2.   CustomHook2: "event"

> Не нужно добавлять этот хук в каждый кастомный хук. Наиболее разумно добавлять его в хуки, которые являются частью общей библиотеки и имеют сложную внутреннюю структуру данных, которую трудно дебажить.

### Использование параметра format

Вторым параметром передана функция, которая принимает строку и вызывает функцию `toUpperCase`.

```js
const useCustomHook = (count: number) => {
    useDebugValue(count % 2 === 0 ? 'even' : 'odd', (status) =>
      status.toUpperCase(),
    );
};

useCustomHook(count);
```
