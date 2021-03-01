# Mutation

## 1. 삭제

```
// index.js
const typeDefs = gql`
  type Query {
    teams: [Team]
    team(id: Int): Team
    equipments: [Equipment]
    supplies: [Supply]
  }
  type Mutation {
    deleteEquipment(id: String): Equipment
  }
  ...
`

const resolvers = {
  Query: {
    ...
  },
  Mutation: {
    deleteEquipment: (parent, args, context, info) => {
      const deleted = database.equipments.filter((equipment) => {
        return equipment.id === args.id;
      })[0];
      database.equipments = database.equipments.filter((equipment) => {
        return equipment.id !== args.id;
      });
      return deleted;
    },
  },
};
```

---

```
// id가 notebook인 equipment 삭제
mutation {
  deleteEquipment(id: "notebook") {
    id
    used_by
    count
    new_or_used
  }
}
```

---

## 2. 추가

```
//index.js
type Mutation {
  ...
  insertEquipment(
      id: String,
      used_by: String,
      count: Int,
      new_or_used: String
  ): Equipment
}

const resolvers = {
  ...
  Mutation: {
    ...
    insertEquipment: (parent, args, context, info) => {
      database.equipments.push(args)
      return args
    },
  },
};
```

---

```
// 다음 정보를 가진 equipment 추가
mutation {
  insertEquipment (
    id: "laptop",
    used_by: "developer",
    count: 17,
    new_or_used: "new"
  ) {
    id
    used_by
    count
    new_or_used
  }
}
```

---

## 3. 수정

```
// index.js
type Mutation {
  ...
  editEquipment(
      id: String,
      used_by: String,
      count: Int,
      new_or_used: String
  ): Equipment
}

const resolvers = {
  ...
  Mutation: {
    ...
    editEquipment: (parent, args, context, info) => {
      return database.equipments.filter((equipment) => {
          return equipment.id === args.id
      }).map((equipment) => {
          Object.assign(equipment, args)
          return equipment
      })[0]
    },
  },
};
```

---

```
// id가 pen tablet인 equipment의 정보를 수정
mutation {
  editEquipment (
    id: "pen tablet",
    new_or_used: "new",
    count: 30,
    used_by: "designer"
  ) {
    id
    new_or_used
    count
    used_by
  }
}
```
