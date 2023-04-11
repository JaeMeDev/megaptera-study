# 🌈 2. 로그아웃

## 🚘 2-1. Access Token 확인

- 사이트 접속 or 새로 고침했을 때 Access Token을 확인하고 사용자를 얻는 작업이 필요하다. 만약 회원정보를 얻는 API가 실패하면 Access Token에 문제가 있는 상황이니 로그아웃 시킨다.
- `APIService` 에 fetchCurrentUser 메서드 추가.

```tsx
async fetchCurrentUser(): Promise<{
  id: string;
  name: string;
}> {
  const { data } = await this.instance.get('/users/me');
  const { id, name } = data;
  return { id, name };
}
```

- `useCheckAccessToken` 훅을 만든다.

```tsx
export default function useCheckAccessToken(): void {
  const { accessToken, setAccessToken } = useAccessToken();

  useEffect(() => {
    const fetchCurrentUser = async () => {
      try {
        await apiService.fetchCurrentUser();
      } catch (e) {
        setAccessToken('');
      }
    };

    fetchCurrentUser();
  }, [accessToken, setAccessToken]);
}
```

- 가장 밖에서 useCheckAccessToekn을 호출한다.

```tsx
export default function Layout() {
  useCheckAccessToken();

  return (
    <Container>
      <Header />
      <Outlet />
    </Container>
  );
}
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 해당 로직을 응용하면 AccessToekn 을 갱신시키는 작업도 할 수 있을 것 같다는 생각이 들었다.

## 🚘 2-2. 로그아웃 구현하기

- `APIService` 에서 로그아웃 API를 호출하는 메서드를 추가한다.

```tsx
async logout(): Promise<void> {
  await this.instance.delete('/session');
}
```

- 헤더 컴포넌트에서 로그아웃 버튼을 수정하자.

```tsx
export default function Header() {
  const navigate = useNavigate();

  const { accessToken, setAccessToken } = useAccessToken();

  const { categories } = useFetchCategories();

  const handleClickLogout = async () => {
    await apiService.logout();
    setAccessToken('');
    navigate('/');
  };

  // …(중략)…
}
```

- `react-router-dom` 을 mocking해서 테스트하자.

```tsx
import { screen, fireEvent, waitFor } from '@testing-library/react';

import { render } from '../test-helpers';

import Header from './Header';

const navigate = jest.fn();

let accessToken = '';
const setAccessToken = jest.fn();

jest.mock('react-router-dom', () => ({
  ...jest.requireActual('react-router-dom'),
  useNavigate: () => navigate,
}));

jest.mock('../hooks/useAccessToken', () => () => ({
  accessToken,
  setAccessToken,
}));

jest.mock('../hooks/useFetchCategories', () => () => ({
  categories: [],
}));

const context = describe;

describe('Header', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

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

    it('renders “Logout” button', async () => {
      render(<Header />);

      const button = screen.getByRole('button', { name: 'Logout' });

      fireEvent.click(button);

      await waitFor(() => {
        expect(setAccessToken).toBeCalledWith('');
        expect(navigate).toBeCalledWith('/');
      });
    });
  });
});
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 로그아웃 할 때 localStorage만 비워줄거라 생각했는데 api로 요청하는게 새로웠다.
- 그동안 외부라이브러리 mocking시 필요한부분 뿐만아니라 다른 함수들도 깨져서 불필효하게 모킹을 했었는데 `jest.requireActual` 를 알게 되어 꼭 활용해야 겠다.
