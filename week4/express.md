# 🌈 1. Express

{% hint style="info" %}
[Express 공식문서 살펴보기](https://expressjs.com/ko/)

{% endhint %}

## 🚘 1-1. Express란

- Node.js의 굉장히 유명한 Server Web Framework
- 굉장히 오래됬고, 많이 사용하는 웹 프레임워크이다.

## 🚘 1-2. 간단한 서버 앱 npm 패키지 세팅

- 디렉터리를 만들어주고 패키지를 초기화 해준다.

```bash
mkdir express-demo-app
cd express-demo-app

npm init -y
```

- `.gitignore` 파일을 준비한다.

```bash
touch .gitignore
echo "/node_modules/" > .gitignore
```

- TypeScript 설정

```bash
npm i -D typescript
npx tsc --init
```

- ts-node 설치
- 타입스크립트로 짠 파일을 노드로 실행시키려면 노드로 컴파일이 필요한데 ts-node를 사용하면 그러한 것 없이 바로 실행이 가능하다.

```bash
npm i -D ts-node
```

- ESLint 설정

```bash
npm i -D eslint
npx eslint --init
```

```
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · none
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · node
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · xo-typescript
✔ What format do you want your config file to be in? · JavaScript
```

- Express 설치

```bash
npm i express
npm i -D @types/express
```

- 참고문서
    - [Express 설치](https://expressjs.com/ko/starter/installing.html)
    - [ts-node](https://github.com/TypeStrong/ts-node)

## 🚘 1-3. Hello World 예제 만들어보기

- [문서](https://expressjs.com/ko/starter/hello-world.html)
- TypeScript에 맞춰서 `app.ts` 파일 작성

```tsx
import express from 'express';

const port = 3000;

// app 인스턴스 생성
const app = express();

// / 주소 get method로 접근시 
app.get('/', (req, res) => {
	// 응답 보내기
	res.send('Hello, world!');
});

// 해당 포트로 서버 띄우기
app.listen(port, () => {
	console.log(`Server running at http://localhost:${port}`);
});
```

- ts-node로 실행하기

```bash
npx ts-node app.ts
```

- 코드를 수정할 때마다 서버를 재실행해야 하는 문제를 피하기 위해 [nodemon](https://github.com/remy/nodemon)을 사용하자.

```bash
npm i -D nodemon
npx nodemon app.ts
```

## 🚘 1-4. REST API

- 참고 문서
    - [Roy Fielding 논문](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
    - [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)
- 대개는 “필딩 제약 조건” 4가지를 모두 만족하지 않고, Resource와 HTTP Verb만 도입하는 수준으로 사용.
    - ❌ : /write-post
    - ⭕️ : /posts → 뭔가를 한다. (CRUD)
- CRUD에 대해 HTTP Method를 대입한다. Read는 Collection(복수)과 Item(Element)(단수)로 나뉜다.
- 기본 리소스 URL : /products
    1. Read(Collection) → `GET` /products ⇒ 상품 목록 확인
    2. Read(Item) → `GET` /products/{id} ⇒ 특정 상품 정보 확인
    3. Create(Collection Pattern 활용) → `POST` /products ⇒ 상품 추가(JSON 정보와 함께 전달)
    4. Update(Item) → `PUT` or `PATCH` /products/{id} ⇒ 특정 상품 정보 변경(JSON 정보와 함께 전달)
    5. Delete(Item) → `DELETE` /products/{id} ⇒ 특정 상품 삭제
- 예제 만들어보기

```tsx
app.get('/products', (req, res) => {

	const products = [
		{
			category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
		},
		{
			category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit',
		},
		{
			category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit',
		},
		{
			category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach',
		},
		{
			category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin',
		},
		{
			category: 'Vegetables', price: '$1', stocked: true, name: 'Peas',
		},
	];
	
	res.send({ products });
});
```