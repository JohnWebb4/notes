# Google API Standards

## Table of Contents

- [Google API Standards](#google-api-standards)
  - [Table of Contents](#table-of-contents)
  - [1. Resource Oriented Design](#1-resource-oriented-design)
    - [REST API](#rest-api)
    - [Example Endpoints](#example-endpoints)
  - [2. Resource Names](#2-resource-names)
    - [Collections](#collections)

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
