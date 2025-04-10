<h1 style='background-color: rgba(55, 55, 55, 0.4); text-align: center'>API 설계(명세)서 </h1>

해당 API 명세서는 '실버케어테크 지적 치료 서비스 수업 - 우리동네 맛집'의 REST API를 명세하고 있습니다.  

- Domain : http://127.0.0.1:4000    

***
  
<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>Auth 모듈</h2>

우리동네 맛집 서비스의 인증 및 인가와 관련된 REST API 모듈입니다.  
로그인, 회원가입, 아이디, 닉네임, 이메일 중복 확인, 이메일 인증 등의 API가 포함되어 있습니다.  
Auth 모듈은 인증 없이 요청할 수 있는 모듈입니다.    
  
- url : /api/v1/auth  

***

#### - 로그인  
  
##### 설명

클라이언트는 사용자 아이디와 평문의 비밀번호를 포함하여 요청하고 아이디와 비밀번호가 일치한다면 인증에 사용될 token과 해당 token의 만료 기간을 응답 데이터로 전달받습니다. 만약 아이디 혹은 비밀번호가 하나라도 일치하지 않으면 로그인 불일치에 해당하는 응답을 받습니다. 서버 에러, 데이터베이스 에러, 유효성 검사 실패 에러가 발생할 수 있습니다.    

- method : **POST**  
- URL : **/sign-in**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자의 아이디 | O |
| userPassword | String | 사용자의 비밀번호 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/sign-in" \
 -d "userId=qwer1234" \
 -d "userPassword=Qwer1234!"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| accessToken | String | Bearer 인증 방식에 사용될 JWT | O |
| expiration | Integer | accessToken의 만료 기간 (초단위) | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success.",
  "accessToken": "${ACCESS_TOKEN}",
  "expiration": 32400
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (로그인 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "SF",
  "message": "Sign in Fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 아이디 중복 확인  
  
##### 설명

클라이언트는 사용할 아이디를 포함하여 요청하고 중복되지 않는 아이디라면 성공 응답을 받습니다. 만약 사용중인 아이디라면 아이디 중복에 해당하는 응답을 받습니다. 서버 에러, 데이터베이스 에러가 발생할 수 있습니다.  

- method : **POST**  
- URL : **/id-check**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 중복확인을 수행할 사용자 아이디 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/id-check" \
 -d "userId=qwer1234"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (중복된 아이디)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "EU",
  "message": "Exist User."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

#### - 닉네임 중복 확인  
  
##### 설명

클라이언트는 사용할 닉네임 포함하여 요청하고 중복되지 않는 닉네임이라면 성공 응답을 받습니다. 만약 사용중인 닉네임이라면 닉네임 중복에 해당하는 응답을 받습니다. 서버 에러, 데이터베이스 에러가 발생할 수 있습니다.  

- method : **POST**  
- URL : **/nickName-check**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userNickname | String | 중복확인을 수행할 사용자 닉네임 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/nickName-check" \
 -d "userNickname=맛집"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (중복된 닉네임)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "EU",
  "message": "Exist User."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

### - 이메일 인증코드 전송, 중복 확인 
  
클라이언트로부터 이메일을 입력받아 해당하는 이메일이 이미 사용중인 이메일인지 확인하고 사용하고 있지 않은 이메일이라면 4자리의 인증코드를 해당 이메일로 전송합니다. 이메일 전송이 성공적으로 종료되었으면 성공처리를 합니다. 만약 중복된 이메일이거나 이메일 전송에 실패했으면 실패처리를 합니다. 데이터베이스 오류가 발생할 수 있습니다.  

##### 설명

- method : **POST**  
- URL : **/email-auth**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail | String | 인증번호를 전송할 사용자 이메일 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/email-auth" \
 -d "userEmail=qwer1234@gmail.com"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (중복된 이메일)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "DE",
  "message": "Duplicatied Email."
}
```

**응답 : 실패 (이메일 전송 실패)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "MF",
  "message": "Mail send Failed."
}
```


**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***
### 이메일 인증 확인

- method : **POST**  
- URL : **/email-auth-check**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail | String | 인증 번호를 확인할 사용자 이메일 | O |
| authNumber | String | 인증 확인할 인증 번호 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/email-auth-check" \
 -d "userEmail=qwer1234@gmail.com" \
    "authNumber=1234"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (이메일 인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Authentication Failed."
}
```


**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 회원가입  
  
##### 설명

클라이언트는 사용자 이름, 닉네임, 성별, 사용자 아이디, 사용자 비밀번호, 사용자 이메일, 주소, 상세주소,  가입경로를 포함하여 요청하고 회원가입이 성공적으로 이루어지면 성공에 해당하는 응답을 받습니다. 만약 존재하는 아이디일 경우 중복된 아이디에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러가 발생할 수 있습니다.  

- method : **POST**  
- URL : **/sign-up**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자 아이디 (영문과 숫자로만 이루어진 6자 이상 20자 이하 문자열) | O |
| userNickname | String | 사용자 닉네임 | O |
| userPassword | String | 사용자 비밀번호 (영문 숫자 조합으로 이루어진 8자 이상 13자 이하 문자열) | O |
| userEmail| String | 사용자 이메일(특수문자는 '@', '.', '-', '_' 만 사용) | O |
| name | String | 사용자 이름 (한글로만 이루어진 2자 이상 5자 이하 문자열) | O |
| gender | String | 사용자 성별 (남, 여) | O |
| address | String | 사용자 주소 | O |
| detailAddress | String | 사용자 상세 주소 | X |
| joinType | String | 가입 경로 (NORMAL: 일반, KAKAO: 카카오, NAVER: 네이버) | O |
| snsId | String | sns 아이디 | X |
| profileImage | String | 사용자 프로필 | X |
| userLevel | String | 사용자 등급 | X |
| emailToken | String | Bearer 인증 방식에 사용될 JWT (email) | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/sign-up" \
 -d "userId=qwer1234" \
 -d "userPassword=qwer1234" \
 -d "userEmail=qwer1234@gmail.com" \
 -d "emailToken=qwer1234""\
 -d "userNickname=맛집" \
 -d "gender=남" \
 -d "name=홍길동" \
 -d "address=부산광역시 부산진구 ..." \
 -d "detailAddress=402호" \
 -d "userLevel=0레벨" \
 -d "joinType=NORMAL"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (중복된 아이디 또는 닉네임)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "EU",
  "message": "Exist User."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***
#### - SNS 회원가입 및 로그인  
  
##### 설명

클라이언트는 가입경로를 포함하여 요청하고 성공시 가입 이력이 있는 사용자라면 메인 페이지로 리다이렉트를 받고 만약 가입 이력이 없는 사용자일 경우 회원가입 페이지로 리다이렉트 됩니다. 서버 에러, 데이터베이스 에러가 발생할 수 있습니다.    

- method : **GET**  
- URL : **sns/{registrationName}**  

##### Request

###### Path Variable

| name | type | description | required |
|---|:---:|:---:|:---:|
| registrationName | String | SNS 종류 (KAKAO, NAVER) | O |

###### Example

```bash
curl -v GET http://127.0.0.1:4000/api/v1/auth/sns/kakao
```

##### Response

###### Example

**응답 성공 (회원가입이 된 상태 일 때)**
```bash
HTTP/1.1 302 Found
Set-Cookie: accessToken=${accessToken}; Path=/;
Location: http://127.0.0.1:4000/main
```

**응답 성공 (회원가입이 안된 상태 일 때)**
```bash
HTTP/1.1 302 Found
Set-Cookie: accessToken=${accessToken}; Path=/;
Location: http://127.0.0.1:4000/auth/${registration}
```

**응답 : 실패 (OAuth 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Auth Fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```
***
#### - 아이디 찾기
  
##### 설명

클라이언트는 사용자에게 사용자의 이름, 이메일 주소를 입력을 요청합니다.
사용자의 이름과 이메일 주소를 데이테베이스에 저장된 사용자 이름, 이메일 주소와 비교하고, 같은 사용자에 이름과 이메일주소가 존재한다면 이메일 주소로 인증번호를 보냅니다.
인증번호를 입력하여 인증번호가 맞다면 아이디를 알려줍니다.

- method : **POST**  
- URL : **id-search**  

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userEmail| String | 사용자 이메일(특수문자는 '@', '.', '-', '_' 만 사용) | O |
| name | String | 사용자 이름 (한글로만 이루어진 2자 이상 5자 이하 문자열) | O |
| authNumber | String | 인증번호 4글자(영어 대문자, 숫자 랜덤값) | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/id-search" \
 -d "name=홍길동" \
    "userEmail=qwer1234@gmail.com" \
    "authNumber=1234"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (이메일 인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Authentication Failed."
}
```


**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```
***

#### - 비밀번호 찾기
  
##### 설명

클라이언트는 사용자에게 사용자의 아이디, 이메일 주소를 입력을 요청합니다.
사용자의 이름과 이메일 주소를 데이테베이스에 저장된 사용자 아이디, 이메일 주소와 비교하고, 같은 사용자에 아이디와 이메일 주소가 존재한다면 이메일 주소로 인증번호를 보냅니다.
인증번호를 입력하여 인증번호가 맞다면 임시비밀번호를 사용자의 이메일로 전송합니다.

- method : **POST**  
- URL : **password-search**

##### Request

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userId | String | 사용자 아이디 (영문과 숫자로만 이루어진 6자 이상 20자 이하 문자열) | O |
| userEmail| String | 사용자 이메일(특수문자는 '@', '.', '-', '_' 만 사용) | O |
| authNumber | String | 인증번호 4글자(영어 대문자, 숫자 랜덤값) | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/auth/password-search" \
 -d "userId=qwer1234" \
    "userEmail=qwer1234@gmail.com" \
    "authNumber=1234"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (이메일 인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Authentication Failed."
}
```


**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```


***


<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>Board 모듈</h2>

#### - 게시글 작성

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 게시글의 지역, 카테고리 선택과 제목, 내용을 입력하여 요청하며, 게시글 작성이 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 인증 실패, 데이터 유효성 검사 실패, 데이터베이스 에러 등이 발생할 수 있습니다.

-  method : POST
-  URL : /api/v1/boards

##### Request

##### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | O |
| ContentType | application/json | O |

###### Request Body

| name | type | description | required |
|---|---|---|---|
| boardAddressCategory | String | 게시글 지역 카테고리 (선택 1번 값: 서울, 경기, && 선택 2번 값: 특정 시/군/구) | O |
| boardDetailCategory | Array[String] | 선택한 카테고리 (ex. 시설, 교통 ... , 최소 1개이상 선택 필수) | O |
| boardTitle | String | 게시글 제목 | O |
| boardContent | String | 게시글 내용 | O |
| boardAddress | String | 작성자가 검색 또는 직접 입력한 주소(위치 버튼에 입력하는 주소) | O |
| boardWriteDate | String | 게시글 작성 날짜 (YYYY-MM-DD 형식, 서버에서 자동 생성) | X |
| boardViewCount | Integer | 조회수 (기본값 0, 서버에서 자동 관리) | X |
| boardScore | Integer | 게시글 점수 (기본값 0, 서버에서 자동 관리) | X |
| boardImage | TEXT | 게시글 이미지 | X |

###### Example

```bash
curl -v -X POST http://127.0.0.1:4000/api/v1/boards \
 -H "Authorization=Bearer XXXX" \
 -H "Content-Type: application/json" \
 -d '{
    "boardAddressCategory": "부산 부산진구",
    "boardDetailCategory": "시설",
    "boardTitle": "주상 복합 완공",
    "boardContent": "<div>25년 5월 1일 서면 NC백화점 위치 주상 복합 완공.</div>",
    "boardAddress": "부산광역시 부산진구 동천로 92(전포동)"
}'
```

##### Response

###### Response Body

| name | type | description | required |
|---|---|---|---|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| data | Object | 생성된 게시글 정보 (성공 시) | X |
| data.boardNumber | Integer | 생성된 게시글의 고유 번호 (성공 시) | X |

###### Example

**응답 성공**
```bash
HTTP/1.1 201 Created
Content-Type: application/json

{
  "code": "SU",
  "message": "작성완료."
}
```

응답 : 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 : 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 : 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```

#### - 게시글 상세 보기 

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL 경로에 조회할 게시글의 고유 번호를 담아 요청합니다. 조회가 성공적으로 이루어지면 서버는 요청한 게시글의 상세 정보를 담은 응답을 반환합니다. 만약 존재하지 않는 게시글일 경우 해당 오류 응답을 받습니다. 서버 에러, 인증 실패, 데이터베이스 에러가 발생할 수 있습니다.

 - method : GET
 - URL : /api/v1/boards/{board_number}

##### Request

###### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | X |

###### Path Parameter

| name | type | description | required |
|---|---|---|---|
| boardNumber | Integer | 조회할 게시글의 고유 번호 | O |

###### Example

```bash
curl -v -X GET "http://127.0.0.1:4000/api/v1/boards/123" \
 -H "Authorization=Bearer XXXX"
```

##### Response

###### Response Body

| name | type | description | required |
|---|---|---|---|
| code | String | 결과 코드 | O |
| message | String | 결과 코드에 대한 설명 | O |
| boardNumber | Integer | 게시글 고유 번호 | O |
| boardAddressCategory | String | 게시글 지역 카테고리 | O |
| boardDetailCategory | Array[String] | 선택한 카테고리 | O |
| boardTitle | String | 게시글 제목 | O |
| boardContent | String | 게시글 내용 | O |
| boardAddress | String | 작성자가 입력한 주소 | X |
| boardWriteDate | String | 게시글 작성 날짜 (YYYY-MM-DD 형식) | O |
| boardViewCount | Integer | 조회수 | O |
| boardScore | Integer | 게시글 점수 | O |
| boardImage | TEXT | 게시글 이미지 | X |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json

{
  "code": "SU",
  "message": "Success.",
  "boardNumber": 123,
  "boardAddressCategory": "부산 부산진구",
  "boardDetailCategory": "시설"
  "boardTitle": "주상 복합 완공",
  "boardContent": "<div>25년 5월 1일 서면 NC백화점 위치 주상 복합 완공.</div>",
  "boardAddress": "부산광역시 부산진구 동천로 92(전포동)",
  "boardWriteDate": "2025-04-30",
  "boardViewCount": 10,
  "boardScore": 5,
  "boardImage": "image_url"
}
```

응답 실패 (존재하지 않는 게시글)
```bash
HTTP/1.1 400 Not Found
ContentType: application/json

{
  "code": "NF",
  "message": "존재하지 않는 게시글입니다."
}
```

응답 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
ContentType: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
ContentType: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
ContentType: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```


#### - 게시글 수정

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL 경로에 수정할 게시글의 고유 번호를 담아 요청합니다. 요청 본문에는 수정할 게시글의 제목, 내용, 카테고리, 주소 등의 정보를 JSON 형태로 담아 전송합니다. 게시글 수정이 성공적으로 완료되면 서버는 성공 응답을 반환합니다. 인증 실패, 데이터 유효성 검사 실패, 해당 게시글이 존재하지 않는 경우, 데이터베이스 에러 등이 발생할 수 있습니다.

 - method :  PATCH
 - URL : /api/v1/boards/{board_number}

##### Request

###### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | O |
| ContentType | application/json | O |

###### Path Parameter

| name | type | description | required |
|---|---|---|---|
| boardNumber | Integer | 수정할 게시글의 고유 번호 | O |

###### Request Body
| name | type | description | required |
|---|---|---|---|
| boardAddressCategory | String | 게시글 지역 카테고리 (선택 1번 값: 서울, 경기, && 선택 2번 값: 특정 시/군/구) | O |
| boardDetailCategory | Array[String] | 선택한 카테고리 (ex. 시설, 교통 ... , 최소 1개이상 선택) | O |
| boardTitle | String | 게시글 제목 | O |
| boardContent | String | 게시글 내용 | O |
| boardAddress | String | 작성자가 검색 또는 직접 입력한 주소(위치 버튼에 입력하는 주소) | X |
| boardImage | TEXT | 게시글 이미지 | X |

###### Example

```bash
curl -v -X PATCH "http://127.0.0.1:4000/api/v1/boards/123" \
 -H "Authorization=Bearer XXXX" \
 -H "ContentType: application/json" \
 -d '{
    "boardAddressCategory": "서울 강남구",
    "boardDetailCategory": "시설",
    "boardTitle": "변경된 주상 복합 건물 정보",
    "boardContent": "<div>25년 6월 15일 삼성역 인근 주상 복합 변경 사항 안내.</div>",
    "boardAddress": "서울특별시 강남구 테헤란로 427(삼성동)"
}'
```

##### Response

###### Response Body

| name | type | description | required |
|---|---|---|---|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
ContentType: application/json

{
  "code": "SU",
  "message": "수정 완료."
}
```

응답 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
ContentType: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 실패 (데이터 유효성 검사 실패)
```bash
HTTP/1.1 400 Bad Request
ContentType: application/json

{
  "code": "VF",
  "message": "데이터 유효성 검사에 실패했습니다."
}
```

응답 실패 (존재하지 않는 게시글)
```bash
HTTP/1.1 400 Not Found
ContentType: application/json

{
  "code": "NF",
  "message": "존재하지 않는 게시글입니다."
}
```

응답 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
ContentType: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
ContentType: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```


#### - 게시글 삭제


##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL 경로에 삭제할 게시글의 고유 번호를 담아 요청합니다. 게시글 삭제가 성공적으로 완료되면 서버는 성공 응답을 반환합니다. 인증 실패, 해당 게시글이 존재하지 않는 경우, 데이터베이스 오류 등 다양한 오류 상황이 발생할 수 있습니다.

 - method : DELETE
 - URL : /api/v1/boards/{board_number}

##### Request

###### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Path Parameter
| name | type | description | required |
|---|---|---|---|
| boardNumber | Integer | 삭제할 게시글의 고유 번호 | O |

###### Example
curl -v -X DELETE "http://127.0.0.1:4000/api/v1/boards/123" \
 -H "Authorization=Bearer XXXX"

##### Response

###### Response Body

| name | type | description | required |
|---|---|---|---|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
ContentType: application/json

{
  "code": "SU",
  "message": "삭제 완료."
}
```

응답 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
ContentType: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 실패 (존재하지 않는 게시글)
```bash
HTTP/1.1 400 Not Found
ContentType: application/json

{
  "code": "NF",
  "message": "존재하지 않는 게시글입니다."
}
```

응답 실패 (권한 없음)
```bash
HTTP/1.1 403 Forbidden
ContentType: application/json

{
  "code": "NP",
  "message": "권한이 없습니다."
}
```

응답 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
ContentType: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
ContentType: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```


#### - 댓글 작성

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL에 게시물 번호를, 본문에 댓글 내용을 입력하여 요청하고 댓글 작성이 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 인증 실패, 존재하지 않는 게시물, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **POST**
- URL : **/{boardNumber}/comment**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Path parameter

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시글 번호 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| commentContent | String | 댓글 내용 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/boards/{boardNumber}/comment" \
  -h "Authorization=Bearer XXXX" \
  -d "commentContent=반가워요. 좋은 정보네요!"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 201 CREATE

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (존재하지 않는 게시글)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Auth fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 댓글 리스트 보기

##### 설명

클라이언트는 URL에 게시물 번호를 요청하여 조회가 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 존재하지 않는 게시물, 데이터 유효성 검사 실패가 발생할 수 있습니다.

- method : **GET**
- URL : **/{boardNumber}/comment**

##### Request

###### Path parameter

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시글 번호 | O |

###### Example

```bash
curl -v -X GET "http://127.0.0.1:4000/api/v1/boards/{boardNumber}/comment"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| comments | Comment[] | 해당 게시물에 작성된 댓글 리스트 | O |

###### Comment

| name | type | description | required |
|---|:---:|:---:|:---:|
| commentNumber | Integer | 댓글 번호 | O |
| commentContent | String | 댓글 내용 | O |
| userId | String | 댓글 작성자 아이디 | O |
| userLevel | Integer | 댓글 작성자 회원 등급 | O |
| userNickname | String | 댓글 작성자 닉네임 | O |
| writeDate | String | 댓글 작성 날짜 | O |
| boardNumber | Integer | 해당 댓글을 작성한 게시글 번호 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success.",
  "comments": [
    {
      "commentNumber": 1,
      "commentContent": "반가워요. 좋은 정보네요!",
      "userId": "asdf123",
      "userLevel": 2,
      "userNickname": "이순신",
      "writeDate": "2025-04-07",
      "boardNumber": 1
    }
  ]
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (존재하지 않는 게시글)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 좋아요 누르기

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL에 게시물 번호를 입력하여 요청하고 작업이 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 인증 실패, 존재하지 않는 게시물이 발생할 수 있습니다.

- method : **PUT**
- URL : **/{boardNumber}/good**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Path parameter

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시글 번호 | O |

###### Example

```bash
curl -v -X PUT "http://127.0.0.1:4000/api/v1/boards/{boardNumber}/good" \
  -h "Authorization=Bearer XXXX"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (존재하지 않는 게시글)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Auth fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 좋아요 누른 사용자 리스트 보기

##### 설명

클라이언트는 URL에 게시물 번호를 입력하여 요청하고 조회가 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 존재하지 않는 게시물이 발생할 수 있습니다.

- method : **GET**
- URL : **/{boardNumber}/good**

##### Request

###### Path parameter

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시글 번호 | O |

###### Example

```bash
curl -v -X GET "http://127.0.0.1:4000/api/v1/boards/{boardNumber}/good"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| goods | String[] | 해당 게시글의 좋아요 사용자 리스트 | O |


###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success.",
  "goods": [
      "asdf123"
      , ...
  ]
}
```

**응답 : 실패 (존재하지 않는 게시글)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 싫어요 누르기

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL에 게시물 번호를 입력하여 요청하고 작업이 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 인증 실패, 존재하지 않는 게시물이 발생할 수 있습니다.

- method : **PUT**
- URL : **/{boardNumber}/hate**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Path parameter

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시글 번호 | O |

###### Example

```bash
curl -v -X PUT "http://127.0.0.1:4000/api/v1/boards/{boardNumber}/hate" \
  -h "Authorization=Bearer XXXX" \
  -d "userId=zxcv123"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (존재하지 않는 게시글)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Auth fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 싫어요 누른 사용자 리스트 보기

##### 설명

클라이언트는 URL에 게시물 번호를 입력하여 요청하고 조회가 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 존재하지 않는 게시물이 발생할 수 있습니다.

- method : **GET**
- URL : **/{boardNumber}/hate**

##### Request

###### Path parameter

| name | type | description | required |
|---|:---:|:---:|:---:|
| boardNumber | Integer | 게시글 번호 | O |

###### Example

```bash
curl -v -X GET "http://127.0.0.1:4000/api/v1/boards/{boardNumber}/hate"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| hates | String[] | 해당 게시글의 싫어요 사용자 리스트 | O |


###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success.",
  "hates": [
    "zxcv123"
    , ...
  ]
}
```

**응답 : 실패 (존재하지 않는 게시글)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "NB",
  "message": "No Exist Board."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```


***

<h2 style='background-color: rgba(55, 55, 55, 0.2); text-align: center'>MyPage 모듈</h2>

우리 동네 맛집 서비스의 회원정보와 관련된 REST API 모듈입니다.
비밀번호 재확인, 게시물 리스트 보기, 내 정보 보기, 내 정보 수정하기 등의 API가 포함되어 있습니다.
MyPage 모듈은 모두 인증 후 요청할 수 있는 모듈입니다.

- url : /api/v1/my-page

***

#### - 비밀번호 재확인

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 사용자의 비밀번호를 입력하여 요청하고 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 만약 비밀번호가 일치하지 않는다면 비밀번호 불일치에 대한 응답을 받습니다. 서버 에러, 데이터베이스 에러, 유효성 검사 실패 에러가 발생할 수 있습니다.

- method : **POST**
- URL : **/password-check**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userPassword | String | 사용자가 회원가입 및 로그인 시 지정한 비밀번호 | O |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:4000/api/v1/my-page/password-check" \
 -h "Authorization=Bearer XXXX" \
 -d "userPassword=qwer1234"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (비밀번호 불일치 에러)**
```bash
HTTP/1.1 400 Bad Reuquest

{
  "code": "PN",
  "message": "Password Not Macthed."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

***

#### - 내 정보 확인
  
##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하여 요청하고 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 네트워크 에러, 서버 에러, 데이터베이스 에러가 발생할 수 있습니다.     

- method : **GET**  
- URL : **/user-info**

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Example

```bash
curl -X GET "http://127.0.0.1:4000/api/v1/my-page/user-info" \
 -h "Authorization=Bearer XXXX"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| userId | String | 사용자 아이디 | O |
| userNickname | String | 사용자 닉네임 | O |
| name | String | 사용자 이름 | O |
| userPassword | String | 사용자 비밀번호 | O |
| address | String | 사용자 주소 | O |
| detailAddress | String | 사용자 주소 | O |
| gender | String | 사용자 성별 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success.",
  "userId": "qwer1234",
  "userNickname": "쾌도",
  "name": "홍길동",
  "userPassword": "qwer1234",
  "address": "부산광역시 부산진구 중앙대로 668",
  "detailAddress": "4층 코리아 IT 아카데미",
  "gender": "남"
}
```

**응답 : 실패 (인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Auth fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database error."
}
```

***

#### - 사용자 정보 수정  
  
##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하여 사용자 이름을 입력하여 요청하고 회원가입이 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 네트워크 에러, 서버 에러, 데이터베이스 에러가 발생할 수 있습니다.  

- method : **PATCH**  
- URL : **/**  

##### Request

###### Header

| name | description | required |
|---|:---:|:---:|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Request Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| userNickname | String | 사용자 닉네임 | O |
| userPassword | String | 사용자 비밀번호 | O |
| address | String | 사용자 주소 | O |
| detailAddress | String | 사용자 주소 | O |

###### Example

```bash
curl -v -X PATCH "http://127.0.0.1:4000/api/v1/my-page" \
 -h "Authorization=Bearer XXXX" \
 -d "userNickname=의적" \
 -d "userPassword=Qwer123$" \
 -d "address=부산광역시 중구" \
 -d "detailAddress=303호"
```

##### Response

###### Response Body

| name | type | description | required |
|---|:---:|:---:|:---:|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK

{
  "code": "SU",
  "message": "Success."
}
```

**응답 : 실패 (데이터 유효성 검사 실패)**
```bash
HTTP/1.1 400 Bad Request

{
  "code": "VF",
  "message": "Validation Fail."
}
```

**응답 : 실패 (인증 실패)**
```bash
HTTP/1.1 401 Unauthorized

{
  "code": "AF",
  "message": "Auth fail."
}
```

**응답 : 실패 (데이터베이스 에러)**
```bash
HTTP/1.1 500 Internal Server Error

{
  "code": "DBE",
  "message": "Database Error."
}
```

