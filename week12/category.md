# 🌈 3. 카테고리 관리

## 🚘 3-1. 카테고리 관리

- `useFetchCategories` 훅이 createCategory 함수를 제공하게 하는데, 여기서 SWR의 캐시 초기화(사실상 refetch 요청)을 위해 mutate 함수를 호출하게 된다.
- [Bound Mutate](https://swr.vercel.app/ko/docs/mutation#bound-mutate)
- 카테고리 추가

```tsx
import useFetch from './useFetch';

import { apiService } from '../services/ApiService';

import { Category } from '../types';

export default function useFetchCategories() {
  const {
    data, error, loading, mutate,
  } = useFetch<{
    categories: Category[];
  }>('/categories');

  return {
    categories: data?.categories ?? [],
    error,
    loading,
    async createCategory({ name }: {
      name: string;
    }) {
      await apiService.createCategory({ name });
      mutate();
    },
  };
}
```

- React Hook Form을 사용한 카테고리 추가 폼

```tsx
import { useNavigate } from 'react-router-dom';

import { Controller, useForm } from 'react-hook-form';

import styled from 'styled-components';

import Button from '../components/ui/Button';

import useFetchCategories from '../hooks/useFetchCategories';

const Container = styled.div`
  h2 {
    margin-bottom: 2rem;
    font-size: 2rem;
  }

  [type=submit] {
    display: block;
    margin-block: 1rem;
  }
`;

export default function CategoryNewPage() {
  const navigate = useNavigate();

  const { createCategory } = useFetchCategories();

  type FormValues = {
    name: string;
  };

  const { handleSubmit, control } = useForm<FormValues>();

  const onSubmit = async (data: FormValues) => {
    await createCategory({
      name: data.name,
    });
    navigate('/categories');
  };

  return (
    <Container>
      <h2>New Category</h2>
      <form onSubmit={handleSubmit(onSubmit)}>
        <Controller
          control={control}
          name="name"
          defaultValue=""
          render={({ field: { onChange, value } }) => (
            <div>
              <label htmlFor="input-name">이름</label>
              <input
                id="input-name"
                value={value}
                onChange={onChange}
              />
            </div>
          )}
        />
        <Button type="submit">
          등록
        </Button>
      </form>
    </Container>
  );
}
```

- 카테고리 수정

```tsx
import useFetch from './useFetch';

import { apiService } from '../services/ApiService';

import { Category } from '../types';

export default function useFetchCategory({ categoryId }: {
  categoryId: string;
}) {
  const url = `/categories/${categoryId}`;

  const {
    data, error, loading, mutate,
  } = useFetch<Category>(url);

  return {
    category: data,
    error,
    loading,
    async updateCategory({ name, hidden }: {
      name: string;
      hidden: boolean;
    }) {
      await apiService.updateCategory({ categoryId, name, hidden });
      mutate();
    },
  };
}
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- `mutate` 를 이용한 refetch에 대해 배웠다.
- `react-hook-form` 을 사용할 때 보통 `register` 를 따로 빼서 많이 사용했는데 `Control` 을 활용해서 만들어도 좋을 것 같다는 생각이 들었다.
