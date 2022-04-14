## Challenge Mission

>  
  <details>
    <summary>3차 Challenge 미션</summary>
  <div markdown="1">
   <img src="https://user-images.githubusercontent.com/82005305/163398131-c1c86b9d-a3b5-45a1-8ddf-22e450466f69.png"></img>
   <img src="https://user-images.githubusercontent.com/82005305/163398374-4d53db97-c5a4-49bc-b691-4928db7de207.gif"></img>    
   
   ```    
   💡 해당 기능을 `swiper` 라는 모듈을 사용해 영화 목록을 밀어서 목록을 바꿔주는 기능을 구현합니다.
    
    Swiper 모듈 사이트 : [https://swiperjs.com/get-started](https://swiperjs.com/get-started)
   ```
   2. GitHub Pages 를 이용해서 리액트 애플리케이션 배포하기
   ```    
   ✌🏻 참조 사이트: [https://pages.github.com/](https://pages.github.com/)
   ```
  </div>
  </details>
  
<br/>
<br/>

- 구현화면
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/159458861-e006a9f1-d07b-45e2-9247-5de7d72eafd7.gif"></img>
</p> 

- Row.js
```js
  import React, { useEffect, useState } from "react";
  import axios from "../api/axios";
  import MovieModal from "./MovieModal";
  import "./Row.css";
  // swiper 추가
  import { Swiper, SwiperSlide } from "swiper/react";
  // swiper style 추가
  import "swiper/css";
  import "swiper/css/free-mode";
  import "swiper/css/pagination";
  import "swiper/css/navigation";

  // import required modules
  import { FreeMode } from "swiper";
  import { Navigation } from "swiper";

  export default function Row({ isLargeRow, title, id, fetchUrl }) {
    const BASE_URL = "https://image.tmdb.org/t/p/original";

    const [movies, setMovies] = useState([]);
    const [modalOpen, setModalOpen] = useState(false);
    const [movieSelected, setMovieSelected] = useState({});

    useEffect(() => {
      fetchMovieData();
    }, []);

    const fetchMovieData = async () => {
      const request = await axios.get(fetchUrl);
      console.log(request);
      setMovies(request.data.results);
    };

    const handleClick = (movie) => {
      setModalOpen(true);
      setMovieSelected(movie);
    };

    const truncate = (str, n) => {
      return str?.length > n ? str.substr(0, n - 1) + "..." : str;
    };

    return (
      <section className="row ">
        {/**TITLE*/}
        <h2>{title}</h2>
        <div className="slider">
          <Swiper
            loop={true} // loop 기능을 사용할 것인지
            breakpoints={{
              1378: {
                slidesPerView: 8, // 한번에 보이는 슬라이드 개수
                slidesPerGroup: 8, // 몇개씩 슬라이드 할지
              },
              998: {
                slidesPerView: 5,
                slidesPerGroup: 5,
              },
              625: {
                slidesPerView: 4,
                slidesPerGroup: 4,
              },
              0: {
                slidesPerView: 3,
                slidesPerGroup: 3,
              },
            }}
            spaceBetween={20}
            freeMode={true}
            pagination={{
              clickable: true,
            }}
            modules={[FreeMode, Navigation]}
            navigation
            className="mySwiper"
          >
            {movies.map((movie) => (
              <SwiperSlide id={id} className="swiper-slide">
                <div>
                  <img
                    key={movie.id}
                    src={`${BASE_URL}${
                      isLargeRow ? movie.poster_path : movie.backdrop_path
                    }`}
                    className={`row__poster ${isLargeRow && "row__posterLarge"}`}
                    loading="lazy"
                    alt="{movie.name}"
                    onClick={() => handleClick(movie)}
                  />
                  <div className="titleOfMovie">
                    {truncate(movie.title ? movie.title : movie.name, 14)}
                  </div>
                </div>
                <div class="swiper-prev"></div> <div class="swiper-next"></div>
              </SwiperSlide>
            ))}
          </Swiper>
        </div>
        {/*modal이 열리면 MovieModal이 열린다. */}
        {modalOpen && (
          // movie 정보를 넣어줌
          <MovieModal {...movieSelected} setModalOpen={setModalOpen} />
        )}
      </section>
    );
  }
```

<br/>
[📑 메인페이지로 돌아가기](/README.md)
