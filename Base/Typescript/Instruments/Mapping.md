#typescript 
## Usage
При помощи маппинга можно пройтись циклом по ключам того или иного типа:

```typescript
const data = {
	name: "Karen",
	age: 24,
}

type MyPartial<T extends Record<string, unknown>> = {
	[Key in keyof T]?: T[Key]
}

type Test = MyPartial<typeof data>;//Поля станут не обязательными
```

В этом примере мы пробежали по всем полям типа и добавили ? чтобы сделать все поля не обязательными

## Remapping
Кроме того оператор as может использоваться для ремапинга. В качестве примера можем реализовать утилиту для генерации типов функций геттеров объекта:

```typescript
const data = {
	name: "Karen",
	age: 24,
}
type Getters<T extends Record<string, unknown>> = {
[Key in keyof T as `get${Capitalize<Key>}`]: () => T[Key]
}

type Test = Getters<typeof data>;

const getters: Getters<typeof data> = {
	getName: () => data.name,
	getAge: () => data.age,
}
```