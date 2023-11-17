#typescript
Тип any - не безопасен, так как он позволяет делать нам все что нам придет в голову:

```ts
const obj: any = {
	a: 1,
	b: 2,
}

obj.c = 3;
obj.a = 'hello world';
```

Мы можем использовать более безопасную альтернативу - тип unknown. Этот тип не позволяет делать манипуляции с переменной пока мы явно не присвоили тип (type assertion) или не конкретизировали его (сужение типов) при помощи оператора as.

Сужение типов:

```ts
const obj: unknown = {
	a: 1
}

if(obj && typeof obj === 'object' && 'a' in obj) {
	obj.a = 2;
}
```

Присвоение типов:

```ts
type TObj = Record<string, number>;

const obj: unknown = {
	a: 1
};

console.log((obj as TObj).a);
```