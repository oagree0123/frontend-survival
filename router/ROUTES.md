# React Router

```bash
npm i react-router-dom
```

## Routes

### Routes 사용하기

```jsx
// src/main.tsx
function main() {
  const element = document.getElementById('root');

  if (!element) {
    return;
  }

  const root = ReactDOM.createRoot(element);
  root.render(
    <React.StrictMode>
      <BrowserRouter>
        <App />
      </BrowserRouter>,
    </React.StrictMode>,
  );
}
```

```jsx
// src/App.tsx
import { Routes, Route } from 'react-router-dom';

function App() {
  return (
    <div>
      <Header />
      <main>
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/about" element={<AboutPage />} />
        </Routes>
      </main>
      <Footer />
    </div>
  );
}
```

### 테스트에서 Routes 사용하기

```jsx
// src/App.test.tsx
describe('render App', () => {
  context('when the current path is "/"', () => {
    it('renders the home page', () => {
      render((
        <MemoryRouter initialEntries={['/']}>
          <App />
        </MemoryRouter>
      ));

      screen.getByText(/Welcome/);
    });

    context('when the current path is "/"', () => {
      it('renders the About page', () => {
        render((
          <MemoryRouter initialEntries={['/about']}>
            <App />
          </MemoryRouter>
        ));

        screen.getByText(/This is Test/);
      });
    });
  });
});
```

## Router

### Router 사용하기

```jsx
// src/components/Layout.tsx
import { Outlet } from 'react-router-dom';

function Layout() {
  return (
    <div>
      <Header />
      <Outlet />
      <Footer />
    </div>
  );
}
```

```jsx
// src/routes.tsx
const routes = [
  {
    element: <Layout />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/about', element: <AboutPage /> },
    ],
  },
];

export default routes;
```

```jsx
// src/App.tsx
const router = createBrowserRouter(routes);

export default function App() {
  return (
    <RouterProvider router={router} />
  );
}
```

### 테스트에서 Router 사용하기

```jsx
// src/routes.test.tsx
import {render, screen} from '@testing-library/react';
import {RouterProvider, createMemoryRouter} from 'react-router-dom';
import routes from './routes';

const context = describe;

describe('render App', () => {
  function renderRouter(path: string) {
    const router = createMemoryRouter(routes, {initialEntries: [path]});
    render((
      <RouterProvider router={router} />
    ));
  }

  context('when the current path is "/"', () => {
    it('renders the home page', () => {
      renderRouter('/');

      screen.getByText(/Welcome/);
    });

    context('when the current path is "/"', () => {
      it('renders the About page', () => {
        renderRouter('/about');

        screen.getByText(/This is Test/);
      });
    });
  });
});

```

## Navigation

### Web APIs - History

- 브라우저의 세션 기록
- 현재 페이지를 불러온 탭 또는 프레임의 방문 기록을 조작할 수 있는 방법을 제공

```jsx
// 세션 기록의 바로 뒤, 앞 페이지로 이동
history.back();
history.forward();

// 세션에서 특정한 페이지를 로딩
history.go();

// 브라우저의 세션 기록 스택에 항목을 추가
history.pushState(state, title, url);
```

```jsx
const state = {};
const title = '';
const url = '/about';

history.pushState(state, title, url);
```

### Link

```jsx
function Header() {

return (
  <header>
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
      </ul>
    </nav>
  </header>
  );
}
```

### NavLink

```jsx
function Header() {
  return (
    <header>
      <nav>
        <ul>
          <li><NavLink to="/">Home</NavLink></li>
          <li><NavLink to="/about">About</NavLink></li>
        </ul>
      </nav>
    </header>
  );
}
```

### Navigate

```jsx
import { Navigate } from 'react-router-dom';

export default function LoginPage() {
  return (
    <Navigate to="/" />
  );
}
```

### useNavigate

```jsx
const navigate = useNavigate();

const handleClickLogout = () => {
  navigate('/logout');
};

<button type='button' onClick={handleClickLogout}>
  Log out
</button>
```
