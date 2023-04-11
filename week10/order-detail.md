# 🌈 4. 주문 목록 & 주문 상세

## 🚘 4-1. 주문 목록 확인하기

- 주문 목록 API에 맞춘 types를 추가하자.

```tsx
export type OrderSummary = {
  id: string;
  title: string;
  totalPrice: number;
  status: string;
  orderedAt: string;
}
```

- `APIService` 에 fetchOrders 메서드를 추가하자.

```tsx
async fetchOrders(): Promise<OrderSummary[]> {
  const { data } = await this.instance.get('/orders');
  const { orders } = data;
  return orders;
}
```

- `OrderListStore` 를 추가하자.

```tsx
@singleton()
@Store()
export default class OrderListStore {
  orders: OrderSummary[] = [];

  async fetchOrders() {
    this.setOrders([]);

    const orders = await apiService.fetchOrders();

    this.setOrders(orders);
  }

  @Action()
  setOrders(orders: OrderSummary[]) {
    this.orders = orders;
  }
}
```

- 스토어를 사용하는 훅도 하나 만들어주자.

```tsx
export default function useFetchOrders() {
  const store = container.resolve(OrderListStore);

  const [{ orders }] = useStore(store);

  useEffect(() => {
    store.fetchOrders();
  }, [store]);

  return { orders };
}
```

- `OrderListPage` 생성

```tsx
export default function OrderListPage() {
  const { orders } = useFetchOrders();

  return (
    <div>
      <h2>주문 목록</h2>
      <Orders orders={orders} />
    </div>
  );
}
```

- 라우트와 헤더에 링크를 추가한다.
- `Orders` 컴폰넌트 작성하자.(사실 나는 개인적이로 s만 붙이면 이름이 햇갈려서 `OrderList` 같은 이름을 선호하는 편이다.)

```tsx
const Container = styled.div`
  li {
    margin-block: .5rem;
    padding: 1rem;
    background: #EEE;
  }

  a {
    display: block;
    text-decoration: none;
  }
`;

type OrdersProps = {
  orders: OrderSummary[];
}

export default function Orders({ orders }: OrdersProps) {
  if (!orders.length) {
    return null;
  }

  return (
    <Container>
      <ul>
        {orders.map((order) => (
          <li key={order.id}>
            <Link to={`/orders/${order.id}`}>
              <Order order={order} />
            </Link>
          </li>
        ))}
      </ul>
    </Container>
  );
}
```

- `Order` 컴포넌트를 작성하자.

```tsx
const Container = styled.div`
  line-height: 1.5;
`;

type OrderProps = {
  order: OrderSummary;
}

export default function Order({ order }: OrderProps) {
  return (
    <Container>
      <div>
        주문 일시:
        {' '}
        {order.orderedAt}
      </div>
      <div>
        주문 코드:
        {' '}
        {order.id}
      </div>
      <div>
        상품:
        {' '}
        {order.title}
      </div>
      <div>
        결제 금액:
        {' '}
        {numberFormat(order.totalPrice)}
        원
      </div>
    </Container>
  );
}
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 항상 작업을 할 때 UI부터 작업하고 비즈니스 로직을 나중에 작성하고 옮기고 리펙터링 하는 작업을 진행했었는데 강의를 보면 항상 비슷한 순서로 API 만들어주고 Store만들고 훅으로 따로 분리하고 작업을 진행한다. 물론 강의여서 그런것도 있겠지만 비슷한 순서로 작업하게 되면 ui를 개발할때 비즈니스 로직을 생각하지 않고 편하게 작업할 수 있겠다는 생각이 들고, 테스트 하기도 hook따로 테스트하면 되서 훨씬 간단할 거 같다는 생각이 들었다.

## 🚘 4-2. 주문 상세 확인하기

- 주문 상세 API에 맞춘 type을 추가하자.

```tsx
export type OrderDetail = {
  id: string;
  title: string;
  lineItems: LineItem[];
  totalPrice: number;
  status: string;
  orderedAt: string;
}
```

- `APIService` 에 fetchOrder 메서드 추가하자.

```tsx
async fetchOrder({ orderId }: {
  orderId: string;
}): Promise<OrderDetail> {
  const { data } = await this.instance.get(`/orders/${orderId}`);
  return data;
}
```

- `OrderDetailStore` 를 추가하자.

```tsx
@singleton()
@Store()
export default class OrderDetailStore {
  order: OrderDetail = nullOrderDetail;

  loading = true;

  error = false;

  async fetchOrder({ orderId }: {
    orderId: string;
  }) {
    this.startLoading();

    try {
      const order = await apiService.fetchOrder({ orderId });
      this.setOrder(order);
    } catch {
      this.setError();
    }
  }

  @Action()
  private startLoading() {
    this.order = nullOrderDetail;
    this.loading = true;
    this.error = false;
  }

  @Action()
  private setOrder(order: OrderDetail) {
    this.order = order;
    this.loading = false;
    this.error = false;
  }

  @Action()
  private setError() {
    this.order = nullOrderDetail;
    this.loading = false;
    this.error = true;
  }
}
```

- NullObject를 준비하자.

```tsx
export const nullOrderDetail: OrderDetail = {
  id: '',
  title: '',
  status: '',
  lineItems: [],
  totalPrice: 0,
  orderedAt: '',
};
```

- 주문 상세 정보를 얻는 Hook을 만든다.

```tsx
export default function useFetchOrder({ orderId }: {
  orderId: string;
}) {
  const store = container.resolve(OrderDetailStore);

  const [{ order, loading, error }] = useStore(store);

  useEffect(() => {
    store.fetchOrder({ orderId });
  }, [store]);

  return { order, loading, error };
}
```

- `OrderDetailPage` 를 만든다.

```tsx
export default function OrderDetailPage() {
  const params = useParams();

  const { order, loading, error } = useFetchOrder({
    orderId: String(params.id),
  });

  if (loading) {
    return (
      <p>Loading...</p>
    );
  }

  if (error) {
    return (
      <p>Error!</p>
    );
  }

  return (
    <Order order={order} />
  );
}
```

- `Order` 컴포넌트를 만든다.

```tsx
const Container = styled.div`
  dl {
    display: flex;
    flex-wrap: wrap;
    line-height: 1.7;

    dt {
      width: 8rem;
    }

    dd {
      width: calc(100% - 8rem);
    }
  }
`;

type OrderProps = {
  order: OrderDetail;
};

export default function Order({ order }: OrderProps) {
  if (!order.lineItems.length) {
    return null;
  }

  return (
    <Container>
      <dl>
        <dt>주문 일시</dt>
        <dd>{order.orderedAt}</dd>
        <dt>주문 코드</dt>
        <dd>{order.id}</dd>
      </dl>
      <Table
        lineItems={order.lineItems}
        totalPrice={order.totalPrice}
      />
    </Container>
  );
}
```

- `ListItem` 을 이용해 상품목록을 보여주는 부분은 장바구니 보여줄 때 만든 `LineItemView` 와 `Options` 컴포넌트를 재사용하면 좋다. `CartView` 컴포넌트에서 `Table` 을 추출하자.

```tsx
type CartViewProps = {
  cart: Cart;
};

export default function CartView({ cart }: CartViewProps) {
  if (!cart.lineItems.length) {
    return (
      <p>장바구니가 비었습니다</p>
    );
  }

  return (
    <Table
      lineItems={cart.lineItems}
      totalPrice={cart.totalPrice}
    />
  );
}
```

- `Table` 컴포넌트는 다음과 같이 추출된다.

```tsx
const Container = styled.div`
  table {
    margin-block: 1rem;
    width: 100%;
  }

  th, td {
    padding: .5rem;
    text-align: left;
  }
`;

type TableProps = {
  lineItems: LineItem[];
  totalPrice: number;
};

export default function Table({ lineItems, totalPrice }: TableProps) {
  if (!lineItems.length) {
    return null;
  }

  return (
    <Container>
      <table>
        <thead>
          <tr>
            <th>제품</th>
            <th>단가</th>
            <th>수량</th>
            <th>금액</th>
          </tr>
        </thead>
        <tbody>
          {lineItems.map((lineItem) => (
            <LineItemView
              key={lineItem.id}
              lineItem={lineItem}
            />
          ))}
        </tbody>
        <tfoot>
          <tr>
            <th colSpan={3}>
              합계
            </th>
            <td>
              {numberFormat(totalPrice)}
              원
            </td>
          </tr>
        </tfoot>
      </table>
    </Container>
  );
}
```

### ✅ 해당 강의에서의 배운점과 나의 생각

- 컴포넌트를 작업하면서 분리할 수 있는게 생기면 바로바로 분리해야겠다. 특히 초기개발시에 디자인이 나오지않아서 미리 개발을 하다가 나중에 공통 컴포넌트로 분리되는 경우가 있는데 조금 형태가 다를때 귀찮음에 나중으로 작업을 미루곤 했었는데 눈에 보일때 분리하는게 가장 좋은 것 같다는 생각이 들었다.
