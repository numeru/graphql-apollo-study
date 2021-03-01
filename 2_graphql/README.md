```
# 프로젝트 모듈 설치
npm install
#
# 프로젝트 실행 명령어 (해당 프로젝트 폴더에서)
nodemon index.js
# 브라우저에서 localhost:4000 으로 확인
```

---

# GraphQl

- 필요한 정보들만 선택하여 받아올 수 있다.
- 여러 계층의 정보들을 한번에 받아올 수 있다.
- 하나의 endpoint에서 모든 요청을 처리할 수 있다.  
  : 하나의 URI에서 POST로 모든 요청을 처리.

## 1. 정보 가져오기 : query

- 모든 정보

```
query {
  teams {
    id
    manager
    office
    extension_number
    mascot
    cleaning_duty
    project
  }
}
```

- 원하는 정보만

```
query {
  teams {
    manager
    office
  }
}

query {
  team(id: 1) {
    manager
    office
  }
}

query {
  team(id: 1) {
    manager
    office
    members {
      first_name
      last_name
    }
  }
}

query {
  teams {
    manager
    office
    mascot
  }
  roles {
    id
    requirement
  }
}
```

---

## 2. 정보 추가, 수정, 삭제하기 : mutation

```
// 붙여넣기
mutation {
  postTeam (input: {
    manager: "John Smith"
    office: "104B"
    extension_number: "#9982"
    mascot: "Dragon"
    cleaning_duty: "Monday"
    project: "Lordaeron"
  }) {
    manager
    office
    extension_number
    mascot
    cleaning_duty
    project
  }
}

// 수정하기
mutation {
  editTeam(id: 2, input: {
    manager: "Maruchi Han"
    office: "105A"
    extension_number: "2315"
    mascot: "Direwolf"
    cleaning_duty: "Wednesday"
    project: "Haemosu"
  }) {
    id,
    manager,
    office,
    extension_number,
    mascot,
    cleaning_duty,
    project
  }
}

// 삭제하기
mutation {
  deleteTeam(id: 3) {
    id,
    manager,
    office,
    extension_number,
    mascot,
    cleaning_duty,
    project
  }
}
```
