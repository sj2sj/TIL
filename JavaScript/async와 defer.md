
<br/>


\<script> 요소를 사용할 때 어떤 속성을 사용하는 것이 더 효율적인가?

<br/>

## 1. 아무 속성도 사용하지 않은 \<script>

~~~javascript
<script src="script.js">
~~~


<br/><hr><br/>


## 2. async

~~~javascript
<script async src="script.js">
~~~

스크립트에 의존성이 없고, 실행 순서가 상관 없는 경우


<br/><hr><br/>

## 3. defer

~~~javascript
<script defer src="script.js">
~~~

스크립트에 의존성이 존재하고, 실행순서에 영향을 받는 경우