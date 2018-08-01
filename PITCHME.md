## CH 03  
## The GraphQL Schema


taylorfang@twreporter.org
---

### Outline

- The schema object
- Type System and Type language
  - Types
    - Scalar and Object types
    - Enumeration types
    - Type modifiers
    - Interfaces and Unions
- Introspection
- // resolve function
- // validation
- // versioning

---

### The schema object

- A GraphQL schema is what we write to represent the capabilities of a GraphQL server.
- The schema defines the types and directives we want the server to support.
  - what data we can ask for
  - what fields can we select
  - What kinds of objects might they return
  - What fields are available on those sub-objects
  
---

### The schema object (cont.)

- The constructor of the `GraphQLSchema` class expects a configuration object

```js
  class GraphQLSchema {
    constructor(config: GraphQLSchemaConfig)
  }

  type GraphQLSchemaConfig = {
    // query: A read-only fetch of information
    query: GraphQLObjectType;
    // mutation: A write followed by a read fetch of information 
    mutation?: ?GraphQLObjectType; 
  }
```

---

### The schema object (cont.)

- The schema is a representation of the capabilities of a GraphQL server starting from the root fields

```js
  const queryType = new GraphQLObjectType({
    name: 'RootQuery',
    fields: {
      hello: {
        // ...
      },
      diceRoll: {
        // ...
      },
      usersCount: {
        // ...
      }
    }
  })
  const mySchema = new GraphQLSchema({
    query: queryType
  });
```
  
---

### Type System and Type language

- GraphQL schemas in a language-agnostic way: **_GraphQL schema language_**
  - GraphQL is a strongly-typed language
  - A GraphQL schema should have types for all objects that it uses

---

### Type System and Type language: Object types
  
- **_Object Types_** : The most basic components of a GraphQL schema represents a kind of object you can fetch from your service
```js
const SimpleType = new GraphQLObjectType({
  name: 'simpleName',
  fields: {
    person: PersonType,
    phoneNumber: { type: GraphQLString }
  }
})
```

---

### Type System and Type language: Scalar Types

- **_Scalar Types_** : a scalar value that cannot have fields of its own
  - GraphQLInt, GraphQLFloat, GraphQLString, GraphQLBoolean, GraphQLID

---

### Type System and Type language: Enums Types

- **_Enums Types_** : when a scalar value to represent for a field has a list of possible values in a set, and it can only be one of them

```js
const ContractType = new GraphQLEnumType({
  name: 'Contract',
  values: {
    FULLTIME: { value: 1 },
    PARTTIME: { value: 2 },
    SHIFTWORK: { value: 3 }
  }
})
```

---

### Type System and Type language: Type Modifiers

- **_Type Modifiers_**
  - GraphQLList: represents a list of those type it wraps
  - GraphQLNonNull: enforces the value it wraps is never null

```js
const DepartmentType = new GraphQLObjectType({
  name: 'department',
  fields: {
    name: { type: new GraphQLNonNull(GraphQLString) },
    contractTypes: new GraphQLList(ContractType)
  }
})
```
  
---

### Type System and Type language: Interfaces

- **_Interfaces_**: An abstract type that includes a certain set of fields a type must include to implement the interface

```js
const TypeA = new GraphQLObjectType({
  name: 'A',
  fields: {
    name: { type: new GraphQLString },
    otherTypes: { type: new GraphQLString }
  }
})
const TypeB = new GraphQLObjectType({
  name: 'B',
  fields: {
    name: { type: new GraphQLString },
    someOtherTypes: { type: new GraphQLString }
  }
})
```
  
---

### Type System and Type language: Interfaces (cont.)

```js
const CommonType = new GraphQLInterfaceType({
  name: 'Common',
  fields: {
    name: { type: GraphQLString }
  }
})
const TypeC = new GraphQLObjectType({
  name: 'C',
  fields: {
    name: { type: new GraphQLString },
    someTypes: { type: new GraphQLString }
  },
  interfaces: [ CommonType ]
})
```
  
---

### Type System and Type language: Unions

```js
const EducationType = new GraphQLObjectType({
  name: 'Education'
  fields: () => ({
    schoolName: { type: GraphQLString }
  })
})
const ExperienceType = new GraphQLObjectType({
  name: 'Experience'
  fields: () => ({
    companyName: { type: GraphQLString }
  })
})
const ResumeType = new GraphQLUnionType({
  name: 'Resume',
  types: [ExperienceType, EducationType],
  resolveType(value) {
    if (value instanceof Experience) {
      return ExperienceType
    }
    if (value instanceof Education) {
      return EducationType
    }
  }
})
```
  
---

### Type System and Type language: Unions

```graphql
query ResumeInformation {
  Resume {
    ... on Education {
      schoolName
    }
    ... on Experience {
      companiName
    }
  }
}
```
  
---

### Introspection

- Use **_introspective API_** to ask about the GraphQL schema, all of the types and what directives that schema supports.

```js
const queryType = new GraphQLObjectType({
  name: 'RootQuery',
  fields: {
    hello: {
      type: GraphQLString
    },
    helloTimes: {
      description: 'Just to **Say Hello**',
      type: new GraphQLList(GraphQLInt),
      args: {
        count: {
          type: GraphQLInt,
          defaultValue: 2
        }
      }
    }
  }
})
```

---

### Introspection (cont.)

```graphql
query TypeFields {
  _type(name: "RootQuery") {
    fields {
      name
      description
      args {
        name
      }
    }
  }
}
```

---

### Introspection (cont.)

```json
{
  "data": {
    "__type": {
      "fields": [
        {
          "name": "hello",
          "description": null,
          "args": []
        },
        {
          "name": "helloTimes",
          "description": "Just to **Say Hello**",
          "arg": [
            {
              "name": "count"
            }
          ] 
        }
      ]
    }
  }
}
```

---

### Introspection (cont.)

```graphql
query QueryTypeName {
  __schema {
    queryType {
      name
    }
  }
}
```

---

### Introspection (cont.)

```json
{
  "data": {
    "__schema": {
      "queryType": {
        "name": "RootQuery"
      }
    }
  }
}
```

---

### Introspection (cont.)

**___Schema_**, **___Type_**, **___TypeKind_**, **___Field_**, **___InputValue_**, **___EnumValue_**, **___Directive_** - These all are preceded with a double underscore, indicating that they are part of the introspection system.

---