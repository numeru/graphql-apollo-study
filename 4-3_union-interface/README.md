# Union and Interface

## Union

- 타입 여럿을 한 배열에 반환하고자 할 때 사용

```
const resolvers = {
  Query: {
    givens: (parent, args) => {
      return [...dbWorks.getEquipments(args), ...dbWorks.getSupplies(args)];
    },
  },
  Given: {
    __resolveType(given, context, info) {
      if (given.used_by) {
        return "Equipment";
      }
      if (given.team) {
        return "Supply";
      }
      return null;
    },
  },
};
```

```
// typename이 Equipment일 경우와 Supply일 경우를 나누어서 정보를 가져온다.
query {
  givens {
    __typename
    ... on Equipment {
      id
      used_by
      count
      new_or_used
    }
    ... on Supply {
      id
      team
    }
  }
}
```

---

## Interface

- 유사한 객체 타입을 만들기 위한 공통 필드 타입

```
const typeDefs = gql`
  interface Tool {
    id: ID!
    used_by: Role!
  }
`;
const resolvers = {
  Tool: {
    __resolveType(tool, context, info) {
      if (tool.developed_by) {
        return "Software";
      }
      if (tool.new_or_used) {
        return "Equipment";
      }
      return null;
    },
  },
};

type Equipment implements Tool {
    id: ID!
    used_by: Role!
    count: Int
    new_or_used: NewOrUsed!
}

type Software implements Tool {
    id: ID!
    used_by: Role!
    developed_by: String!
    description: String
}
```

```
query {
  equipments {
    id
    used_by
    count
    new_or_used
  }
  softwares {
    id
    used_by
    description
    developed_by
  }
}
```
