# CH 01  
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

## What problems does GraphQL solve ? (1/4)

- Multiple endpoints
  - solution:
    - Shift this multi-request complexity to the server-side and have the GraphQL layer ( runtime layer ) deal with it
    - Fetch all the initial data required by a view with a single round-trip to the server

---

## What problems does GraphQL solve ? (2/4)

- Versioning
  - solution:
    - Add new fields without removing the old ones, because we have a graph and we can flexibly grow the graph by adding more nodes
    - Leave paths on the graph for old APIs and introduce new ones without labeling them as new versions
  
---

## What problems does GraphQL solve ? (3/4)

- Over-fetching
  - solution:
    - Having a client request language means that the clients will be in control
    - They can ask for exactly what they need and the server will reply with exactly what that they’re asking for
  
---

## What problems does GraphQL solve ? (4/4)

- Clients dependency on servers
  - solution:
    - The runtime layer defines a generic graph-based schema
    - Client applications who speak GraphQL can query that schema within its capabilities
    - The runtime layer will do the communication with the two different data services
    - This is how GraphQL first isolates the clients from needing to communicate in multiple languages
  
---

## What’s the core principles of Relay ? (/)

- Storage and caching
- Object identification
- The connection model
  
---

## What’s the core principles of Relay ? (/)

- Storage and caching
  - Relay uses a single client-side data store called **_Relay Store_**
  - In front of Relay Store : **_Queue Store_** manages the inflight changes to the data
    - optimistic update
    - rollbacks : remove the faulty object from the Queue Store
  - Behind of Relay Store : **_Cache layer_**, which can be any storage engine (e.g. localStorage)

---