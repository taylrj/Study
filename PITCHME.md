## CH 02  
## GraphQL: Queries and Mutations


taylorfang@twreporter.org
---

### Outline

- Queries
  - Syntax
  - Directives
  - Fragments
- Mutations

---

### Queries - Syntax

Example: A query to read the list of comments on a blog article

```grapghql
  query ArticleComments($articleId: Int!) {
    article(articleId: $articleId){
      comments {
        commentId
        formattedBody
        timestamp
      }
    }
  }
```

---

### Queries - Syntax (Cont.)

- To execute the generic query, we supply a JSON object for the variables input like: 

```json
{
  "articleId" : 42
}
```

---

### Queries - Syntax (Cont.)

- This is a possible response:

```graphql
  {
    "article":{
      "comments": [
        {
          "commentId": 1,
          "formattedBody": "GraphQL is <strong>cool</strong>",
          "timestamp": "12/12/2015 - 12:12"
        },
        {
          ...
        }
      ]
    }
  }
```

---

### Queries - Directives

We can provide options to alter the GraphQL runtime execution using directives.

characteristics of directives:
- unique name
- list of arguments
- list of locations where the use of directive is accepted
  - operation
  - fragment
  - field

---

### Queries - Directives (cont.)

We can provide options to alter the GraphQL runtime execution using directives.
- `@include`, which directs the GraphQL executor to include a field or a fragment only when the `if` argument is true
- `@skip`

```graphql
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}
```

---

### Queries - Directives (cont.)

- Aliases

```graphql
query ArticleResponses {
  post: Articles(articleId: 47) {
    responseId: commentId
  }
}
```

---

### Queries - Fragments

- Using the spread operator

```graphql
query TwoArticles {
  firstArticle: article(articleId: 42) {
    ...CommentList
  }
  secondArticle: article(articleId: 43) {
    ...CommentList
  }
}

fragment CommentList on Article {
  comments {
    commentId
    formattedBody
    timestamp
  }
}
```
---

### Mutations


---

### What problems does GraphQL solve (1/4)

- Multiple endpoints
  - solution:
    - Shift this multi-request complexity to the server-side and have the GraphQL layer ( runtime layer ) deal with it
    - Fetch all the initial data required by a view with a single round-trip to the server

---

### What problems does GraphQL solve (2/4)

- Versioning
  - solution:
    - Add new fields without removing the old ones, because we have a graph and we can flexibly grow the graph by adding more nodes
    - Leave paths on the graph for old APIs and introduce new ones without labeling them as new versions
  
---

### What problems does GraphQL solve (3/4)

- Over-fetching
  - solution:
    - Having a client request language means that the clients will be in control
    - They can ask for exactly what they need and the server will reply with exactly what that they’re asking for
  
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