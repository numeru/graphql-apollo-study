# modularized

- typeDefs와 resolvers는 배열을 받을 수 있다.

## 구성

```
// _queries.js: type Query
const typeDefs = gql`
  type Query {
    equipments: [Equipment]
    supplies: [Supply]
  }
`;

// equipment.js: type과 Query함수, 자세한 함수는 dbWorks에 정의.
const typeDefs = gql`
  type Equipment {
    id: String
    used_by: String
    count: Int
    new_or_used: String
  }
`;
const resolvers = {
  Query: {
    equipments: (parent, args) => dbWorks.getEquipments(args),
  },
};
```
