# 1차 Challenge 과제

브라우저에서 4000번 포트로 접근했을 때(localhost:4000) 도커 컨테이너 안에서 3000번 포트를 이용해서 실행되고 있는 리액트 앱에 접근 할 수 있게 해주세요.  
(도커 파일과 그 파일을 이용해서 리액트 앱을 실행하는 명령어를 작성해주세요.)

<br/>
<br/>
<br/>  

> ## 1. 도커파일 작성  
  <p align="center">
    <img src="https://user-images.githubusercontent.com/82005305/155272779-c48471c3-7c5b-4de1-92dd-aa59f15096a5.png">
  </p>  


<br/>
<br/>
<br/>

> ## 2. 명령어를 이용한 이미지 빌드 및 컨테이너 실행
  ```
  docker build ./
  docker run -p 4000:3000 <docker image id> 
  ```
  <p align="center">
    <img src="https://user-images.githubusercontent.com/82005305/155273465-4cea9536-282c-4b97-a358-b33791bb94d4.png">
  </p> 
  
  <p align="center">
    <img src="https://user-images.githubusercontent.com/82005305/155273579-0897928d-04e2-40e9-9130-ef1f05c3c0d9.png">
  </p> 
  
  
<br/>
<br/>
<br/>
  

[📑 메인페이지로 돌아가기](/README.md)
  

