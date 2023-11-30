#typescript

Union и Intersection - это возможность получать объединение и пересечения несколько типов в один. Например, функция printId может принимать id, который может быть либо string либо number:

```ts
function printId(id: number | string) {
	console.log("Your ID is: " + id);
}

// OK
printId(101);

// OK
printId("202");

// Error Argument of type '{ myID: number; }' is not assignable to parameter of type 'string | number'.
printId({ myID: 22342 });
```

Для создания типа, который расширяет сразу несколько других типов, можно использовать Intersection:

```ts
type Animal = {
  name: string;
}  
type Bear = Animal & { 
  honey: boolean;
}  
const bear = getBear();
bear.name;
bear.honey;
```

---
## Свойства объединений

1. Если в объединении у нас хотя бы одно свойство равно never, то оно проигнорируется:

![[typescript-union.png]]
2. Объединение типов дает нам пересечение свойств этих типов! Это сбивает некоторых с толку, но на самом деле все логично. Как написано в хэндбуке: Если у нас есть комната с высокими людьми в шляпах и вторая комната с испанцами в шляпах, то при объединении этих комнат мы можем быть уверены только в том, что все мужчины носят шляпы.

```ts
function printId(id: number | string) {
	console.log(id.toUpperCase());// Ошибка, toUpperCase нет в типе number
}
```

Проблема может быть решена при помощи сужения типов:

```ts
function printId(id: number | string) {
	if (typeof id === "string") {
		// In this branch, id is of type 'string'
		console.log(id.toUpperCase());
	} else {
		// Here, id is of type 'number'
		console.log(id);
	}
}
```

3. Тип any перекрывает любой другой тип при объединении:

```ts
type Test = any | string | number;// any
```