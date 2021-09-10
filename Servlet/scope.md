
<br/>

# JSP/Servlet의 4가지 Scope

## 0. Scope란?
- JSP에서 제공하는 내장 객체 중 Application, Session, Request, Page 객체는 서로 공유되는데, 그 유효범위를 말한다.
> `범위`: Application > Session > Request > Page


<br><hr><br>

## 1. Application
- 하나의 application이 생성되고 소멸될 때 까지 유지
- 같은 application 내에서 요청되는 페이지들은 같은 객체를 공유 (application 영역)
- `ServletContext`

<br/><hr><br/>

## 2. Session
- 하나의 웹 브라우저 당 하나의 session 생성, session 종료 시 객체 반환
- 같은 웹 브라우저 내에서 요청되는 페이지들은 같은 객체를 공유 (session 영역)
- `HttpSession`

<br/><hr><br/>

## 3. Request
- 요청 발생 시 생성, 응답 시 소멸
- 모든 요청이 들어올 때 마다 Request 객체와 Response 객체가 생성됨
- `HttpServletRequest`


<br/><hr><br/>

## 4. Page
- client 요청 시 하나의 JSP 페이지가 응답
- 하나의 JSP 페이지 내에서만 객체를 공유함
- 페이지 내에서 지역변수처럼 사용
- `PageContext`

<br/><hr><br/>