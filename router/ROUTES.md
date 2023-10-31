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