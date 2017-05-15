## Resolving Circular References

```
const CompanyType = new GraphQLObjectType({
  name: 'Company',
  fields: {
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString },
    users: {
      type: new GraphQLList(UserType),
      resolve(parentValue, args) {
        return axios.get(`http://localhost:3000/companies/${parentValue.id}`)
          .then(res => res.data)
      }
    }
  }
})

const UserType = new GraphQLObjectType({
  name: 'User',
  fields: {
    id: { type: GraphQLString },
    firstName: { type: GraphQLString },
    age: { type: GraphQLInt },
    company: {
      type: CompanyType,
      resolve(parentValue, args) {
        return axios.get(`http://localhost:3000/companies/${parentValue.companyId}`)
          .then(res => res.data)
      }
    }
  }
})
```

```
ReferenceError: UserType is not defined
```

**Using arrow function for solution.**

```
const CompanyType = new GraphQLObjectType({
  name: 'Company',
  fields: () => ({
    id: { type: GraphQLString },
    name: { type: GraphQLString },
    description: { type: GraphQLString },
    users: {
      type: new GraphQLList(UserType),
      resolve(parentValue, args) {
        return axios.get(`http://localhost:3000/companies/${parentValue.id}`)
          .then(res => res.data)
      }
    }
  })
})
```

## Query Fragments

```
{
  apple: company(id: "1"){
    ...companyDetails
  }
  google: company(id: "2"){
    ...companyDetails
  }
}

fragment companyDetails on Company {
  id
  name
  description
}
```
