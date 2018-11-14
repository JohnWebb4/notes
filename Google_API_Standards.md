# Google API Standards

## Table of Contents

- [Google API Standards](#google-api-standards)
  - [Table of Contents](#table-of-contents)
  - [1. Resource Oriented Design](#1-resource-oriented-design)
    - [REST API](#rest-api)
    - [Example Endpoints](#example-endpoints)
  - [2. Resource Names](#2-resource-names)
    - [Collections](#collections)
  - [3. Standard Methods](#3-standard-methods)
    - [LIST](#list)
    - [SEARCH](#search)
    - [GET](#get)
    - [CREATE](#create)
    - [UPDATE](#update)
    - [DELETE](#delete)

## 1. Resource Oriented Design

Goal: How should I structure my endpoint?

- RPC involves complex interface and methods
- REST composes commands in simple small commands that return finite pieces of data.
- Streams/Sockets should use RPC
- Resource Names: Collection of objects in individual addressable resources
  - /users/{id}/messages/
- Collection or item
  - Collection contains multiple elements of same **type**

### REST API

- Operations: GET, LIST, CREATE, DELETE
- Avoid, but may use custom operations

### Example Endpoints

- REST: /users/{Id}/messages/
- Pub/Sub: pubsub.googleapis.com
  - collection of topics: projects/_/topics/_
  - similar for subscriptions
- Database API: spanner.googleapis.com
  - instance projects/_/instances/_
  - operations: projects/_instances/_/operations/
  - similar for database, database operations, and database sessions

## 2. Resource Names

Goal: What should my endpoint be?

- Resource Oriented Architecture use resouce name / entity ID as endpoint
- gRPC (Remote Procedure Call) use scheme-less URI
  - Very similar to ROA but ends with command

### Collections

- ID should be valid C identified in lower camel case and plural word
- ID should be unique string, not resource ID
  - Large systems have multiple resource IDs leads to confusion
- Use display_name for resource ID field
  - Forces a proper name

## 3. Standard Methods

Goal: Understand common methods used by Google in requests

| Type   | Mapping         | Request Body | Response Body         |
| ------ | --------------- | ------------ | --------------------- |
| LIST   | Get collection  | N/A          | Resource*             |
| GET    | Get resouce     | N/A          | Resource*             |
| Create | POST collection | Resouce      | Resource*             |
| Update | PUT resouce     | Resouce      | Resouce*              |
| Delete | DELTE resouce   | N/A          | google.protobuf.Empty |

*Resouces can contain partial data (masked elements)

Any long running operation shoud return reference to operation.

### LIST

Request type: GET

- Search for array of elements with params
- No request body
- Array is bounded in size and not cached

See [collection naming](#2-resource-names). (Ex. /messages)

### SEARCH

Request type : GET

- Search for collection of elements with params
- No request body
- General version of LIST that is not bounded with size

See [collection naming](#2-resource-names). (Ex. /messages)

### GET

Request type: GET

- Get single element
- No request body
- Response body is object

See [item naming](#2-resource-names). (Ex. /message/{id?})

### CREATE

Request type: POST

- Should specify **parent** param for where the object should be created
- Params specify where to create object
  - Possible resource ID
- Request body is object to be created
- Response if created object
- Anything in response that cannot be specified by client should be documented as *output only*
- Should return ALREADY_EXISTS is already exists

See [collection naming](#2-resource-names). (Ex /messages/)

### UPDATE

Request type: PUT or PATCH

- Can change any value except name or parent
- Request body if object to be updated
- Return NOT_FOUND if missing
- Use PATCH if changing a field(s). Use PUT if full update (discouraged).
  - Full update discouraged since migrations suck

See [item naming](#2-resource-names). (Ex /message/{id?})

### DELETE

Request type: DELETE

- Never rely on response body (has many different response)
- No request body
- Returns empty if immediately deleting object
- Returns long operation if in process of being deleted
- Returns object with a delete flag if queued for deletion at some point

See [item naming](#2-resource-names). (Ex /message/{id?})

