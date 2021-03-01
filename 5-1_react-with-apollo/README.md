# React with Apollo

- 2_graphql과 5-1_react-with-apollo를 실행

```
// 2_graphql
nodemon index.js

// 5-1_react-with-apollo
yarn start
```

## 1. Apollo Client 설치

[Apollo Client](https://www.apollographql.com/docs/react/get-started/)

```
npm install @apollo/client graphql
```

## 2. 모듈 import

```
// App.js
import { ApolloProvider } from '@apollo/client';
import { ApolloClient, InMemoryCache } from '@apollo/client'
```

## 3. client

```
// App.js
const client = new ApolloClient({
  uri: 'http://localhost:4000',
  cache: new InMemoryCache()
});
```

## 4. Apollo Provider

```
// App.js
return (
  <div className="App">
    <ApolloProvider client={client}>
      ...
    </ApolloProvider>
  </div>
);
```

## 5. add query or mutation

```
import { useQuery, useMutation, gql } from "@apollo/client";
```

```
// roles.js
const GET_ROLES = gql`
  query GetRoles {
    roles {
      id
    }
  }
`;

// teams.js
const DELETE_TEAM = gql`
  mutation DeleteTeam($id: ID!) {
    deleteTeam(id: $id) {
      id
    }
  }
`;
```

## 6. 필요한 곳에서 query, mutation실행

### 1. query

- useQuery를 사용하여 loading, error, data, refetch를 불러온다.

| 코드    | 설명                                       |
| :------ | :----------------------------------------- |
| loading | GraphQL 서버에서 정보를 받아오는 동안 표시 |
| error   | 요청에 오류가 발생할 시 반환               |
| data    | GraphQL 요청대로 받아진 정보               |

```
const { loading, error, data, refetch } = useQuery(GET_ROLES);


function AsideItems() {
  const { loading, error, data } = useQuery(GET_ROLES);
  if (loading) return <p className="loading">Loading...</p>;
  if (error) return <p className="error">Error :(</p>;
  return (
    <ul>
      {data.roles.map(({ id }) => {
        return (
          <li
            key={id}
            className={"roleItem " + (contentId === "id" ? "on" : "")}
          >
            <span>{contentId === id ? "🔲" : "⬛"}</span>
            {roleIcons[id]} {id}
          </li>
        );
      })}
    </ul>
  );
}
```

- refetch를 실행하면 곧바로 최신 data를 받아올 수 있다.

```
let refetchTeams;

const { loading, error, data, refetch } = useQuery(GET_TEAMS);
refetchTeams = refetch;

refetchTeams();
```

### 2. mutation

- useMutation을 사용하여 작업을 수행한다.

1. 필요한 인자를 mutation 함수인 deleteTeam에 전달하며 실행한다.

```
// click할 때 실행
function execDeleteTeam() {
  if (window.confirm("이 항목을 삭제하시겠습니까?")) {
    deleteTeam({ variables: { id: contentId } });
  }
}
```

2. useMutation형식에 맞게 인자 전달.

```
const [deleteTeam] = useMutation(DELETE_TEAM, {
  onCompleted: deleteTeamCompleted,
});

```

3. 성공 후 onCompleted에 전달했던 함수가 실행된다.

```
function deleteTeamCompleted(data) {
  console.log(data.deleteTeam);
  alert(`${data.deleteTeam.id} 항목이 삭제되었습니다.`);
  refetchTeams();
  setContentId(0);
}
```
