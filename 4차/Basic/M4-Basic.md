# Basic Mission
>  
  <details>
    <summary>4차 Basic 미션</summary>
  <div markdown="1">
  <p align="center">
  <img src="https://user-images.githubusercontent.com/82005305/163388043-aaa87cb8-195f-4701-9d87-54b917a8e3d4.png">
</p>
  </div>
  </details>  

<br/>
<br/>

- 실행화면
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/161424373-f61ed172-b93d-413e-a113-01340fa36183.gif">
</p>

<br/>
<br/>

- Counter.js
```js
    import React, { useState } from 'react';
    import './Counter.css';

    const Counter = () => {
        const [count, setCount] = useState(0);
        const [disabled, setDisabled] = useState(true);

        const add =() => {
            setCount(count + 1);
        };

        const minus = () => {
            setCount(count - 1);
        }

        const toggleHandler = () => {
            setDisabled(!disabled);
            if (!disabled) {
                console.log("on");

            }else {
                console.log("off");

            }
        }



        return (
            <div>
                <p className="counterNum">{count}</p>
                <button disabled={!disabled} onClick={minus} className="minusBtn">➖</button>
                <button disabled={!disabled} onClick={add} className="plusBtn">➕</button><br/>
                <button onClick={toggleHandler} className="on_offBtn" style={{  backgroundColor: "cornflowerblue"}}>On / Off</button>
            </div>
        )
    };

    export default Counter;
```

- App.test.js
```js
  import { fireEvent, render, screen } from '@testing-library/react';
  import App from './App';
  import Counter from './components/Counter';


  test("the counter starts at 0", () => {
    render(<App />);
    expect(screen.getByText('0')).toBeTruthy();
  });

  test("minus button has correct text", () => {
    render(<Counter />);
      const minusBtn = screen.getByRole("button", {name: '➖'});
      expect(minusBtn).toHaveTextContent("➖");

  });

  test("plus button has correct text", () => {
      render(<Counter />);
    const plusBtn = screen.getByRole("button", {name: '➕'});
    expect(plusBtn).toHaveTextContent("➕");
  });

  test("When the + button is pressed, the counter changes to 1", () => {
    render(<Counter />);
    const plusBtn = screen.getByRole("button", {name: '➕'});
    fireEvent.click(plusBtn);
    expect(screen.getByText('1')).toBeTruthy();

  });

  test("When the - button is pressed, the counter changes to -1", () => {
    render(<Counter />);
    const minusBtn = screen.getByRole("button", {name: '➖'});
    fireEvent.click(minusBtn);
    expect(screen.getByText('-1')).toBeTruthy();

  });

  test("on/off button has blue color", () => {
    render(<Counter />);
    const onOffBtn = screen.getByRole("button", {name: 'On / Off'});
    expect(onOffBtn).toHaveStyle({backgroundColor: 'cornflowerblue'});
  });

  test("Prevent the -,+ button from being pressed when the on/off button is clicked", () => {
    render(<Counter />);
    const onOffBtn = screen.getByRole("button", {name: 'On / Off'});
    const plusBtn = screen.getByRole("button", {name: '➕'});
    const minusBtn = screen.getByRole("button", {name: '➖'});
    fireEvent.click(onOffBtn);
    expect(plusBtn && minusBtn).toHaveAttribute("disabled");
  });
```

<br/>
<br/>  

[📑 메인페이지로 돌아가기](/README.md)

