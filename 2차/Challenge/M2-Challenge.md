# ๐


### Challenge Mission

---

1. localStorage์ todoData ๊ฐ์ ๋ด์์ ํ์ด์ง๋ฅผ refresh ํด๋ todoData๊ฐ ๊ณ์ ๋จ์ ์์ ์ ์๊ฒ ํด ์ฃผ์ธ์.  
<p align="center">
    <img src="https://user-images.githubusercontent.com/82005305/156797994-a1897f27-4880-4ba7-847e-9b7859af82b7.gif">
</p>  

<br/>   
    
- App.js์ List.js๋ฅผ ์์ฑํ์ธ์
    - App.js  
    ```jsx
    import React, { useCallback, useState, useEffect } from "react";
    import Lists from "./Components/Lists";
    import Form from "./Components/Form";
    import "./App.css";

    export default function App() {
      console.log("App is Rendering");

      //  localStorage์ ์ ์ฅ๋ ์ ๋ณด ๋ฐ์์ค๊ธฐ
      const [todoData, setTodoData] = useState(
        () => JSON.parse(window.localStorage.getItem("todoData")) || []
      );

      // localStorage์ ์ ๋ณด์ ์ฅ
      useEffect(() => {
        window.localStorage.setItem("todoData", JSON.stringify(todoData));
      }, [todoData]);

      const [value, setValue] = useState("");

      const handleClick = useCallback(
        (id) => {
          let newTodoData = todoData.filter((data) => data.id !== id);
          setTodoData(newTodoData);
        },
        [todoData]
      );

      const handleSubmit = (e) => {
        // form ์์ input์ ์ ์กํ  ๋ ํ์ด์ง ๋ฆฌ๋ก๋ ๋๋ ๊ฒ์ ๋ง์์ค๋ค.
        e.preventDefault();

        // ์๋ก์ด ํ  ์ผ ๋ฐ์ดํฐ
        let newTodo = {
          id: Date.now(),
          title: value,
          completed: false,
        };

        // ์๋ ์๋ ํ  ์ผ์ ์๋ก์ด ํ  ์ผ ๋ํด์ฃผ๊ธฐ
        // ์๋ ฅ๋์ ์๋ ๊ธ์จ ์ง์์ฃผ๊ธฐ
        setTodoData((prev) => [...prev, newTodo]);
        setValue("");
      };

      // ๋ชจ๋ ์ง์ฐ๊ธฐ ๋ฒํผ ํจ์
      const handleRemoveClick = () => {
        setTodoData([]);
      };

      return (
        <div className="flex items-center justify-center w-screen h-screen bg-pink-100">
          <div className="w-full p-10 m-4 bg-white rounded shadow-2xl lg:w-3/4 lg:max-w-lg">
            <div className="flex justify-between mb-20">
              <h1 className="text-4xl font-bold text-gray-700">Todo List๐โโ๏ธ</h1>
              <button
                onClick={handleRemoveClick}
                className="px-2 py-2 bg-violet-400 rounded-full text-white hover:bg-violet-500 hover:text-white shadow-lg"
              >
                delete all
              </button>
            </div>

            <Lists
              handleClick={handleClick}
              todoData={todoData}
              setTodoData={setTodoData}
            />

            <Form handleSubmit={handleSubmit} value={value} setValue={setValue} />
          </div>
        </div>
      );
    }
    ```  
    
     - List.js  
    ```jsx
    import React, { useState } from "react";

    const List = React.memo(
      ({
        id,
        title,
        completed,
        todoData,
        setTodoData,
        provided,
        snapshot,
        handleClick,
      }) => {
        console.log("List is Rendering");

        // State
        const [isEdting, setIsEditing] = useState(false);
        const [editedTitle, setEditedTitle] = useState(title);

        // ํ  ์ผ ์๋ฃ ํ์ ๊ธฐ๋ฅ
        const handleCompleteChange = (id) => {
          let newTodoData = todoData.map((data) => {
            if (data.id === id) {
              data.completed = !data.completed;
            }
            return data;
          });
          setTodoData(newTodoData);
        };

        // ํ  ์ผ ์์ ๊ธฐ๋ฅ
        const handleEditChange = (e) => {
          setEditedTitle(e.target.value);
        };

        // ์์ ๋ ๋ด์ฉ์ผ๋ก ๋ณ๊ฒฝ๋์ด ๋ณด์ด๊ฒ ํ๋ ๊ธฐ๋ฅ
        const handleSubmit = () => {
          let newTodoData = todoData.map((data) => {
            if (data.id === id) {
              data.title = editedTitle;
            }
            return data;
          });

          setTodoData(newTodoData);
          setIsEditing(false);
        };

        // ์์ ์ค ์ผ ๋
        if (isEdting) {
          return (
            <div className="flex items-center justify-between w-full px-4 bg-gray-100  py-1 my-2 text-gray-500 border rounded shadow-md">
              <div className="items-center">
                <form onSubmit={handleSubmit}>
                  <input
                    className="w-full px-3 py-2 mr-4 text-gray-500"
                    autoFocus="true"
                    value={editedTitle}
                    onChange={handleEditChange}
                  />
                </form>
              </div>
              <div>
                <button
                  className="px-4 py-1 float-right text-xl font-bold "
                  onClick={() => setIsEditing(false)}
                >
                  โบ
                </button>
                <button
                  type="submit"
                  className="px-4 py-2 float-right"
                  onClick={handleSubmit}
                >
                  ๐พ
                </button>
              </div>
            </div>
          );
          // ํ์ ๋ณด์ด๋ ๋ชจ์ต & ์์ ์๋ฃ ๋ชจ์ต
        } else {
          return (
            <div
              key={id}
              {...provided.draggableProps}
              ref={provided.innerRef}
              {...provided.dragHandleProps}
              className={`${
                snapshot.isDragging ? "bg-gray-400" : "bg-gray-100"
              } flex items-center justify-between w-full px-4 py-1 my-2 text-gray-500 border rounded shadow-md`}
            >
              <div className="items-center">
                <input
                  type="checkbox"
                  defaultChecked={completed}
                  onChange={() => handleCompleteChange(id)}
                />{" "}
                <span className={completed ? "line-through" : undefined}>
                  {title}
                </span>
              </div>
              <div>
                <button
                  className="px-2 py-2 float-right"
                  onClick={() => handleClick(id)}
                >
                  โ
                </button>
                <button
                  className="px-4 py-2 float-right"
                  onClick={() => setIsEditing(true)}
                >
                  โ
                </button>
              </div>
            </div>
          );
        }
      }
    );

    export default List;
    ```





<br/>  

[๐ ์ ์ฒด์ฝ๋(react-todo-app repository)](https://github.com/Jeongmmin/react-todo-app)  


[๐ ๋ฉ์ธํ์ด์ง๋ก ๋์๊ฐ๊ธฐ](/README.md)
