---
layout: post
title: JavaScript 기본만 빠르게 살펴보기
subtitle: 니꼴라스 쌤의 JS 강의로 빠르게 JS를 훑어보자!
author: weejw
categories: JavaScript

tags: [JavaScript]
---

토이 프로젝트를 위해서 JS를 새로 공부하기로 했다. [니꼴라스 쌤의 강의](https://nomadcoders.co/javascript-for-beginners) 를 듣고 나에게 필요한 것들만 메모하여 다시 열어보기 위한 글이다. <br>


- 니꼴라스쌤이 생각하는 element를 가져오는 가장 멋진 방법 
css 방식으로 element 찾는 것이 가능하다. <br>
- 
```javascript

// class name이 hello이고 element가 h1일 때
document.querySelector(".hello h1")
document.querySelectorAll(".hello h1")

// id의 경우
document.querySelectorAll("#hello")

```

- 이벤트 처리 방법
아래 코드처럼 element를 가져오고 해당 element에 event를 추가할 수 있다. <br>

```javascript

const title = document.querySelector("div.hello:first-child h1")

function handleTitleClick(){
    title.style.color="blue";
}

function handleMouseEnter(){
    title.innerText = "Mouse is here!";
}

function handleMouseLeave(){
    title.innerText = "Mouse is gone!"
}

title.addEventListener("click", handleTitleClick)
// 아래처럼 사용도 가능
title.onclick("click", handleTitleClick())
title.addEventListener("mouseenter", handleMouseEnter)
title.addEventListener("mouseleave", handleMouseLeave)

```

- 윈도우에 함수 추가하기

```javascript

window.addEventListener("resize", handleWindowResize)
window.addEventListener("copy", handlewindowCopy)
window.addEventListener("offline", handleWindowOffline)
window.addEventListener("online", handleWindowOnline)

```
