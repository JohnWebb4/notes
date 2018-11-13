# RESTful Web Services

## Table of Contents

- [RESTful Web Services](#restful-web-services)
  - [Table of Contents](#table-of-contents)
  - [I. In a nutshell: Make it easy to use](#i-in-a-nutshell-make-it-easy-to-use)
    - [What is REST](#what-is-rest)
    - [REST vs Big Web Services](#rest-vs-big-web-services)
  - [1. General Web](#1-general-web)
    - [HTTP Protocal Sender](#http-protocal-sender)
    - [HTTP Protocol Response](#http-protocol-response)
    - [REST vs SOAP](#rest-vs-soap)
      - [SOAP: Simple Object Access Protocol](#soap-simple-object-access-protocol)
      - [REST: Reprentational State Transfer](#rest-reprentational-state-transfer)
    - [RPC: Remote Procedure Call](#rpc-remote-procedure-call)
  - [2. Intro Web Service Clients](#2-intro-web-service-clients)
    - [Considerations for RESTful Services](#considerations-for-restful-services)
    - [Basic Guidelines](#basic-guidelines)
    - [Examples](#examples)
    - [JSON vs XML](#json-vs-xml)
  - [3. REST Web Service Clients](#3-rest-web-service-clients)
    - [Storage services](#storage-services)
    - [Response Code](#response-code)
    - [Authentication](#authentication)
    - [Signed URIs](#signed-uris)
    - [Access Control](#access-control)
  - [4. REST Architecture](#4-rest-architecture)
  - [5. Write REST Endpoints](#5-write-rest-endpoints)
  - [6. REST Services](#6-rest-services)
  - [7. REST Service Example](#7-rest-service-example)
  - [8. REST and ROA Best Practices](#8-rest-and-roa-best-practices)
  - [9. The Building Blocks of Services](#9-the-building-blocks-of-services)
  - [10. Comparing REST with Big Web Services](#10-comparing-rest-with-big-web-services)
  - [11. How AJAX works into REST](#11-how-ajax-works-into-rest)
  - [12. REST Framworks](#12-rest-framworks)

## I. In a nutshell: Make it easy to use

Goal: What is this about?

### What is REST

- Representational State Transfer
- Resource Oriented Architecture
  - Query string parameters
  - You are performing an action on some resource
- Simpler than Remote Procedure Calls
  - RPC: You send a command for server to run a snippet of code

### REST vs Big Web Services

- REST / HTTP handle simple jobs very well
- Big Web Services handle distributed complicated problems very well
- Don't over complicate: Use HTTP if you can

## 1. General Web

Goal: General Web Services

Programmable web: It runs code
Web is mainly HTML, XML, JavaScript

### HTTP Protocal Sender

- Method: What you want to do (GET, POST, etc.)
- Path: Relative to website what endpoint are you hitting
- Header: Extra metadata you send
- Body: Any information you are sending

### HTTP Protocol Response

- Code: Server response code (404, etc.)
- Header: Extra metadata you send
- Body: Any information you are sending

### REST vs SOAP

#### SOAP: Simple Object Access Protocol

- Async communication protocal
- Less plumbing (tunnelling, security, etc.)
- Complex operations work well
- Can communicate through HTTP, SMTP, etc.
- XMl Formatting in body

#### REST: Reprentational State Transfer

- You are querying/modifying state on server
- Handles simple very well
- Endpoint describes object: PUT www.url.com/name

### RPC: Remote Procedure Call

- Format for encoding data
- Often inside HTTP envelope
- Often XML-RPC
- You are running a command on the server
- Endpoint describes action: www.url.com/name/rename

## 2. Intro Web Service Clients

Goal: Introduction to RESTful client?

- Implementing a hybrid RPC is very straightforward
  - Use HTTP libraries, XML libraries, and create endpoints

### Considerations for RESTful Services

- Supporting proxies (Most libraries by default)
- Caching common request responses
- Handling re-directs well
- Handle headers and authorization

### Basic Guidelines

- Always enfore the param types and number of them
- User curl to test endpoints

### Examples

- Java, PHP, C# all use SAX style XML parser (Simple API for XML)
- JavaScript XMLHTTPOnRequest handles it for your

### JSON vs XML

- JSON represents objects well
- XML / HTML represents documents well

## 3. REST Web Service Clients

Goal: How do you write a RESTful client?

### Storage services

- Goolgle's Atom Publishing Protocal (schema for storing data)
- Amazon's Simple Storage Service (store private data / make public)
  - Supports HTTP, BitTorrent, etc.
- Most systems allow storing schema-less objects in groups (buckets)
- Most of the time these groups (buckets) cannot be nested

### Response Code

- 200 OK
- 3XX Error / Redirect
- 4xx Bad request
- 5xx Server error

### Authentication

- Auth header for HTTP authentication
- If you want a user to only have access to certain items, use key-pair

### Signed URIs

- New unauthenticated URI that performs authenticated action on your behalf (forgot password)
- Works for limited time

### Access Control

Use filesystem access control (read, write, execute) to allow requests to resource

// TODO: Left off page 74

## 4. REST Architecture

Goal: Practical REST architecture

## 5. Write REST Endpoints

Goal: How do you write REST endpoints?

## 6. REST Services

Goal: Extending REST endpoints.

## 7. REST Service Example

Goal: Converting to turly RESTful server

## 8. REST and ROA Best Practices

Goal: Recap / When to use REST

## 9. The Building Blocks of Services

Goal: Extending REST

## 10. Comparing REST with Big Web Services

Goal: Compare / Contrast REST with alternatives

## 11. How AJAX works into REST

Goal: How Ajax aligns with REST

## 12. REST Framworks

Goal: Implementing REST with Frameworks
