```
# nodemon 설치
npm install -g nodemon
#
# 프로젝트 모듈 설치
npm install
#
# 프로젝트 실행 명령어 (해당 프로젝트 폴더에서)
nodemon index.js
# 브라우저에서 localhost:3000 으로 확인
```

---

# REST API

> 데이터를 주고받을 주체들간 약속된 형식.

| 요청 형식 | 용도          |
| :-------- | :------------ |
| GET       | 정보 받아오기 |
| POST      | 정보 입력하기 |
| PUT/PATCH | 정보 수정하기 |
| DELETE    | 정보 삭제하기 |

---

# REST API의 한계

## 1. Overfetching

- 필요한 정보를 얻기 위해 모든 정보를 불러와 한다.

## 2. Underfetching

- 원하는 형태의 정보를 얻기 위해 여러번의 호출이 요구될 수 있다.
