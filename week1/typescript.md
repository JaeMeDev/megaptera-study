# π 2. TypeScript

{% hint style="info" %}
[TypeScript κ³΅μλ¬Έμ](https://www.typescriptlang.org/ko/)λ λΆλΆμ μΌλ‘ νκ΅­μ΄λ₯Ό μ κ³΅νκΈ° λλ¬Έμ λ³΄λλ° μ‘°κΈ λΆνΈν¨μ΄ μμ μ μμ§λ§ κ³΅μλ¬Έμλ₯Ό ν΅ν΄ κΌ­ νμ΅ν΄λ³΄μ.
{% endhint %}

## πΒ 2-1. κ°λ¨νκ² REPL μ¬μ©νκΈ°

```bash
npx ts-node
```

## πΒ 2-2. νμ μ§μ 

```tsx
// λ³μμ λν νμ μ§μ 
let name: string;
let age: number;

name = 'μ΄μ¬μ€';
age = 26;

// object νμ μ§μ 
let human: {
	name: string;
	age: number;
};

human = { name: 'μ΄μ¬μ€', age: 26 };
```

```tsx
// λ³΅μ‘ν μ€λΈμ νΈμ νμμ μ¬μ¬μ©νκΈ° μν΄ νμμ μ μν  μ μλ€.
// νμ
type Human = {
	name: string;
	age: number;
};

let me: Human;

me = { name: 'μ΄μ¬μ€', age: 26 };

// μΈν°νμ΄μ€
interface Person {
	name: string;
	age: number;
};

let friend: Person;

friend = { name: 'νκΈΈλ', age: 26 };

// ν¨μμ νμ μ μ(μλ ₯κ³Ό μΆλ ₯ νμ)
function add(x: number, y: number): number {
	return x + y;
}

add(1, 2);

// Error!
add('Hello', 'World');

function sub(x: number, y: number): string {
	// Error!
	return x - y;
}
```

- [νμ λ³μΉ­κ³Ό μΈν°νμ΄μ€μ μ°¨μ΄μ ](https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD%EA%B3%BC-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- νμ λ³μΉ­μ νμμ νμ₯ν  λ κ΅μ§ν©(`&`)μ μ¬μ©νμ§λ§, μΈν°νμ΄μ€λ `extends` ν€μλλ₯Ό ν΅ν΄ νμ₯νλ€.
- μΈν°νμ΄μ€λ κΈ°μ‘΄μ μΈν°νμ΄μ€μ μ νλλ₯Ό μΆκ°ν  μ μμ§λ§, νμμ μμ±λ λ€μ λ¬λΌμ§ μ μλ€.

```tsx
// μ ν΄μ§ κ°μΌλ‘ νμμ μ§μ ν  μ μλ€.
// μ΄λ° νμμ Unionμμ μ μ©νκ² μ°μΈλ€.
let category: 'food';

category = 'food';
```

```tsx
// λ°°μ΄μ νμ λ€μ λκ΄νΈλ₯Ό λΆμ¬μ£Όλ©΄ λλ€.
let numbers: number[];

number = [1, 2, 3];
```

```tsx
// λͺ¨λ  νμμ΄ κ°λ₯ν any
// μ’μλ³΄μ΄μ§λ§ μ°λ©΄ νμμ€ν¬λ¦½νΈλ₯Ό μ°λ μλ―Έκ° μκΈ°μ μ§μνμ.
let anythings: any[];

anythings = ['hp', 256];

// λ°°μ΄λ³΄λ€ κΉκΉνκ² νμμ κ΄λ¦¬νκ³  μΆλ€λ©΄ Tupleμ μ΄λ€.
let pair: [string, number];

pair = ['hp', 256];
```

## πΒ 2-3. νμ μΆλ‘ 

- [λ¬Έμ](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html#%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-types-by-inference)

```tsx
const name1: string = 'μ΄μ¬μ€';

// νμ μΆλ‘ μ ν΅ν΄ nameμ valueκ° 'μ΄μ¬μ€'μ typeμΈ stringμΌλ‘ μλ νμμ μνλ€.
const name2 = 'μ΄μ¬μ€'; 
```

## πΒ 2-4. Union Type

- [λ¬Έμ](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html#%EC%9C%A0%EB%8B%88%EC%96%B8-unions)

```tsx
// μ¬λ¬ νμ μ€ νλ. trueμ΄κ±°λ falseμ΄λ€.
type bool = true | false;

let flag: bool;

flag = true;

flag = false;

flag = 3;
```

```tsx
// λ§€κ°λ³μλ₯Ό μ ννκ±°λ ν  λ λ§€μ° μ©νκ² μΈ μ μλ€.
type Category = 'food' | 'toy' | 'bag';

// μλμμ±λ μ μ΅νλ€.
function fetchProducts({ category }: { category: Category }) {
	console.log(`Fetch ${category}`);
}
```

```tsx
// λ κ±°μ νκ²½ λλ μ½λ λλ¬Έμ μμΈ μ μμ
// ReactNodeκ° λνμ μΈ μλ€.
let target: string | number;

target = 'μ΄μ¬μ€';

target = 26;

// Error!
target = null;

// Error!
target = undefined;

let targetName: string | undefined;

targetName = 'μ΄μ¬μ€';

targetName = undefined;
```

- [React Type](https://github.com/facebook/react/blob/main/packages/shared/ReactTypes.js)

```tsx
// undefinedλ₯Ό μ§μ  μΈ μΌμ μλ€. κ°λ ν¨μλ§€κ°λ³μ(parameter)μμ μ¬μ©λκ³  νλ€.
// νμ§λ§ μ΄λ³΄λ€ λ¬Όμν κΈ°νΈ(?)λ₯Ό μ¨μ Optional Parameterλ‘ μ²λ¦¬νλ κ±Έ μΆμ²
function greeting(name?: string): string {
	return `Hello, ${name || 'world'}`;
}

// κΈ°λ³Έκ°μ μ‘μμ£Όλ©΄ λ μ’λ€. undefinedλ₯Ό μκ°ν  νμκ° μμ΄μ§.
function greeting(name: string = 'world'): string {
	return `Hello, ${name}`;
}

// λ§€κ°λ³μκ° μ€λΈμ νΈμΌ λ νμ©νλ©΄ μ’μ
// μ¬λ¬μ€λ‘ object typeμ μ§μ νλ©΄ ts-nodeμμλ ν΄μμ΄ λΆκ°λ₯νλ€.
// ts-nodeμμ μ¬μ©ν  λλ νμ€λ‘ μ°κ±°λ, type λ±μΌλ‘ λ°λ‘ μ‘μμ£Όλ©΄ λλ€.
function greeting({ name = '', age }: {
  name: string;
  age?: number;  
}): string {
  return age ? `${name} (${age})` : name;
}

// λ§€κ°λ³μ type λ°λ‘ μ μ
type Human = {
  name: string;
  age?: number;
};

function greeting({ name = '', age }: Human): string {
  return age ? `${name} (${age})` : name;
}

greeting()

greeting({ name: 'μ΄μ¬μ€' })

greeting({ name: 'μ΄μ¬μ€', age: 26 })
```

## πΒ 2-5. Intersection Type

- [κ΅μ§ν© λ¬Έμ](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes-func.html#%EA%B5%90%EC%A7%91%ED%95%A9)
- [Intersection Types λ¬Έμ](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types)

```tsx
type Human = {
	name: string;
	age: number;
};

type Creature = {
	hp: number;
	mp: number;
}

// κ΅μ§ν©μ ν΅ν νμ νμ₯
type Person = Human & Creature;

let person: Person;

person = { name: 'μ΄μ¬μ€', age: 26, hp: 3000, mp: 2000 };
```

## πΒ 2-6. Generics, Utility Types, and Tips

- [Generics λ¬Έμ](https://www.typescriptlang.org/docs/handbook/2/generics.html)

```tsx
// μ΄λ€ νμμ΄ λ€μ΄μ¬μ§ λͺ¨λ₯Όλ, νμμ ν΄λΉνλ νλμ ν  λ μ¬μ©
// νμ μμ μ±μ κ°μ§ μ μμ
function log<Type>(arg: Type): Type {
	return arg;
}

log<string>('μ΄μ¬μ€');
log<number>(26);
```

- [Utility Types λ¬Έμ](https://www.typescriptlang.org/docs/handbook/utility-types.html)
- [λ μ’μ νμμ€ν¬λ¦½νΈ νλ‘κ·Έλλ¨Έλ‘ λ§λλ 11κ°μ§ ν](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)

## πΒ 2-7. μλ μμ± κΈ°λ₯κ³Ό μ€μκ° μ€λ₯ κ²μ¬

- VSCode or Webstormμμλ TypeScriptλ₯Ό μ¬μ©ν  λ **μλ μμ± κΈ°λ₯κ³Ό μ€μκ° μ€λ₯ κ²μ¬**λ₯Ό μ§μνλ€. μ΄λ νμ€μ μΌλ‘ TypeScriptλ₯Ό μ°λ κ°μ₯ ν° μ΄μ μ΄λ€.
- μ€λλ λΌμ΄λΈλ¬λ¦¬μ κ²½μ° `d.ts` νμΌλ§ λ°λ‘ ν¨ν€μ§λ‘ μ κ³΅λλ€. ν¨ν€μ§ μ΄λ¦μ `@types/oooo` ννμ΄λ€. μλ‘ `@types/react` κ°μ κ²μ΄ μλ€.
    - [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)
    - [DefinitelyTyped/types](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types)
    - [DefinitelyTypes/types/react](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/react)