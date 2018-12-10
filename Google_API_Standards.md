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
  - [4. Custom Methods](#4-custom-methods)
    - [When to use](#when-to-use)
    - [Common Custom Methods](#common-custom-methods)
  - [5. Standard Fields](#5-standard-fields)
  - [6. Errors](#6-errors)
    - [Should help user understand and resolve issue](#should-help-user-understand-and-resolve-issue)
  - [7. Naming Conventions](#7-naming-conventions)
    - [General Naming](#general-naming)
    - [Product Naming](#product-naming)
    - [Service Naming](#service-naming)
    - [Package Naming](#package-naming)
    - [Various Code Namings](#various-code-namings)

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

## 4. Custom Methods

[Standard methods](#3-standard-methods) if creating, updating, or deleting an object

Follow generic mapping template:
```
https://service.name/v1/some/resource/name:customVerb
```

- Use POST for any method with side effects (prefer POST, most versatile)
- Use GET if no side effects

### When to use

- A very complicated long running action (reboot a virtual machine)
- When you are performing an action on an already created object (send a drafted email)
- Custom batch methods

### Common Custom Methods

| Name     | Custom Verb | HTTP Verb | Note                            |
| -------- | ----------- | --------- | ------------------------------- |
| Cancel   | `:cancel`   | POST      | Cancel operation                |
| BatchGet | `:batchGet` | GET       | Get multiple resources          |
| Move     | `:move`     | POST      | Move from one parent to another |
| Search   | `:search`   | GET       | Searching a non-list collection |
| Undelete | `:undelete` | POST      | Undelete resource (< 30 days)   |

## 5. Standard Fields

List of common field definitions

| Name            | Type                | Descriptions                                                                 |
| --------------- | ------------------- | ---------------------------------------------------------------------------- |
| name            | string              | Relative resource name                                                       |
| parent          | string              | LIST/CREATE requests. Relative resource name                                 |
| create_time     | Timestamp           | Time created                                                                 |
| update_time     | Timestamp           | Last updated. CREATE/PATCH/DELETE request                                    |
| delete_time     | Timestamp           | Time deleted, if supports retention                                          |
| expire_time     | Timestamp           | Time expired                                                                 |
| start_time      | Timestamp           | Beginning of some time period                                                |
| end_time        | Timestamp           | End of some time period (reguardless of success)                             |
| read_time       | Timestamp           | Time last read                                                               |
| time_zone       | string              | IANA TZ name (ex. America/Los_Angeles)                                       |
| region_code     | string              | Unicode country/region code (US or 419)                                      |
| language_code   | string              | BCP-47 language code (en-US)                                                 |
| mime_type       | string              | IANA Mime Type                                                               |
| display_name    | string              | Display name of entity                                                       |
| title           | string              | Formal name as apposed to display name                                       |
| description     | string              | 1+ paragraphs about entity                                                   |
| filter          | string              | Standard filter params of LIST method                                        |
| query           | string              | Query params for SEARCH method. Same style as filter                         |
| page_token      | string              | Pagination token of LIST                                                     |
| page_size       | int32               | Pagination size                                                              |
| total_size      | int32               | Total size of LIST reguardless of page size                                  |
| next_page_token | string              | Next page token in response                                                  |
| order_by        | string              | Result order of LIST                                                         |
| request_id      | string              | Unique string id (detect duplicates)                                         |
| resume_token    | string              | opague token for resuming a stream                                           |
| labels          | map<string, string> | Cloud resource label                                                         |
| deleted         | bool                | Must have if supports `undelete`                                             |
| show_deleted    | bool                | Must have if supports `undelete` so user can find deleted items              |
| update_mask     | FieldMask           | UPDATE request for parital updates. Relative to resource not request message |
| validate_only   | bool                | If true request must be validated before executed                            |

## 6. Errors

protocol agnostic errors

``` js
{
  int32 code = 1;
  string message = "Developer facing human readable message in English. Explain and offer actionable solution"
  repeated google.protobuf.Any details = { extraDetail1: "", extraDetail2: "" };
}
```

ROA design: Few error code types. Describe the affected resource. (Ex. NOT_FOUND)

### Should help user understand and resolve issue

- Assume basic knowledge of your API (casual user)
- No knowledge of inner workings
- Such that a casual user can resolve error
- Keep short. Provide link for more information

Do not blindly propogate errors. Throw `INTERNAL` and describe issue. Do not reveal inner workings of system.

Common Request Errors: `INVALID_ARGUMENT`, `NOT_FOUND`, `UNAUTHENTICATED` and `INTERNAL`

## 7. Naming Conventions

### General Naming

All names should be simple and intuitive

Documentation should use formal name (no abbreviations)

Names should not vary across platforms

- API should be correct American English
- Only use commonly accepted abbreviations (Ex. api, config, id, spec, stats)
- Reuse verbage where possible (always say remove)
  - Applies to same name for concept
- Prefer being short and descriptive over reusing a ton of common words
- Consider naming across all available platforms
  - Will this name work in Java, JS, C++, etc.

### Product Naming

Product names should be consistent across API, UI, Docs, ToS, Billing, etc.

| API Name   | Example                            |
| ---------- | ---------------------------------- |
| Product    | Google Calendar API                |
| Service    | calendar.googleapis.com            |
| Package    | google.calendar.v3                 |
| Interface  | google.calendar.v3.CalendarService |
| Source Dir | //google/caledar/v3                |
| API        | calendar                           |

### Service Naming

Resolve to valid DNS name

Use subdomains `xxx.googleapis.com`

API with multiple services should share common prefix `build.googleapis.com` and `buildresults.googleapis.com`

### Package Naming

Consistent with product and service naming `package google.calendar.com`

Abstract API should use proto package naming consistent with product name.

Java packages **MUST** match standard Java proto package naming `com.google.calendar.v3`

### Various Code Namings

- Collections IDs should be plural lowerCamelCase
- Interface is an intuitive noun (Does not conflict with keywords, etc.)
  - Prefix with Api to disambiguate if necessary
- RPC Method verb nount in UpperCamelCase
- RPC Message is named after RPC method with Request or Response suffix
- Enum is UpperCamelCase name
- Wrappers of object should have inner object name suffixed with value
- Fields are lower_underscore_snake_case
  - Adjective before noun
  - Repeated files must be plural forms
  - Number value fiels must include unit of measurement
  - Anything involving time is suffixed with time
  - Anything involving duration is suffixed with duration
  - Anything involving date is suffixed with date