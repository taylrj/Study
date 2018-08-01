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

- We can provide options to alter the GraphQL runtime execution using directives.

-characteristics of directives:
  - unique name
  - list of arguments
  - list of locations where the use of directive is accepted
    - operation
    - fragment
    - field

---

### Queries - Directives (cont.)

- We can provide options to alter the GraphQL runtime execution using directives.
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

- Using fragments to remove duplication in queries

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

- Most clients will communicate their need to update the data through field arguments

```graphql
mutation AddNewComment {
  addComment(
    articleId: 42,
    authorEmail: "mark@gmail.com",
    mardkown: "Hello world"
  ){
    id,
    formattedBody
    timestamp
  }
}
```