# 🚀
### Basic Misson

---

1. 할 일 목록 제목 옆에 새로운 버튼을 만들어서 그 버튼을 누를 시에 모든 할 일이 지워지는 기능을 만드세요. (함수 컴포넌트를 사용합니다.)
    
- code
    
    ```jsx
    <button onClick={handleRemoveClick} className="px-2 py-2 bg-violet-400 rounded-full text-white hover:bg-violet-500 hover:text-white">delete all</button>
    
    // 모두 지우기 버튼 함수
    const handleRemoveClick = () => {
        setTodoData([]);
    };
    ```
    
- UI & 실행 모습
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/156791554-2a6b9232-251c-46d9-b93a-7dfe158be7ba.gif">
</p>    
<!--     ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/82850366-3248-4048-a289-6267d6dabc0e/Untitled.png) -->
    

2. 할 일 목록을 수정하는 기능을 만들어 주세요.  
  a. List.js를 작성하세요.
   
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

- UI & 실행 모습
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/156792764-f7e3d138-edf2-45cd-bf59-2f17798ba7d8.gif">
</p> 

<br/>  

[🔗 전체코드(react-todo-app repository)](https://github.com/Jeongmmin/react-todo-app)  

[📑 메인페이지로 돌아가기](/README.md)

