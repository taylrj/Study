# An Introduction to GraphQL and Relay

taylorfang@twreporter.org
---

## Outline

- What are they ?
  - What is GraphQL ?
  - What is Relay ?
- What problems do they solve ?
- What's the core principles of Relay ?

---

## What are they ? (/)

![GraphQL Explained](https://cdn-images-1.medium.com/max/1600/1*2mTYU2RCJHagQrqQokYpww.png)

---

## What are they ? (/)

- What is GraphQL ?
  - GraphQL provides a **_common_** interface between client and server applications for fetching and manipulating data

---

## What are they ? (/)

- What is GraphQL ?
  - GraphQL is a language
    - The GraphQL query language is basically about selecting fields on objects

---

### What are they ? (/)

- What is GraphQL ?
  - GraphQL is a runtime
    - To teach a data service to speak GraphQL, we need to implement a **_runtime layer _** and expose it to the clients who want to communicate with the service
    - Think of this layer on the server side as simply a translator of the GraphQL language

---

## What are they ? (/)

![GraphQL Explained](https://cdn-images-1.medium.com/max/1600/1*2mTYU2RCJHagQrqQokYpww.png)

---

### What are they ? (/)

- What is GraphQL ?
  - Schema
    - Defines a GraphQL API's type system. It describes the complete set of possible data (objects, fields, relationships, everything) that a client can access
    - A capabilities document that has a list of all the questions which the client can ask the GraphQL layer

---