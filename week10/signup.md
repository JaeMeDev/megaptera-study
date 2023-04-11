# 🌈 3. 회원가입

## 🚘 3-1. 회원가입 구현

- `ApiService` 에 signup 메서드를 추가한다.
- 회원가입시에도 `accessToken` 을 받아온다.
- 여기서는 회원가입 완료페이지로 가지만 바로 로그인시켜도 될 것 같다.

```tsx
async signup({ email, name, password }: {
  email: string;
  name: string;
  password: string;
}): Promise<string> {
  const { data } = await this.instance.post('/users', {
    email, name, password,
  });
  const { accessToken } = data;
  return accessToken;
}
```

- 로그인과 비슷한 형태로 `SignupFormStore` 를 만들어준다.

```tsx
@singleton()
@Store()
export default class SignupFormStore {
  email = '';

  name = '';

  password = '';

  passwordConfirmation = '';

  error = false;

  accessToken = '';

  get valid() {
    return this.email.includes('@')
      && !!this.name
      && this.password.length >= 4
      && this.password === this.passwordConfirmation;
  }

  @Action()
  changeEmail(email: string) {
    this.email = email;
  }

  @Action()
  changeName(name: string) {
    this.name = name;
  }

  @Action()
  changePassword(password: string) {
    this.password = password;
  }

  @Action()
  changePasswordConfirmation(password: string) {
    this.passwordConfirmation = password;
  }

  @Action()
  setAccessToken(accessToken: string) {
    this.accessToken = accessToken;
  }

  @Action()
  setError() {
    this.error = true;
  }

  @Action()
  reset() {
    this.email = '';
    this.name = '';
    this.password = '';
    this.passwordConfirmation = '';
    this.error = false;
    this.accessToken = '';
  }

  async signup() {
    try {
      const accessToken = await apiService.signup({
        email: this.email,
        name: this.name,
        password: this.password,
      });

      this.setAccessToken(accessToken);
    } catch (e) {
      this.setError();
    }
  }
}
```

- `SignupFormStore` 훅을 만들어준다.

```tsx
export default function useSignupFormStore() {
  const store = container.resolve(SignupFormStore);
  return useStore(store);
}
```

- `SignupForm` 컴포넌트를 만든다.

```tsx
export default function SignupForm() {
  const { setAccessToken } = useAccessToken();

  const [{
    email, password, passwordConfirmation, valid, error, accessToken,
  }, store] = useSignupFormStore();

  useEffect(() => {
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

  const handleChangePasswordConfirmation = (value: string) => {
    store.changePasswordConfirmation(value);
  };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    store.signup();
  };

  return (
    <Container>
      <h2>회원 가입</h2>
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
        <TextBox
          label="Password Confirmation"
          type="password"
          value={passwordConfirmation}
          onChange={handleChangePasswordConfirmation}
        />
        <Button type="submit" disabled={!valid}>
          회원 가입
        </Button>
        {error && (
          <p>회원 가입 실패</p>
        )}
      </form>
    </Container>
  );
}
```

- `SignupPage` 를 준비한다.

```tsx
export default function SignupPage() {
  const navigate = useNavigate();

  const [{ accessToken }, store] = useSignupFormStore();

  useEffect(() => {
    store.reset();
  }, []);

  useEffect(() => {
    if (accessToken) {
      store.reset();
      navigate('/signup/complete');
    }
  }, [accessToken]);

  return (
    <SignupForm />
  );
}
```

- `SignupCompletePage` 도 준비한다.(회원가입 완료)

```tsx
export default function SignupCompletePage() {
  return (
    <p>
      회원 가입이 완료되었습니다.
    </p>
  );
}
```

- 모든게 추가되면 route와 링크를 추가시켜준다.

### ✅ 해당 강의에서의 배운점과 나의 생각

- 로그인할때와 비슷한 형태로 개발해서 복습하는 느낌으로 진행할 수 있었고 현재 외부 프로젝트로 작업하는 목록이 스텝별로 회원가입을 진행하는 것인데 `Store` 를 사용하면 페이지를 나누거나 화면을 새로 렌더링해줘도 문제가 없기 때문에 `Store` 를 사용하면 좋을 것 같다는 생각이 들었다. (응용해야겠다.)
