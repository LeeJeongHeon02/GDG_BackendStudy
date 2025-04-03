![image](https://github.com/user-attachments/assets/c6e61c01-c555-4016-a461-f1b585a5ba44)
## 프로토콜과 HTTP

- 프로토콜 : 네트워크 안에서 요청과 응답을 보내는 규칙
- HTTP를 통해 요청을 보낼 때는 HTTP Method, URL이 필요하다.

### 자주 사용하는 HTTP Method

- GET : 데이터를 가져온다 : 조회
- POST : 데이터를 게시한다 : 생성
    
    서버에 특정 데이터를 생성 ( 데이터를 중복적으로 생성) Like 중복된 글 계속 생성
    
- PUT : 데이터를 교체한다 : 수정
    
    데이터가 없으면 생성하고, 있으면 교체한다. ex) 중복된 글이 있으면 삭제하고 재생성
    
- PATCH : 데이터를 수정한다 : 수정
    
    기존 데이터를 진짜 수정한다.
    
- DELETE : 데이터를 삭제한다 : 삭제

### Path Parameter, Query String

http:// → 프로토콜 (scheme)

[www.example.com](http://www.example.com) → 서버 주소 (domain)

/user/{user_id}/nickname → 서버 내 데이터 위치 (path)

?page=1&keyword=hello → Query String

- HTTP Method, URL은 편지 봉투 겉면에 적는 받는 주소와
같은 개념이다. 따라서 그 내용에 대한 규약이 필요함

### http 데이터는 헤더와 바디로 나뉜다.

- 헤더 : 통신에 대한 정보
- 바디 : 주고 받으려는 실제 데이터 (주로 json)

![image.png](attachment:2a8e4c1c-635c-44f4-a486-13b0e74c22ce:image.png)

### 상태 코드

: 클라이언트에게 응답을 보낼 때, 요청에 대한 처리 결과를 나타내는 코드 (HTTP 헤더에 들어감)

### 대표적인 상태 코드

- 200 → 처리 성공 (ok)
- 201 → 데이터 생성 성공 (created)
- 400 → 클라이언트 요청 오류 (bad request)
- 404 → 요청 데이터 없음 (not found)
- 500 → 서버 에러 (internal server error)

맨 앞자리가 2면 성공, 4면 클라이언트 잘못때문에 문제가 생긴겨

### 프론트엔드와 백엔드

- 프론트는 화면에 채울 컨텐츠 데이터를 백엔드에게 요청
- 백엔드는 DB에서 가져온 컨텐츠 데이터를 프론트에게 응답

## API

: Application Programming Interface

: 어플리케이션에서 원하는 기능을 수행하기 위해 **어플리케이션과 소통하는 방법**을 정의한것

### REST API

: REST 아키텍처를 따르도록 설계한 API

- URL : 조작할 데이터 (명사)
- HTTP method : 데이터에 대한 행위 (동사)
- 세부적인 규칙은 [이곳](https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80)에서..
