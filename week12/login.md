# 🌈 2. 로그인, 사용자 목록

## 🚘 2-1. 로그인

- 일반 페이지와 로그인 페이지를 완전히 분리해야 한다.
- 다른 페이지들의 상위인 `Layout` 에서 AccessToken을 체크하고 없으면 기다리도록 한다.

```tsx
import { Outlet } from 'react-router-dom';

import styled from 'styled-components';

import Header from './Header';

import useCheckAccessToken from '../hooks/useCheckAccessToken';

const Container = styled.div`
  margin-inline: auto;
  width: 90%;
`;

export default function Layout() {
  const ready = useCheckAccessToken();

  if (!ready) {
    return null;
  }

  return (
    <Container>
      <Header />
      <Outlet />
    </Container>
  );
}
```

- `useCheckAccessToken` 훅에서 로그인 페이지로 리다이렉션하거나 기다리라고 안내한다.

```tsx
import { useEffect, useState } from 'react';

import { useNavigate } from 'react-router-dom';

import useAccessToken from './useAccessToken';

import { apiService } from '../services/ApiService';

export default function useCheckAccessToken(): boolean {
  // Access Token 검증이 끝나면 ready가 된다.
  const [ready, setReady] = useState(false);

  const { accessToken, setAccessToken } = useAccessToken();

  const navigate = useNavigate();

  useEffect(() => {
    const fetchCurrentUser = async () => {
      try {
        await apiService.fetchCurrentUser();
        setReady(true);  // 👈 확인됨!
      } catch (e) {
        setAccessToken('');
      }
    };

    fetchCurrentUser();
  }, [accessToken, setAccessToken]);

  // 기존과 다른 부분 —---------
  useEffect(() => {
    if (!accessToken) {
      navigate('/login');
    }
  }, [accessToken, navigate]);

  return ready;
}
```

## 🚘 2-2. 사용자 목록

- 사용자 관리는 사실 대단히 복잡할 수 있다. 정보가 많기 때문에 이를 모두 다루는 것 자체도 문제고, 고객 지원 업무에서도 사용자는 핵심 축이 되기 때문에 제대로 된 사용자 관리는 정말 복잡한 일이다. 여기서는 단순한 CRUD 만 구현해보자.
- SWR을 바로 사용하지 않고 개별 자료형에 대응하기 위해 한번 감싸주는 형태로 구성한다.

```tsx
import useFetch from './useFetch';

import { User } from '../types';

export default function useFetchUsers() {
  const { data, error, loading } = useFetch<{
    users: User[];
  }>('/users');

  return {
    users: data?.users ?? [],
    error,
    loading,
  };
}
```

- useFetch 훅을 만들자.

```tsx
import useSWR from 'swr';

import { apiService } from '../services/ApiService';

const API_BASE_URL = process.env.API_BASE_URL || 'http://localhost:3000/admin';

export default function useFetch<Data>(path: string) {
  const url = `${API_BASE_URL}${path}`;

  const {
    data, error, isLoading, mutate,
  } = useSWR<Data>(url, apiService.fetcher());

  return {
    data,
    error,
    loading: isLoading,
    mutate,
  };
}
```

- axios를 사용할 때는 공식문서를 참고해보자.

```tsx
fetcher() {
  return async (url: string) => {
    const { data } = await this.instance.get(url);
    return data;
  };
}
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 복잡한 내용을 다루지 않는다면 `SWR` 을 사용해서 개발해도 될 것 같다.
