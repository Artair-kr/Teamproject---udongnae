#### - 게시글 작성

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 게시글의 지역, 카테고리 선택과 제목, 내용을 입력하여 요청하며, 게시글 작성이 성공적으로 이루어지면 성공에 대한 응답을 받습니다. 서버 에러, 인증 실패, 데이터 유효성 검사 실패, 데이터베이스 에러 등이 발생할 수 있습니다.

-  method : POST
-  URL : /api/boards

##### Request

##### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | O |
| Content-Type | application/json | O |

###### Request Body

| name | type | description | required |
|---|---|---|---|
| board_address_category | String | 게시글 지역 카테고리 (선택 1번 값: 서울, 경기, && 선택 2번 값: 특정 시/군/구) | O |
| board_detail_category | Array[String] | 선택한 카테고리 (ex. 시설, 교통 ... , 최소 1개이상 선택 필수) | O |
| board_title | String | 게시글 제목 | O |
| board_content | String | 게시글 내용 | O |
| board_address | String | 작성자가 검색 또는 직접 입력한 주소(위치 버튼에 입력하는 주소) | O |
| board_writeDate | String | 게시글 작성 날짜 (YYYY-MM-DD 형식, 서버에서 자동 생성) | O |
| board_viewCount | Integer | 조회수 (기본값 0, 서버에서 자동 관리) | O |
| board_score | Integer | 게시글 점수 (기본값 0, 서버에서 자동 관리) | O |
| board_image | TEXT | 게시글 이미지 | X |

###### Example

```bash
curl -v -X POST "http://127.0.0.1:5000/api/boards" \
 -H "Authorization=Bearer XXXX" \
 -H "Content-Type: application/json" \
 -d '{
    "board_address_category": "부산 부산진구",
    "board_detail_category": "시설",
    "board_title": "주상 복합 완공",
    "board_content": "<div>25년 5월 1일 서면 NC백화점 위치 주상 복합 완공.</div>",
    "board_address": "부산광역시 부산진구 동천로 92(전포동)"
}'
```

##### Response

###### Response Body

| name | type | description | required |
|---|---|---|---|
| code | String | 응답 결과 코드 | O |
| message | String | 응답 결과 코드에 대한 설명 | O |
| data | Object | 생성된 게시글 정보 (성공 시) | X |
| data.board_number | Integer | 생성된 게시글의 고유 번호 (성공 시) | X |

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
 - URL : /api/boards/{board_number}

##### Request

###### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | X |

###### Path Parameter

| name | type | description | required |
|---|---|---|---|
| board_number | Integer | 조회할 게시글의 고유 번호 | O |

###### Example

```bash
curl -v -X GET "http://127.0.0.1:5000/api/boards/123" \
 -H "Authorization=Bearer XXXX"
```

##### Response

###### Response Body

| name | type | description | required |
|---|---|---|---|
| code | String | 결과 코드 | O |
| message | String | 결과 코드에 대한 설명 | O |
| board_number | Integer | 게시글 고유 번호 | O |
| board_address_category | String | 게시글 지역 카테고리 | O |
| board_detail_category | Array[String] | 선택한 카테고리 | O |
| board_title | String | 게시글 제목 | O |
| board_content | String | 게시글 내용 | O |
| board_address | String | 작성자가 입력한 주소 | X |
| board_writeDate | String | 게시글 작성 날짜 (YYYY-MM-DD 형식) | O |
| board_viewCount | Integer | 조회수 | O |
| board_score | Integer | 게시글 점수 | O |
| board_image | TEXT | 게시글 이미지 | X |

###### Example

**응답 성공**
```bash
HTTP/1.1 200 OK
Content-Type: application/json

{
  "code": "SU",
  "message": "Success.",
  "board_number": 123,
  "board_address_category": "부산 부산진구",
  "board_detail_category": "시설"
  "board_title": "주상 복합 완공",
  "board_content": "<div>25년 5월 1일 서면 NC백화점 위치 주상 복합 완공.</div>",
  "board_address": "부산광역시 부산진구 동천로 92(전포동)",
  "board_writeDate": "2025-04-30",
  "board_viewCount": 10,
  "board_score": 5,
  "board_image": "image_url"
}
```

응답 실패 (존재하지 않는 게시글)
```bash
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "code": "NF",
  "message": "존재하지 않는 게시글입니다."
}
```

응답 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```







#### - 게시글 수정

##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL 경로에 수정할 게시글의 고유 번호를 담아 요청합니다. 요청 본문에는 수정할 게시글의 제목, 내용, 카테고리, 주소 등의 정보를 JSON 형태로 담아 전송합니다. 게시글 수정이 성공적으로 완료되면 서버는 성공 응답을 반환합니다. 인증 실패, 데이터 유효성 검사 실패, 해당 게시글이 존재하지 않는 경우, 데이터베이스 에러 등이 발생할 수 있습니다.

 - method :  PATCH
 - URL : /api/boards/{board_number}

##### Request

###### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | O |
| Content-Type | application/json | O |

###### Path Parameter

| name | type | description | required |
|---|---|---|
| board_number | Integer | 수정할 게시글의 고유 번호 | O |

###### Request Body
| name | type | description | required |
|---|---|---|---|
| board_address_category | String | 게시글 지역 카테고리 (선택 1번 값: 서울, 경기, && 선택 2번 값: 특정 시/군/구) | O |
| board_detail_category | Array[String] | 선택한 카테고리 (ex. 시설, 교통 ... , 최소 1개이상 선택) | O |
| board_title | String | 게시글 제목 | O |
| board_content | String | 게시글 내용 | O |
| board_address | String | 작성자가 검색 또는 직접 입력한 주소(위치 버튼에 입력하는 주소) | X |
| board_image | TEXT | 게시글 이미지 | X |

###### Example

```bash
curl -v -X PATCH "http://127.0.0.1:5000/api/boards/123" \
 -H "Authorization=Bearer XXXX" \
 -H "Content-Type: application/json" \
 -d '{
    "board_address_category": "서울 강남구",
    "board_detail_category": "시설",
    "board_title": "변경된 주상 복합 건물 정보",
    "board_content": "<div>25년 6월 15일 삼성역 인근 주상 복합 변경 사항 안내.</div>",
    "board_address": "서울특별시 강남구 테헤란로 427(삼성동)"
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
Content-Type: application/json

{
  "code": "SU",
  "message": "수정 완료."
}
```

응답 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 실패 (데이터 유효성 검사 실패)
```bash
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "code": "VF",
  "message": "데이터 유효성 검사에 실패했습니다."
}
```

응답 실패 (존재하지 않는 게시글)
```bash
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "code": "NF",
  "message": "존재하지 않는 게시글입니다."
}
```

응답 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```







#### - 게시글 삭제


##### 설명

클라이언트는 요청 헤더에 Bearer 인증 토큰을 포함하고 URL 경로에 삭제할 게시글의 고유 번호를 담아 요청합니다. 게시글 삭제가 성공적으로 완료되면 서버는 성공 응답을 반환합니다. 인증 실패, 해당 게시글이 존재하지 않는 경우, 데이터베이스 오류 등 다양한 오류 상황이 발생할 수 있습니다.

 - method : DELETE
 - URL : /api/boards/{board_number}

##### Request

###### Header

| name | description | required |
|---|---|---|
| Authorization | Bearer 토큰 인증 헤더 | O |

###### Path Parameter
| name | type | description | required |
|---|---|---|---|
| board_number | Integer | 삭제할 게시글의 고유 번호 | O |

###### Example
curl -v -X DELETE "http://127.0.0.1:5000/api/boards/123" \
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
Content-Type: application/json

{
  "code": "SU",
  "message": "삭제 완료."
}
```

응답 실패 (인증 실패 - 토큰 만료 또는 유효하지 않음)
```bash
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "code": "AF",
  "message": "인증에 실패했습니다."
}
```

응답 실패 (존재하지 않는 게시글)
```bash
HTTP/1.1 404 Not Found
Content-Type: application/json

{
  "code": "NF",
  "message": "존재하지 않는 게시글입니다."
}
```

응답 실패 (권한 없음)
```bash
HTTP/1.1 403 Forbidden
Content-Type: application/json

{
  "code": "NP",
  "message": "권한이 없습니다."
}
```

응답 실패 (서버 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "SE",
  "message": "서버 내부 오류가 발생했습니다."
}
```

응답 실패 (데이터베이스 에러)
```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "code": "DBE",
  "message": "데이터베이스 오류가 발생했습니다."
}
```
