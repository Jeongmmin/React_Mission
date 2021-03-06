# ๐
### Basic Misson

---

1. ํ  ์ผ ๋ชฉ๋ก ์ ๋ชฉ ์์ ์๋ก์ด ๋ฒํผ์ ๋ง๋ค์ด์ ๊ทธ ๋ฒํผ์ ๋๋ฅผ ์์ ๋ชจ๋  ํ  ์ผ์ด ์ง์์ง๋ ๊ธฐ๋ฅ์ ๋ง๋์ธ์. (ํจ์ ์ปดํฌ๋ํธ๋ฅผ ์ฌ์ฉํฉ๋๋ค.)
    
- code
    
    ```jsx
    <button onClick={handleRemoveClick} className="px-2 py-2 bg-violet-400 rounded-full text-white hover:bg-violet-500 hover:text-white">delete all</button>
    
    // ๋ชจ๋ ์ง์ฐ๊ธฐ ๋ฒํผ ํจ์
    const handleRemoveClick = () => {
        setTodoData([]);
    };
    ```
    
- UI & ์คํ ๋ชจ์ต
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/156791554-2a6b9232-251c-46d9-b93a-7dfe158be7ba.gif">
</p>    
<!--     ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/82850366-3248-4048-a289-6267d6dabc0e/Untitled.png) -->
    

2. ํ  ์ผ ๋ชฉ๋ก์ ์์ ํ๋ ๊ธฐ๋ฅ์ ๋ง๋ค์ด ์ฃผ์ธ์.  
  a. List.js๋ฅผ ์์ฑํ์ธ์.
   
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

- UI & ์คํ ๋ชจ์ต
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/156792764-f7e3d138-edf2-45cd-bf59-2f17798ba7d8.gif">
</p> 

<br/>  

[๐ ์ ์ฒด์ฝ๋(react-todo-app repository)](https://github.com/Jeongmmin/react-todo-app)  

[๐ ๋ฉ์ธํ์ด์ง๋ก ๋์๊ฐ๊ธฐ](/README.md)

