# CORS
## SOP(Same Origin Policy, 동일 출처 정책)이란
동일 출처 간의 요청과 응답만 허용하는 정책

- 다른 출처로부터 조회된 자원들의 읽기 접근을 막아 다른 출처 공격을 예방함
<br></br>
### 출처란

![cors_1.png](./image/cors_1.png)

위의 URL 구성요소 중에서 **Protocol + Host + Port** 3가지가 같으면 동일 출처라고 한다!

```sql
http://Example.com:80
http://example.com
-> ⭕️호스트는 대소문자를 구분하지 않음, 기본 포트 80이 생략되어 있으므로 동일 출처

http://example.com/app1
https://example.com/app2
-> ❌프로토콜이 다르므로 다른 출처
```

<br></br>
### 다른 출처 요청의 위험성
출처가 다른 애플리케이션에 자유롭게 접근할 수 있다면, 해커가 CSRF(Cross-Site Request Forgery)나 XSS(Cross-Site Scripting) 등의 방법을 이용해 악성 코드를 실행시킬 수 있음
- CSRF 예시
    - 공격자가 희생자의 권한을 도용하여 특정 웹 사이트의 기능을 실행하는 것
 
![cors_2.png](./image/cors_8.jpg)
   	 
- XSS 예시
    - 웹 사이트의 관리자가 아닌 악의적인 목적을 가진 제 3자가 악성 코드(대부분 자바스크립트)를 삽입하여 의도하지 않은 명령을 실행시키거나 세션 등을 탈취하는 것
  
![cors_2.png](./image/cors_2.png)

<br></br>

## CORS(Cross-Origin Resource Sharing, 교차 출처 리소스 공유)란
추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이 **다른 출처의 선택된 자원에 접근할 수 있는 권한을 부여하도록** 브라우저에 알려주는 메커니즘

- 서로 다른 출처 간 요청과 응답이 가능하게 함
- 웹 애플리케이션은 리소스가 자신의 출처(도메인, 프로토콜, 포트)와 다를 때 CORS HTTP 요청을 실행함
- CORS 요청 방식 종류로는 Simple Request, Preflighted Request, Credential Request이 있음
<br></br>

**💡 즉 CORS를 허용해줄 때는 요청을 보낸 클라이언트의 출처가 자신(서버)의 출처와 다르면서, 아래 2가지 경우에 해당하는 요청이 왔을 때!**
- Simple Request: **GET, HEAD, POST 같은 Method**와 Content-Type 헤더의 값이 text/plain, application/x-www-form-urlencoded 또는 multipart/form-data 일 때
- Non Simple Request(->Preflighted Request): **GET, HEAD, POST Method가 아닌 요청**과 Simple Request에서 언급하지 않는 Content-Type으로 요청이 들어 왔을 때    

-> 자신과 같은 출처이거나 허용되는 다른 출처의 요청만 처리함으로써 제3자가 서버에 액세스하는 것을 방지함!

<br></br>
### Simple Request (단순 요청)

![cors_4.png](./image/cors_4.png)

1. **조건**

다음 조건을 만족하면, 브라우저는 해당 CORS 요청을 Simple Request로 처리함

- HTTP Method가 GET, POST, HEAD 중 하나인 경우
- Content-Type 헤더가 다음 중 하나인 경우
    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain
- CORS-safelisted request-header를 포함하는 경우(Fetch spec)
- XMLHttpRequest.upload 에 이벤트 핸들러, 리스너가 등록되지 않은 경우
- ReadableStream 객체가 포함되지 않은 경우
<br></br>
2. **동작방식**
- 1) 사용자가 요청 헤더에 자신의 Origin을 실어서 서버로 요청을 보냄
- 2) 서버는 요청 헤더의 Origin을 확인함
- 3) CORS 요청이 유효하다면(서버의 CORS 정책에 위배되지 않는다면), 서버는 응답 헤더에 Access-Control-Allow-Origin 헤더를 추가해 사용자에게 다시 전송함
- 4) 유효하지 않다면, 브라우저는 응답을 차단함
    
    ![cors_5.png](./image/cors_5.png)
    
<br></br>
조건이 까다롭기 때문에, 위 조건을 모두 만족되어 단순 요청이 일어나는 상황은 드물다!
→ 대부분 HTTP API 요청은 text/xml 이나 application/json 으로 통신하기 때문에 Content-Type 조건이 위반되기 때문

<br></br>

### Preflighted Request (프리 플라이트)

![cors_6.png](./image/cors_6.png)

- Preflight Request는 Simple Request의 조건을 만족하지 못할 시 브라우저가 자동으로 생성함
- 클라이언트가 OPTIONS 메서드로 HTTP 요청을 미리 보내고, 서버는 실제 요청이 전송하기에 안전한지 확인함(자신의 CORS 정책에 위배되는지 확인함)
- 브라우저는 자신이 보낸 프리플라이트 요청과 서버가 응답해준 정책(서버의 CORS 정책)을 비교하여, 해당 요청이 안전한지 확인하고 본 요청을 보내게 됨
- 실제 요청에 걸리는 시간이 늘어나게 되어 애플리케이션 성능에 영향을 미치는 단점이 있음

```sql
요청 헤더
- access-control-request-method: 실제 요청이 보낼 HTTP 메서드
- access-control-request-headers: 실제 요청에 포함된 header

응답 헤더
access-control-allow-origin: 서버가 허용하는 출처
access-control-allow-methods: 서버가 허용하는 HTTP 메서드 리스트
access-control-allow-headers: 서버가 허용하는 header 리스트
access-control-max-age: 프리 플라이트 요청의 응답을 캐시에 저장하는 시간
```

<br></br>
### Credential Request (인증된 요청)

![cors_7.png](./image/cors_7.png)

- 인증된 요청은 클라이언트에서 서버에게 자격 인증 정보(Credential)를 실어 요청할때 사용되는 요청
- 자격 인증 정보란 세션 ID가 저장되어있는 쿠키 혹은 Authorization 헤더에 설정하는 토큰 값 등을 말함
  
<br></br>
기본적으로 브라우저가 제공하는 요청 API 들은 별도의 옵션 없이 브라우저의 쿠키와 같은 인증과 관련된 데이터를 함부로 요청 데이터에 담지 않도록 되어 있음

- 이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 credentials 옵션
- 서버에 인증된 요청을 보내는 방법으로는 fetch 메소드, axios 라이브러리, jQuery 라이브러리 등을 사용하는 것

```jsx
// fetch 메서드
fetch("https://example.com:1234/users/login", {
	method: "POST",
	credentials: "include",  // credentials 옵션
    body: JSON.stringify({
        userId: 1,
    }),
})
```
```jsx
// axios 라이브러리
axios.post('https://example.com:1234/users/login', { 
    profile: { username: username, password: password } 
}, { 
	withCredentials: true  // credentials 옵션
})
```
```jsx
// jQuery 라이브러리
$.ajax({
	url: "https://example.com:1234/users/login",
	type: "POST",
	contentType: "application/json; charset=utf-8",
	dataType: "json",		
	xhrFields: { 
    	withCredentials: true  // credentials 옵션
    },
	success: function (retval, textStatus) {
		console.log( JSON.stringify(retval));
	}
});
```

<br></br>

## CSP(Content Security Policy, 콘텐츠 보안 정책)란
XSS와 같이 웹 페이지 내에 악성 스크립트를 삽입하는 공격 기법을 막기 위해 사용하는 것으로, 스트립트, 이미지, 글꼴, 미디어 등 모든 동적 콘텐츠에 대해 허용된 도메인에서 로드된 콘텐츠만 실행함
- 제3자가 웹 페이지 내에 악성 스크립트를 사용하는 것을 방지함
- Content-Security-Policy 헤더를 사용해서 특정 리소스가 어디서 왔는지 검사하고, 허용된 범위에 포함됐는지 검토함
### 사용 예시
- Content-Security-Policy: default-src ‘self’
    - 모든 컨텐츠의 소스는 자기 도메인에서 갖고 오게 됨
- Content-Security-Policy: default-src ‘self’; img-src *; media-src media1.com media2.com; script-src script.example.com;
    - 모든 컨텐츠는 자기 도메인에서 갖고 오지만 몇가지 예외 사항이 있음
    - img-src *; -> 이미지 관련 컨텐츠는 모든 사이트들을 허용함
    - media-src media1.com media2.com; -> 미디어 관련 컨텐츠는 media1과 media2라는 사이트에서만 갖고 옴
    - script-src script.example.com; -> 실행 가능한 스크립트 관련 컨텐츠는 script.example.com에서만 갖고 옴

<br></br>
<br></br>

### 면접질문
1. CORS란 무엇이며 이것에 대해서 설명해보세요 → 꼬리질문     
⭐️⭐️ 실제 면접장에서 질문 받았다는 사람들이 많음!!!!!

<br></br>
### 출처
[https://ko.wikipedia.org/wiki/동일-출처_정책](https://ko.wikipedia.org/wiki/%EB%8F%99%EC%9D%BC-%EC%B6%9C%EC%B2%98_%EC%A0%95%EC%B1%85)     
[https://escapefromcoding.tistory.com/724](https://escapefromcoding.tistory.com/724)     
[https://inpa.tistory.com/entry/WEB-📚-CORS-💯-정리-해결-방법-👏#📜_동일_출처_정책이_필요한_이유](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F#%F0%9F%93%9C_%EB%8F%99%EC%9D%BC_%EC%B6%9C%EC%B2%98_%EC%A0%95%EC%B1%85%EC%9D%B4_%ED%95%84%EC%9A%94%ED%95%9C_%EC%9D%B4%EC%9C%A0)     
[https://yoo11052.tistory.com/139](https://yoo11052.tistory.com/139)     
https://w01fgang.tistory.com/147     
https://chanto11.tistory.com/67
