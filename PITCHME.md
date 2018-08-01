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
  
---

### What problems does GraphQL solve (4/4)

- Clients dependency on servers
  - solution:
    - The runtime layer defines a generic graph-based schema
    - Client applications who speak GraphQL can query that schema within its capabilities
    - The runtime layer will do the communication with different data services
  
---

### What’s the core principles of Relay (1/13)

- Storage and caching
- Object identification
- The connection model
  
---

### What’s the core principles of Relay (2/13)

- Storage and caching
  - Single client-side data store called **_Relay Store_**
  - **_Queue Store_** manages the inflight changes to the data
    - optimistic update
    - rollbacks : remove the faulty object from the Queue Store
  - **_Cache layer_**, which can be any storage engine

---

### What’s the core principles of Relay (3/13)

- Object identification
  - unique identifiers
  - Diffing algorithm: Only ask for the difference of the object between what we need and what we have

---

### What’s the core principles of Relay (4/13)

- The Connection Model
  - GraphQL connection
    - Introduced by **"Relay Cursor Connections Specification.”**
    - cursor-based pagination model

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