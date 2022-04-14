## Basic Misson
>  
  <details>
    <summary>3차 Basic 미션</summary>
  <div markdown="1">
   <img src="https://user-images.githubusercontent.com/82005305/163395515-057fdd94-5a9a-4a75-8c6e-8cd97b30305f.png"></img>
   <img src="https://user-images.githubusercontent.com/82005305/163396804-54a8141e-55ca-4884-9938-2e05db49fd6e.gif"></img>
  </div>
  </details>
  
<br/>
<br/>


- 구현화면
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/159458411-7cb2fd9e-0ab6-45d9-985b-82bacece55ea.gif"></img>
</p>


- useCloseOverlay.js
```js    
  import { useEffect } from "react";

  export const useCloseOverlay = (ref, handler) => {
    useEffect(() => {
      const handleClickOutside = (event) => {
        const el = ref?.current;
        if (!el || el.contains(event.target)) {
          return;
        }
        handler(event);
      };

      document.addEventListener("mousedown", handleClickOutside);

      return () => {
        document.removeEventListener("mousedown", handleClickOutside);
      };
    }, [ref, handler]);
  };

```

- MovieModal/index.js
```js
  // rfce
  import React, { useRef } from "react";

  // closeOverlay hooks 추가
  import { useCloseOverlay } from "../../hooks/useCloseOverlay";
  import "./MovieModal.css";

  // props 가져오기
  function MovieModal({
    backdrop_path,
    title,
    overview,
    name,
    release_date,
    first_air_date,
    vote_average,
    setModalOpen,
  }) {
    const BASE_URL = "https://image.tmdb.org/t/p/original";

    const ref = useRef(null);

    useCloseOverlay(ref, () => setModalOpen(false));

    return (
      <div className="presentation">
        <div className="wrapper-modal">
          <div className="modal" ref={ref}>
            <span onClick={() => setModalOpen(false)} className="modal-close">
              ✖
            </span>
            <img
              src={`${BASE_URL}${backdrop_path}`}
              alt="modal__poster-img"
              className="modal__poster-img"
            />

            <div className="modal__content">
              <p className="modal__details">
                <span className="modal__user-perc">100% for you</span>&nbsp;&nbsp;
                {release_date ? release_date : first_air_date}
              </p>

              <h2 className="modal__title">{title ? title : name}</h2>
              <p className="modal__overview"> 평점: {vote_average}</p>
              <p className="modal__overview"> {overview} </p>
            </div>
          </div>
        </div>
      </div>
    );
  }

export default MovieModal;

```



<!-- >  
  <details>
    <summary>3차 Basic 미션</summary>
  <div markdown="1">
   <img src=""></img>
   <img src=""></img>
  - [Basic](./1차/Basic/M1-Basic.md)  
  - [Chanllenge](./1차/Challenge/M1-Challenge.md)  

  </div>
  </details>  -->


<br/>
[📑 메인페이지로 돌아가기](/README.md)
