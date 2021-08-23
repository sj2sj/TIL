
<br/>


\<script> 요소를 사용할 때 어떤 속성을 사용하는 것이 더 효율적인가?

<br/>

## 1. 아무 속성도 사용하지 않은 \<script>

   ### 1) `\<head> 안`에서 사용
   - HTML 파싱을 진행하다가 script를 태그를 발견하면 HTML 파싱이 멈추고 script를 다운로드 한다.
   - 스크립트 파일에서 DOM에 접근해야 하는 경우, 실행 순서 때문에 에러가 발생할 수 있다.
~~~javascript
<head>
    <meta charset="UTF-8">
    <title> </title>
    <script src="script.js">
</head>
~~~

<br/>

   ### 2) `\<body>의 끝`에서 사용
   - html파일을 다 읽어온 후 페이지가 실행된다. 따라서 스크립트 파일을 다 받기 전에 페이지가 보인다. 자바스크립트에 의존적인 페이지라면 의미있는 콘텐츠를 볼 수 없게된다.
~~~javascript
<body>
    ...
    ...
    <script src="script.js">
</body>
~~~

<br/><hr><br/>


## 2. `async`
- HTML 파싱을 진행하면서 script를 읽어오고, script가 준비될 때마다 script를 실행한다. <br>
    즉, script의 실행 순서를 보장할 수 없다.
- 스크립트에 의존성이 없고, 실행 순서가 상관 없는 경우 사용한다.
~~~javascript
<script async src="script.js">
~~~




<br/><hr><br/>

## 3. `defer` (추천)
- HTML 파싱을 진행하면서 script를 읽어온다. 그 후, **HTML파싱이 끝나면** script를 실행한다.
- 스크립트에 의존성이 존재하고, 실행순서에 영향을 받는 경우 사용한다.
~~~javascript
<script defer src="script.js">
~~~



<br/><hr/><br/>
## 참고한 문서
- https://blog.asamaru.net/2017/05/04/script-async-defer/