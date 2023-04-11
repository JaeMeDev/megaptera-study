# 🌈 1. 로그인

## 🚘 1-1. 로그인1(로그인 페이지를 구현해보자.)

- 아이디와 password를 통해 로그인해서 Access Token(JWT)를 얻는 과정을 구현해보자.
- JWT를 관리하는 방법에는 쿠키, 세션, localStorage가 있지만 해당 강의에서는 브라우저의 localStorage를 사용한다. (localStorage 사용시 XSS 공격에도 취약하다. 쿠키를 `httpOnly` / `secure` / `SameSite` 옵션을 사용하면 자바스크립트 코드 상 접근이 불가능 해지기 때문에 백엔드 측에서 설정을 해준다면 쿠키가 좋을 것 같다.) → 협업이 필요할 것 같다.
- E2E 테스트 작성(코드를 작성해 테스트를 통과해보자.)

```tsx
Scenario('Log in', ({ I }) => {
  I.amOnPage('/');

  I.click('Login');

  I.fillField('Email', 'tester@example.com');
  I.fillField('Password', 'password');

  I.click('로그인', { css: 'form' });

  I.waitForText('Cart');
});

Scenario('Log failed', ({ I }) => {
  I.amOnPage('/');

  I.click('Login');

  I.fillField('Email', 'tester@example.com');
  I.fillField('Password', 'xxx');

  I.click('로그인', { css: 'form' });

  I.waitForText('로그인 실패');
});
```

- 로그인 페이지를 만들어주고 `LoginPage.tsx` route에 연결하는 작업부터 진행해준다.

```tsx
import { useEffect } from 'react';

import { useNavigate } from 'react-router-dom';

import LoginForm from '../components/login/LoginForm';

import useLoginFormStore from '../hooks/useLoginFormStore';

export default function LoginPage() {
  const navigate = useNavigate();

  const [{ accessToken }, store] = useLoginFormStore();

  useEffect(() => {
		// 화면 처음 접근 시 상태를 초기화해준다.
    store.reset();
  }, []);

  useEffect(() => {
		// 토큰 생성시 hook
    if (accessToken) {
      store.reset();
      navigate('/');
    }
  }, [accessToken]);

  return (
    <LoginForm />
  );
}
```

- `LoginForm.tsx` 컴포넌트를 만들어준다.

```tsx
import React, { useEffect } from 'react';

import styled from 'styled-components';

import TextBox from '../ui/TextBox';
import Button from '../ui/Button';

import useAccessToken from '../../hooks/useAccessToken';
import useLoginFormStore from '../../hooks/useLoginFormStore';

const Container = styled.div`
  h2 {
    margin-bottom: 1rem;
    font-size: 4rem;
  }

  button {
    margin-left: 10.5rem;
  }

  p {
    margin-block: 1rem;
  }
`;

export default function LoginForm() {
	// 로컬 스토지를 숨기려고 사용하는 것.
  const { setAccessToken } = useAccessToken();

  // 입력받은 것... Submit 누를 수 있는 상태인지 확인하려고 사용.
  const [{
    email, password, valid, error, accessToken,
  }, store] = useLoginFormStore();

  useEffect(() => {
		// 있으면 로그인
    if (accessToken) {
      setAccessToken(accessToken);
    }
  }, [accessToken]);

  const handleChangeEmail = (value: string) => {
    store.changeEmail(value);
  };

  const handleChangePassword = (value: string) => {
    store.changePassword(value);
  };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    store.login();
  };

  // Form Submit이 기존 버튼에 해당되지 않기 때문에 Button 컴포넌트의 type을 추가하자.
  return (
    <Container>
      <h2>로그인</h2>
      <form onSubmit={handleSubmit}>
        <TextBox
          label="E-mail"
          placeholder="tester@example.com"
          value={email}
          onChange={handleChangeEmail}
        />
        <TextBox
          label="Password"
          type="password"
          value={password}
          onChange={handleChangePassword}
        />
        <Button type="submit" disabled={!valid}>
          로그인
        </Button>
        {error && (
          <p>로그인 실패</p>
        )}
      </form>
    </Container>
  );
}
```

- 사용할 `TextBox` 컴포넌트 → 개인적으로는 `react-hook-form` 을 사용해서 보통 `TextField` 나 `InputBox` 같은 것을 만들어서 사용해줬는데 많은 옵션이 추가되지 않는 다면 기본 `input` 을 사용해주는 것이 더 좋을 것 같다.

```tsx
import React, { useRef } from 'react';

import styled from 'styled-components';

const Container = styled.div`
  margin-block: .5rem;

  label {
    display: inline-block;
    width: 10rem;
    margin-right: .5rem;
    text-align: right;
    vertical-align: middle;
  }

  input {
    width: 20rem;
  }
`;

type TextBoxProps = {
  label: string;
  placeholder?: string;
  type?: 'text' | 'number' | 'password'; // 지원 타입을 써주자.
  value: string;
  onChange: (value: string) => void;
}

export default function TextBox({
  label, placeholder = undefined, type = 'text', value, onChange,
}: TextBoxProps) {
  const id = useRef(`textbox-${Math.random().toString().slice(2)}`);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    onChange(event.target.value);
  };

  return (
    <Container>
      <label htmlFor={id.current}>
        {label}
      </label>
      <input
        id={id.current}
        type={type}
        placeholder={placeholder}
        value={value}
        onChange={handleChange}
      />
    </Container>
  );
}
```

- 기본 Props 값을 지원하는 ESLint를 추가해서 사용하자.

```tsx
'react/require-default-props': [2, { functions: 'defaultArguments' }],
```

- `Button` 컴포넌트에 type을 추가하고 disabled 스타일을 추가해주자.

```tsx
import styled from 'styled-components';

type ButtonProps = {
  type?: 'button' | 'submit' | 'reset';
}

const Button = styled.button.attrs<ButtonProps>((props) => ({
  type: props.type ?? 'button',
}))`
  border: .1rem solid ${(props) => props.theme.colors.primary};
  background: transparent;
  color: ${(props) => props.theme.colors.primary};
  cursor: pointer;

  :disabled {
    filter: grayscale(80%);
    cursor: not-allowed;
  }
`;

export default Button;
```

- `useAccessToken` 을 만들자.

```tsx
export default function useAccessToken() {
  const [accessToken, setAccessToken] = useLocalStorage('accessToken', '');

  return { accessToken, setAccessToken };
}
```

- `LoginFormStore` 을 만들자.

```tsx
@singleton()
@Store()
export default class LoginFormStore {
  email = '';

  password = '';

  error = false;

  accessToken = '';

  // Validation 체크 (버튼을 비활성화 시켜줌)
  get valid() {
    return this.email.includes('@') && !!this.password;
  }

  @Action()
  changeEmail(email: string) {
    this.email = email;
  }

  @Action()
  changePassword(password: string) {
    this.password = password;
  }

  @Action()
  setAccessToken(accessToken: string) {
    this.accessToken = accessToken;
  }

  @Action()
  private setError() {
    this.error = true;
  }

  @Action()
  reset() {
    this.email = '';
    this.password = '';
    this.error = false;
    this.accessToken = '';
  }

  async login() {
    try {
      const accessToken = await apiService.login({
        email: this.email,
        password: this.password,
      });

      this.setAccessToken(accessToken);
    } catch (e) {
      this.setError();
    }
  }
}
```

- `useLoginFormStore` 훅을 준비해주자.

```tsx
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import LoginFormStore from '../stores/LoginFormStore';

export default function useLoginFormStore() {
  const store = container.resolve(LoginFormStore);
  return useStore(store);
}
```

- `APIService` 에 로그인 API 호출 메서드를 추가한다.

```tsx
async login({ email, password }: {
  email: string;
  password: string;
}): Promise<string> {
  const { data } = await this.instance.post('/session', { email, password });
  // JWT 토큰 받아오기
	const { accessToken } = data;
  return accessToken;
}
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 라이브러리를 사용하거나 Form 상태를 관리할 때는 따로 Hook으로 분리하거나 Store에서 관리하는 방법 말고 해당 컴포넌트에 상태를 만들어서 만들어줬었는데 해당 상태를 분리시키니까 복잡한 작업을 컴포넌트 외로 분리시킬 수 있는 것 같다는 생각이 들었다.

## 🚘 1-2. 로그인2(로그인 여부에 따라 화면을 다르게 보여주자.)

- 로그인 여부에 따라 GNB 구성이 바뀌도록 `Header` 를 수정하자. `useAccessToken` 훅을 만들어놨기 때문에 해당 훅을 이용하면 간단하게 Header를 수정할 수 있다.
- import를 컴포넌트 훅 순으로 정리하자. (권장사항)

```tsx
export default function Header() {
  const { accessToken } = useAccessToken();

  const { categories } = useFetchCategories();

  const handleClickLogout = async () => {
    // TODO: 로그아웃
  };

  return (
    <Container>
      <h1>Shop</h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
            {!!categories.length && (
              <ul>
                {categories.map((category) => (
                  <li key={category.id}>
                    <Link to={`/products?categoryId=${category.id}`}>
                      {category.name}
                    </Link>
                  </li>
                ))}
              </ul>
            )}
          </li>
          {accessToken ? (
            <>
              <li>
                <Link to="/cart">Cart</Link>
              </li>
              <li>
                <Button onClick={handleClickLogout}>
                  Logout
                </Button>
              </li>
            </>
          ) : (
            <li>
              <Link to="/login">Login</Link>
            </li>
          )}
        </ul>
      </nav>
    </Container>
  );
}
```

- 훅을 mocking해서 테스트할 수 있다.

```tsx
let accessToken = '';

jest.mock('../hooks/useAccessToken', () => () => ({
  get accessToken() {
    return accessToken;
  },
}));

jest.mock('../hooks/useFetchCategories', () => () => ({
  categories: [],
}));

const context = describe;

describe('Header', () => {
  it('renders title', () => {
    render(<Header />);

    screen.getByText(/Shop/);
  });

  it('renders basic link', () => {
    render(<Header />);

    screen.getByRole('link', { name: 'Home' });
  });

  context("when the current user isn't logged in", () => {
    beforeEach(() => {
      accessToken = '';
    });

    it('renders “Login” link', () => {
      render(<Header />);

      screen.getByRole('link', { name: 'Login' });
    });
  });

  context('when the current user is logged in', () => {
    beforeEach(() => {
      accessToken = 'ACCESS-TOKEN';
    });

    it('renders “Cart” link', () => {
      render(<Header />);

      screen.getByRole('link', { name: 'Cart' });
    });

    it('renders “Logout” button', () => {
      render(<Header />);

      screen.getByRole('button', { name: 'Logout' });
    });
  });
});
```

- `AddToCartForm` 컴포넌트도 비슷한 식으로 수정해주자.

```tsx
import { Link } from 'react-router-dom';

import styled from 'styled-components';

import Options from './Options';
import Quantity from './Quantity';
import Price from './Price';
import SubmitButton from './SubmitButton';

import useAccessToken from '../../../hooks/useAccessToken';

const Container = styled.div`
  margin-bottom: 2rem;
  line-height: 1.8;
`;

export default function AddToCartForm() {
  const { accessToken } = useAccessToken();

  if (!accessToken) {
    return (
      <Container>
        주문하려면
        {' '}
        <Link to="/login">로그인</Link>
        하세요
      </Container>
    );
  }

  return (
    <Container>
      <Options />
      <Quantity />
      <Price />
      <SubmitButton />
    </Container>
  );
}
```

- mocking 해서 테스트해주자.

```tsx
let accessToken = '';

jest.mock('../../../hooks/useAccessToken', () => () => ({
  get accessToken() {
    return accessToken;
  },
}));

const context = describe;

describe('AddToCartForm', () => {
  const [product] = fixtures.products;

  beforeEach(async () => {
    iocContainer.clearInstances();

    const productDetailStore = iocContainer.resolve(ProductDetailStore);
    await productDetailStore.fetchProduct({ productId: product.id });
  });

  context("when the current user isn't logged in", () => {
    beforeEach(() => {
      accessToken = '';
    });

    it('renders message', () => {
      const { container } = render(<AddToCartForm />);

      expect(container).toHaveTextContent('주문하려면 로그인하세요');
    });
  });

  context('when the current user is logged in', () => {
    beforeEach(() => {
      accessToken = 'ACCESS-TOKEN';
    });

    it('renders “Add To Cart” button', async () => {
      render(<AddToCartForm />);

      fireEvent.click(screen.getByText('장바구니에 담기'));

      await waitFor(() => {
        screen.getByText(/장바구니에 담았습니다/);
      });
    });
  });
});
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 개인적으로 나는 mocking 해서 `context` 안에서 beforeEach로 설정하는 것도 좋지만 `given2` 라이브러리를 사용해서 쉽게 return 값을 수정하는 것이 더 간단하고 편한 것 같다.
- [given2](https://github.com/tatyshev/given2)

## 🚘 1-3. 로그인3(AccessToken 사용과 확인)

- `ApiService` 에 setAccessToken 메서드를 추가해서 API 호출할 때 AccessToken을 전달하게 만들자. Axios 인스턴스를 다시 만들어주면 다른 메서드를 수정하지 않아도 된다.

```tsx
private accessToken = '';

setAccessToken(accessToken: string) {
  if (accessToken === this.accessToken) {
    return;
  }

  const authorization = accessToken ? `Bearer ${accessToken}` : undefined;

  this.instance = axios.create({
    baseURL: API_BASE_URL,
    headers: { Authorization: authorization },
  });

  this.accessToken = accessToken;
}
```

- `useAccessToken` 에서 메서드를 호출해서 간단히 적용시키자.

```tsx
export default function useAccessToken() {
  const [accessToken, setAccessToken] = useLocalStorage('accessToken', '');

  useEffect(() => {
    apiService.setAccessToken(accessToken);
  }, [accessToken]);

  return { accessToken, setAccessToken };
}
```
