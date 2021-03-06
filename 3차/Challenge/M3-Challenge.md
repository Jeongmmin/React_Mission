## Challenge Mission

>  
  <details>
    <summary>3์ฐจ Challenge ๋ฏธ์</summary>
  <div markdown="1">
   <img src="https://user-images.githubusercontent.com/82005305/163398131-c1c86b9d-a3b5-45a1-8ddf-22e450466f69.png"></img>
   <img src="https://user-images.githubusercontent.com/82005305/163398374-4d53db97-c5a4-49bc-b691-4928db7de207.gif"></img>    
   
   ```    
   ๐ก ํด๋น ๊ธฐ๋ฅ์ `swiper` ๋ผ๋ ๋ชจ๋์ ์ฌ์ฉํด ์ํ ๋ชฉ๋ก์ ๋ฐ์ด์ ๋ชฉ๋ก์ ๋ฐ๊ฟ์ฃผ๋ ๊ธฐ๋ฅ์ ๊ตฌํํฉ๋๋ค.
    
    Swiper ๋ชจ๋ ์ฌ์ดํธ : [https://swiperjs.com/get-started](https://swiperjs.com/get-started)
   ```
   2. GitHub Pages ๋ฅผ ์ด์ฉํด์ ๋ฆฌ์กํธ ์ ํ๋ฆฌ์ผ์ด์ ๋ฐฐํฌํ๊ธฐ
   ```    
   โ๐ป ์ฐธ์กฐ ์ฌ์ดํธ: [https://pages.github.com/](https://pages.github.com/)
   ```
  </div>
  </details>
  
<br/>
<br/>

- ๊ตฌํํ๋ฉด
<p align="center">
<img src="https://user-images.githubusercontent.com/82005305/159458861-e006a9f1-d07b-45e2-9247-5de7d72eafd7.gif"></img>
</p> 

- Row.js
```js
  import React, { useEffect, useState } from "react";
  import axios from "../api/axios";
  import MovieModal from "./MovieModal";
  import "./Row.css";
  // swiper ์ถ๊ฐ
  import { Swiper, SwiperSlide } from "swiper/react";
  // swiper style ์ถ๊ฐ
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
            loop={true} // loop ๊ธฐ๋ฅ์ ์ฌ์ฉํ  ๊ฒ์ธ์ง
            breakpoints={{
              1378: {
                slidesPerView: 8, // ํ๋ฒ์ ๋ณด์ด๋ ์ฌ๋ผ์ด๋ ๊ฐ์
                slidesPerGroup: 8, // ๋ช๊ฐ์ฉ ์ฌ๋ผ์ด๋ ํ ์ง
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
        {/*modal์ด ์ด๋ฆฌ๋ฉด MovieModal์ด ์ด๋ฆฐ๋ค. */}
        {modalOpen && (
          // movie ์ ๋ณด๋ฅผ ๋ฃ์ด์ค
          <MovieModal {...movieSelected} setModalOpen={setModalOpen} />
        )}
      </section>
    );
  }
```

<br/>


[๐ ๋ฉ์ธํ์ด์ง๋ก ๋์๊ฐ๊ธฐ](/README.md)
