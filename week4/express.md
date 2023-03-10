# ๐ 1. Express

{% hint style="info" %}
[Express ๊ณต์๋ฌธ์ ์ดํด๋ณด๊ธฐ](https://expressjs.com/ko/)

{% endhint %}

## ๐ย 1-1. Express๋

- Node.js์ ๊ต์ฅํ ์ ๋ชํ Server Web Framework
- ๊ต์ฅํ ์ค๋๋ฌ๊ณ , ๋ง์ด ์ฌ์ฉํ๋ ์น ํ๋ ์์ํฌ์ด๋ค.

## ๐ย 1-2. ๊ฐ๋จํ ์๋ฒ ์ฑ npm ํจํค์ง ์ธํ

- ๋๋ ํฐ๋ฆฌ๋ฅผ ๋ง๋ค์ด์ฃผ๊ณ  ํจํค์ง๋ฅผ ์ด๊ธฐํ ํด์ค๋ค.

```bash
mkdir express-demo-app
cd express-demo-app

npm init -y
```

- `.gitignore` ํ์ผ์ ์ค๋นํ๋ค.

```bash
touch .gitignore
echo "/node_modules/" > .gitignore
```

- TypeScript ์ค์ 

```bash
npm i -D typescript
npx tsc --init
```

- ts-node ์ค์น
- ํ์์คํฌ๋ฆฝํธ๋ก ์ง  ํ์ผ์ ๋ธ๋๋ก ์คํ์ํค๋ ค๋ฉด ๋ธ๋๋ก ์ปดํ์ผ์ด ํ์ํ๋ฐ ts-node๋ฅผ ์ฌ์ฉํ๋ฉด ๊ทธ๋ฌํ ๊ฒ ์์ด ๋ฐ๋ก ์คํ์ด ๊ฐ๋ฅํ๋ค.

```bash
npm i -D ts-node
```

- ESLint ์ค์ 

```bash
npm i -D eslint
npx eslint --init
```

```
โ How would you like to use ESLint? ยท style
โ What type of modules does your project use? ยท esm
โ Which framework does your project use? ยท none
โ Does your project use TypeScript? ยท No / Yes
โ Where does your code run? ยท node
โ How would you like to define a style for your project? ยท guide
โ Which style guide do you want to follow? ยท xo-typescript
โ What format do you want your config file to be in? ยท JavaScript
```

- Express ์ค์น

```bash
npm i express
npm i -D @types/express
```

- ์ฐธ๊ณ ๋ฌธ์
    - [Express ์ค์น](https://expressjs.com/ko/starter/installing.html)
    - [ts-node](https://github.com/TypeStrong/ts-node)

## ๐ย 1-3. Hello World ์์  ๋ง๋ค์ด๋ณด๊ธฐ

- [๋ฌธ์](https://expressjs.com/ko/starter/hello-world.html)
- TypeScript์ ๋ง์ถฐ์ `app.ts` ํ์ผ ์์ฑ

```tsx
import express from 'express';

const port = 3000;

// app ์ธ์คํด์ค ์์ฑ
const app = express();

// / ์ฃผ์ get method๋ก ์ ๊ทผ์ 
app.get('/', (req, res) => {
	// ์๋ต ๋ณด๋ด๊ธฐ
	res.send('Hello, world!');
});

// ํด๋น ํฌํธ๋ก ์๋ฒ ๋์ฐ๊ธฐ
app.listen(port, () => {
	console.log(`Server running at http://localhost:${port}`);
});
```

- ts-node๋ก ์คํํ๊ธฐ

```bash
npx ts-node app.ts
```

- ์ฝ๋๋ฅผ ์์ ํ  ๋๋ง๋ค ์๋ฒ๋ฅผ ์ฌ์คํํด์ผ ํ๋ ๋ฌธ์ ๋ฅผ ํผํ๊ธฐ ์ํด [nodemon](https://github.com/remy/nodemon)์ ์ฌ์ฉํ์.

```bash
npm i -D nodemon
npx nodemon app.ts
```

## ๐ย 1-4. REST API

- ์ฐธ๊ณ  ๋ฌธ์
    - [Roy Fielding ๋ผ๋ฌธ](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
    - [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)
- ๋๊ฐ๋ โํ๋ฉ ์ ์ฝ ์กฐ๊ฑดโ 4๊ฐ์ง๋ฅผ ๋ชจ๋ ๋ง์กฑํ์ง ์๊ณ , Resource์ HTTP Verb๋ง ๋์ํ๋ ์์ค์ผ๋ก ์ฌ์ฉ.
    - โย : /write-post
    - โญ๏ธ : /posts โ ๋ญ๊ฐ๋ฅผ ํ๋ค. (CRUD)
- CRUD์ ๋ํด HTTP Method๋ฅผ ๋์ํ๋ค. Read๋ Collection(๋ณต์)๊ณผ Item(Element)(๋จ์)๋ก ๋๋๋ค.
- ๊ธฐ๋ณธ ๋ฆฌ์์ค URL : /products
    1. Read(Collection) โ `GET` /products โ ์ํ ๋ชฉ๋ก ํ์ธ
    2. Read(Item) โ `GET` /products/{id} โ ํน์  ์ํ ์ ๋ณด ํ์ธ
    3. Create(Collection Pattern ํ์ฉ) โ `POST` /products โ ์ํ ์ถ๊ฐ(JSON ์ ๋ณด์ ํจ๊ป ์ ๋ฌ)
    4. Update(Item) โ `PUT` or `PATCH` /products/{id} โ ํน์  ์ํ ์ ๋ณด ๋ณ๊ฒฝ(JSON ์ ๋ณด์ ํจ๊ป ์ ๋ฌ)
    5. Delete(Item) โ `DELETE` /products/{id} โ ํน์  ์ํ ์ญ์ 
- ์์  ๋ง๋ค์ด๋ณด๊ธฐ

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