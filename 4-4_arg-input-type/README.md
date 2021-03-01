# arguments and input-type

## 1. 데이터 조건들로 필터 넣어 받아오기

```
const typeDefs = gql`
  type Query {
    people: [People]
    peopleFiltered(
      team: Int
      sex: Sex
      blood_type: BloodType
      from: String
    ): [People]
    ...
  }
`;
```

```
// 다음 조건을 만족하는 정보만 불러온다.
query {
  peopleFiltered (
    team: 1
    blood_type: B
    from: "Texas"
  ) {
    id
    first_name
    last_name
    sex
    blood_type
    serve_years
    role
    team
    from
  }
}
```

---

## 2. 페이지로 나누어 받아오기

```
const typeDefs = gql`
  type Query {
    peoplePaginated(page: Int!, per_page: Int!): [People]
    ...
  }
`;
```

```
// id가 1~7인 정보를 불러온다.
query {
  peoplePaginated(page: 1, per_page: 7) {
    id
    first_name
    last_name
    sex
    blood_type
    serve_years
    role
    team
    from
  }
}
```

---

## 3. 별칭으로 받아오기

```
// 조건을 임의의 별칭으로 정하여 정보를 불러온다.
query {
  badGuys: peopleFiltered(sex: male, blood_type: B) {
    first_name
    last_name
    sex
    blood_type
  }
  newYorkers: peopleFiltered(from: "New York") {
    first_name
    last_name
    from
  }
}
```

---

## 4. 인풋 타입

```
input PostPersonInput {
  first_name: String!
  last_name: String!
  sex: Sex!
  blood_type: BloodType!
  serve_years: Int!
  role: Role!
  team: ID!
  from: String!
}
```

```
// input형태의 정보를 추가한다.
mutation {
  postPerson(input: {
    first_name: "Hanna"
    last_name: "Kim"
    sex: female
    blood_type: O
    serve_years: 3
    role: developer
    team: 1
    from: "Pusan"
  }) {
    id
    first_name
    last_name
    sex
    blood_type
    role
    team
    from
  }
}
```
