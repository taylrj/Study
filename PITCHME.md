## CH 03  
## The GraphQL Schema


taylorfang@twreporter.org
---

### Outline

- The schema object
- Type System and Type language
  - Types
    - Scalar and Object types
    - Interfaces and Unions
    - Enumeration types
    - Type modifiers
- Introspective API
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
- Define multiple schemas by creating new instances of **_GraphQLSchema_** class

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

### Type System and Type language: Types

- The most basic components of a GraphQL schema are **_object types_**, which just represent a kind of object you can fetch from your service

---

### Type System and Type language: Types (cont.)

- 

---

### What’s the core principles of Relay (5/13)

- The Connection Model
  - A connection is a way to get all of the nodes that are connected to another node in a specific way
  - Example:
  
---

### What’s the core principles of Relay (6/13)

  ![Connection Explained](https://cdn-images-1.medium.com/max/800/1*G2Byvcku-CB0qz6Xmhp1RA.png)
  
---

### What’s the core principles of Relay (7/13)

  ```js
    {
      user(id: "ZW5jaG9kZSBIZWxsb1dvcmxk") {
        id
        name
        friendsConnection(first: 3) {
          edges {
            cursor
            node {
              id
              name
            }
          }
        }
      }
    }
  ```
  
---

### What’s the core principles of Relay (8/13)

  - pagination models
    - offset/limit model
    - after/first model
    - The connection model
  
---

### What’s the core principles of Relay (9/13)

  ![flaw Explained](https://cdn-images-1.medium.com/max/800/1*VGCj1SK3VCdWAleXK_rvkg.png)
  
---

### What’s the core principles of Relay (10/13)

  ![flaw Explained2](https://cdn-images-1.medium.com/max/800/1*xfqxO28vw5G1vVIDD8HihQ.png)
  
---

### What’s the core principles of Relay (11/13)

  ```js
  {
    movie(title: "Inception") {
      releaseDate
      actors(first: 2) {
        edges {
          cursor
          node {
            name
          }
        }
        pageInfo {
          hasNextPage
        }
      }
    }
  }
  ```
  
---

### What’s the core principles of Relay (12/13)

  ```js
  {
    "releaseDate": "2010-08-29T12:00:00Z",
    "actors": {
      "edges": [
        {
        "cursor": "YXJyYXljb25uZWN0aW9uOjA=",
        "node": {
          "name": "Leonardo DiCaprio"
        }
      }, 
      {
        "cursor": "YXJyYXljb25uZWN0aW9uOjE=",
        "node": {
          "name": "Ellen Page"
        }
      }]
    }
  }
  ```
  
---

### What’s the core principles of Relay (13/13)

  ```js
  {
    movie(title: "Inception") {
      releaseDate
      actors(first: 2 after: "YXJyYXljb25uZWN0aW9uOjE=") {
        edges {
          cursor
          node {
            name
          }
        }
        pageInfo {
          hasNextPage
        }
      }
    }
  }
  ```