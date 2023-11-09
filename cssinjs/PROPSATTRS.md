# Props & Attrs

## Props

- 활성화 여부를 표현하거나, 특정 스타일을 잡아주고 싶을 때 유용

```jsx
import styled, {css} from 'styled-components';
import {useBoolean} from 'usehooks-ts';

type ButtonProps = {
  active: boolean;
};

function background(props: ButtonProps) {
  return props.active ? '#00F' : '#FFF';
}

const Button = styled.button<ButtonProps>`
  background: ${background};
  color: ${(props) => (props.active ? '#FFF' : '#000')};
  border: 1px solid #888;

  ${(props) => props.active && css`
    background: #F00;
  `}
`;

export default function Switch() {
  const {value: active, toggle} = useBoolean(false);

  return (
    <Button
      type='button'
      onClick={toggle}
      active={active}
    >
      On/Off
    </Button>
  );
}
```

## Attrs

- 기본 속성을 추가할 수 있음
- 불필요하게 반복되는 속성을 처리할 때 유용

```jsx
type ButtonProps = {
  type?: 'button' | 'submit' | 'reset';
  active?: boolean;
};

const Button = styled.button.attrs<ButtonProps>(props => ({
	type: props.type ?? 'button',
}))<ButtonProps>`
  background: ${(props: ButtonProps) => (props.active ? '#00F' : '#FFF')};
  color: ${props => (props.active ? '#FFF' : '#000')};
  border: 1px solid #888;

  ${props => props.active && css`
    background: #F00;
  `}
`;

export default function Switch() {
  const {value: active, toggle} = useBoolean(false);

  return (
    <Button
      type='submit'
      onClick={toggle}
      active={active}
    >
      On/Off
    </Button>
  );
}
```
