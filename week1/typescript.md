# 🌈 2. TypeScript

{% hint style="info" %}
[TypeScript 공식문서](https://www.typescriptlang.org/ko/)는 부분적으로 한국어를 제공하기 때문에 보는데 조금 불편함이 있을 수 있지만 공식문서를 통해 꼭 학습해보자.
{% endhint %}

## 🚘 2-1. 간단하게 REPL 사용하기

```bash
npx ts-node
```

## 🚘 2-2. 타입 지정

```tsx
// 변수에 대한 타입 지정
let name: string;
let age: number;

name = '이재준';
age = 26;

// object 타입 지정
let human: {
	name: string;
	age: number;
};

human = { name: '이재준', age: 26 };
```

```tsx
// 복잡한 오브젝트의 타입을 재사용하기 위해 타입을 정의할 수 있다.
// 타입
type Human = {
	name: string;
	age: number;
};

let me: Human;

me = { name: '이재준', age: 26 };

// 인터페이스
interface Person {
	name: string;
	age: number;
};

let friend: Person;

friend = { name: '홍길동', age: 26 };

// 함수의 타입 정의(입력과 출력 타입)
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

- [타입 별칭과 인터페이스의 차이점](https://www.typescriptlang.org/ko/docs/handbook/2/everyday-types.html#%ED%83%80%EC%9E%85-%EB%B3%84%EC%B9%AD%EA%B3%BC-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)
- 타입 별칭은 타입을 확장할 때 교집합(`&`)을 사용하지만, 인터페이스는 `extends` 키워드를 통해 확장한다.
- 인터페이스는 기존의 인터페이스에 새 필드를 추가할 수 있지만, 타입은 생성된 뒤에 달라질 수 없다.

```tsx
// 정해진 값으로 타입을 지정할 수 있다.
// 이런 타입은 Union에서 유용하게 쓰인다.
let category: 'food';

category = 'food';
```

```tsx
// 배열은 타입 뒤에 대괄호를 붙여주면 된다.
let numbers: number[];

number = [1, 2, 3];
```

```tsx
// 모든 타입이 가능한 any
// 좋아보이지만 쓰면 타입스크립트를 쓰는 의미가 없기에 지양하자.
let anythings: any[];

anythings = ['hp', 256];

// 배열보다 깐깐하게 타입을 관리하고 싶다면 Tuple을 쓴다.
let pair: [string, number];

pair = ['hp', 256];
```

## 🚘 2-3. 타입 추론

- [문서](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html#%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-types-by-inference)

```tsx
const name1: string = '이재준';

// 타입 추론을 통해 name의 value값 '이재준'의 type인 string으로 자동 타입정의한다.
const name2 = '이재준'; 
```

## 🚘 2-4. Union Type

- [문서](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes.html#%EC%9C%A0%EB%8B%88%EC%96%B8-unions)

```tsx
// 여러 타입 중 하나. true이거나 false이다.
type bool = true | false;

let flag: bool;

flag = true;

flag = false;

flag = 3;
```

```tsx
// 매개변수를 제한하거나 할 때 매우 용하게 쓸 수 있다.
type Category = 'food' | 'toy' | 'bag';

// 자동완성도 유익하다.
function fetchProducts({ category }: { category: Category }) {
	console.log(`Fetch ${category}`);
}
```

```tsx
// 레거시 환경 또는 코드 때문에 안쓸 수 없음
// ReactNode가 대표적인 예다.
let target: string | number;

target = '이재준';

target = 26;

// Error!
target = null;

// Error!
target = undefined;

let targetName: string | undefined;

targetName = '이재준';

targetName = undefined;
```

- [React Type](https://github.com/facebook/react/blob/main/packages/shared/ReactTypes.js)

```tsx
// undefined를 직접 쓸 일은 없다. 가끔 함수매개변수(parameter)에서 사용되고 한다.
// 하지만 이보다 물음표 기호(?)를 써서 Optional Parameter로 처리하는 걸 추천
function greeting(name?: string): string {
	return `Hello, ${name || 'world'}`;
}

// 기본값을 잡아주면 더 좋다. undefined를 생각할 필요가 없어짐.
function greeting(name: string = 'world'): string {
	return `Hello, ${name}`;
}

// 매개변수가 오브젝트일 때 활용하면 좋음
// 여러줄로 object type을 지정하면 ts-node에서는 해석이 불가능하다.
// ts-node에서 사용할 때는 한줄로 쓰거나, type 등으로 따로 잡아주면 된다.
function greeting({ name = '', age }: {
  name: string;
  age?: number;  
}): string {
  return age ? `${name} (${age})` : name;
}

// 매개변수 type 따로 정의
type Human = {
  name: string;
  age?: number;
};

function greeting({ name = '', age }: Human): string {
  return age ? `${name} (${age})` : name;
}

greeting()

greeting({ name: '이재준' })

greeting({ name: '이재준', age: 26 })
```

## 🚘 2-5. Intersection Type

- [교집합 문서](https://www.typescriptlang.org/ko/docs/handbook/typescript-in-5-minutes-func.html#%EA%B5%90%EC%A7%91%ED%95%A9)
- [Intersection Types 문서](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types)

```tsx
type Human = {
	name: string;
	age: number;
};

type Creature = {
	hp: number;
	mp: number;
}

// 교집합을 통한 타입 확장
type Person = Human & Creature;

let person: Person;

person = { name: '이재준', age: 26, hp: 3000, mp: 2000 };
```

## 🚘 2-6. Generics, Utility Types, and Tips

- [Generics 문서](https://www.typescriptlang.org/docs/handbook/2/generics.html)

```tsx
// 어떤 타입이 들어올지 모를때, 타입에 해당하는 행동을 할 때 사용
// 타입 안정성을 가질 수 있음
function log<Type>(arg: Type): Type {
	return arg;
}

log<string>('이재준');
log<number>(26);
```

- [Utility Types 문서](https://www.typescriptlang.org/docs/handbook/utility-types.html)
- [더 좋은 타입스크립트 프로그래머로 만드는 11가지 팁](https://velog.io/@lky5697/11-tips-that-help-you-become-a-better-typescript-programmer)

## 🚘 2-7. 자동 완성 기능과 실시간 오류 검사

- VSCode or Webstorm에서는 TypeScript를 사용할 때 **자동 완성 기능과 실시간 오류 검사**를 지원한다. 이는 현실적으로 TypeScript를 쓰는 가장 큰 이유이다.
- 오래된 라이브러리의 경우 `d.ts` 파일만 따로 패키지로 제공된다. 패키지 이름은 `@types/oooo` 형태이다. 예로 `@types/react` 같은 것이 있다.
    - [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)
    - [DefinitelyTyped/types](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types)
    - [DefinitelyTypes/types/react](https://github.com/DefinitelyTyped/DefinitelyTyped/tree/master/types/react)