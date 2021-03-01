# Query

```
const typeDefs = gql`
    type Query {
        teams: [Team] // Team형태의 정보를 여러개 가져온다.
        supplies: [Supply]
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
    type Supply {
      id: String,
      team: Int
    }
`

const resolvers = {
  Query: {
    teams: () => database.teams
    .map((team) => {
        team.supplies = database.supplies
        .filter((supply) => {
            return supply.team === team.id
        })
        return team
    }), // teams와 supplies를 한번에 가져온다.

    team: (parent, args, context, info) => database.teams
    .filter((team) => {
        return team.id === args.id
    })[0], // 입력한 id를 갖는 team만 불러온다.

    supplies: () => database.supplies
  }
}
```
