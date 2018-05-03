# 01  
# An Introduction to GraphQL and Relay

taylorfang@twreporter.org
---

## Outline

- What are they ?
  - What is GraphQL ?
  - What is Relay ?
- What problems does GraphQL solve ?
- What's the core principles of Relay ?

---

## What are they ? (1/8)

![GraphQL Explained](https://cdn-images-1.medium.com/max/1600/1*2mTYU2RCJHagQrqQokYpww.png)

---

## What are they ? (2/8)

- What is GraphQL ?
  - GraphQL provides a **_common_** interface between client and server applications for fetching and manipulating data

---

## What are they ? (3/8)

- What is GraphQL ?
  - GraphQL is a language
    - The GraphQL query language is basically about selecting fields on objects

---

## What are they ? (4/8)

- What is GraphQL ?
  - GraphQL is a runtime
    - To teach a data service to speak GraphQL, we need to implement a **_runtime layer _** and expose it to the clients who want to communicate with the service
    - Think of this layer on the server side as simply a translator of the GraphQL language

---

## What are they ? (5/8)

![GraphQL Explained](https://cdn-images-1.medium.com/max/1600/1*2mTYU2RCJHagQrqQokYpww.png)

---

## What are they ? (6/8)

- What is GraphQL ?
  - Schema
    - Defines a GraphQL API's type system. It describes the complete set of possible data (objects, fields, relationships, everything) that a client can access
    - A capabilities document that has a list of all the questions which the client can ask the GraphQL layer

---

## What are they ? (7/8)

- What is Relay ?
  - Relay is a Javascript framework for building data-driven applications that uses GraphQL to enable React application to communicate their data requirements in a **_declarative way_**

---

## What are they ? (8/8)

- What is Relay ?
  - Relay provides a way for React applications to declaratively specify data requirements instead of imperatively dictating instructions for how to fetch that data
  - Declarative data requirements allow Relay to efficiently batch network requests

---

## What problems does GraphQL solve ? (8/8)

- What is Relay ?
  - Relay provides a way for React applications to declaratively specify data requirements instead of imperatively dictating instructions for how to fetch that data
  - Declarative data requirements allow Relay to efficiently batch network requests

---