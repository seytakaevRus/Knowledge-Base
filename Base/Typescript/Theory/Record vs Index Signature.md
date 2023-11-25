#typescript
Мы можем описывать объект при помощи утилки Record или при помощи Index Signature.

Описанием объекта при помощи Index Signature:

```ts
interface IObj {  
	[key: string]: number  
}
```

Описанием объекта при помощи Record:

```ts
type TObj = Record<string, number>  
  
const obj1: TObj = {  
	a: 1  
}  
  
const obj2: TObj = {  
	b: 2,  
	c: 3  
}
```

Однако утилка Record может использоваться только когда ключи объекта нам заранее известны:

```ts
type TObj1 = Record<string, number>
type TObj2 = Record<'foo' | 'bar', number>
```

Index Signature может быть использована даже тогда, когда мы заранее не знаем ключи объекта. Например, мы можем передать ключи объекты с помощью дженерика:

```ts
type TObj<Keys extends string | number | symbol> = {
	[Key in Keys]: number;
}

const obj: TObj<'a' | 'b' | 'c'> = {
	a: 1,
	b: 2,
	c: 3,
}
```