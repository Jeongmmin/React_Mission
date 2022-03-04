# 🚀


### Challenge Mission

---

1. localStorage에 todoData 값을 담아서 페이지를 refresh 해도 todoData가 계속 남아 있을 수 있게 해 주세요.  
<p align="center">
    <img src="https://user-images.githubusercontent.com/82005305/156795459-24fdbff6-d777-4403-b995-c6d26039c80d.gif">
</p>  

<br/>   
    
- App.js와 List.js를 작성하세요
    - App.js  
    ```jsx
    import React, { useCallback, useState, useEffect } from "react";
    import Lists from "./Components/Lists";
    import Form from "./Components/Form";
    import "./App.css";

    export default function App() {
      console.log("App is Rendering");

      //  localStorage에 저장된 정보 받아오기
      const [todoData, setTodoData] = useState(
        () => JSON.parse(window.localStorage.getItem("todoData")) || []
      );

      // localStorage에 정보저장
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
        // form 안에 input을 전송할 떄 페이지 리로드 되는 것을 막아준다.
        e.preventDefault();

        // 새로운 할 일 데이터
        let newTodo = {
          id: Date.now(),
          title: value,
          completed: false,
        };

        // 원래 있던 할 일에 새로운 할 일 더해주기
        // 입력란에 있던 글씨 지워주기
        setTodoData((prev) => [...prev, newTodo]);
        setValue("");
      };

      // 모두 지우기 버튼 함수
      const handleRemoveClick = () => {
        setTodoData([]);
      };

      return (
        <div className="flex items-center justify-center w-screen h-screen bg-pink-100">
          <div className="w-full p-10 m-4 bg-white rounded shadow-2xl lg:w-3/4 lg:max-w-lg">
            <div className="flex justify-between mb-20">
              <h1 className="text-4xl font-bold text-gray-700">Todo List🏃‍♀️</h1>
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

        // 할 일 완료 표시 기능
        const handleCompleteChange = (id) => {
          let newTodoData = todoData.map((data) => {
            if (data.id === id) {
              data.completed = !data.completed;
            }
            return data;
          });
          setTodoData(newTodoData);
        };

        // 할 일 수정기능
        const handleEditChange = (e) => {
          setEditedTitle(e.target.value);
        };

        // 수정된 내용으로 변경되어 보이게 하는 기능
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

        // 수정중 일 때
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
                  ↺
                </button>
                <button
                  type="submit"
                  className="px-4 py-2 float-right"
                  onClick={handleSubmit}
                >
                  💾
                </button>
              </div>
            </div>
          );
          // 평소 보이는 모습 & 수정완료 모습
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
                  ✖
                </button>
                <button
                  className="px-4 py-2 float-right"
                  onClick={() => setIsEditing(true)}
                >
                  ✏
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

[🔗 전체코드(react-todo-app repository)](https://github.com/Jeongmmin/react-todo-app)  


[📑 메인페이지로 돌아가기](/README.md)
