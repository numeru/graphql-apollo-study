# GraphQL server setup

## 설치

```
npm init

"start": "nodemon index.js"

npm i graphql

npm i apollo-server
```

---

## 구성

```
// index.js
const database = require("./database");
const { ApolloServer, gql } = require("apollo-server");
const typeDefs = gql`
  type Query {
    teams: [Team]
  }
  type Team {
    id: Int
    manager: String
    office: String
    extension_number: String
    mascot: String
    cleaning_duty: String
    project: String
  }
`;
const resolvers = {
  Query: {
    teams: () => database.teams,
  },
};
const server = new ApolloServer({ typeDefs, resolvers });
server.listen().then(({ url }) => {
  console.log(`🚀  Server ready at ${url}`);
});

```

- ApolloServer : typeDef와 resolver를 인자로 받아서 서버생성.

- typeDef : GraphQL에서 사용될 데이터, 요청의 타입 지정.

- resolver : 서비스의 액션들을 함수로 지정.
