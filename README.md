
# Reqres Postman Collection

A Postman collection for exercising the public **Reqres** REST API with ready‑made tests, sample payloads, environment variables, and example flows for CRUD, authentication, and delayed responses.

***

## Table of Contents

*   Overview
*   Quick Start
*   Postman Environment
*   Endpoints Covered
*   Test Coverage
*   How to Run the Suite
*   Variables
*   Notes

***

## Overview

This collection demonstrates common API testing patterns:

*   Users: list, get single, create, update (PUT/PATCH), delete
*   Resources: list, get single, and not‑found cases
*   Auth: register/login (success + failure paths)
*   Network behavior: delayed response using `?delay=3`
*   Strong assertions: status codes, headers, strict response shape & types
*   Dynamic data: a pre‑request script generates a random name for create‑user calls

***

## Quick Start

1.  **Import the collection**: `reqres.In.postman_collection.json`.
2.  **Create/import environment** (see JSON below) with:
    *   `base_url` → `https://reqres.in`
    *   `randomName` → left empty (set at runtime by script)
    *   `userId` → `2` (used in single‑user examples)
    *   `x-api-key` → optional demo header (disabled by default)
3.  Select the environment and **Send** any request. Tests run automatically in the **Tests** tab.

***

## Postman Environment

Create a file named **`Reqres.postman_environment.json`** with the following content, then import it in Postman (**Environments → Import**):

```json
{
  "id": "a7b5c7a8-reqres-env-001",
  "name": "Reqres Environment",
  "values": [
    {
      "key": "base_url",
      "value": "https://reqres.in",
      "type": "text",
      "enabled": true
    },
    {
      "key": "randomName",
      "value": "",
      "type": "text",
      "enabled": true,
      "description": "Set at runtime by the collection's Pre-request Script"
    },
    {
      "key": "userId",
      "value": "2",
      "type": "text",
      "enabled": true,
      "description": "Optional: user id used in single-user endpoints"
    },
    {
      "key": "x-api-key",
      "value": "reqres_14a8fa41c0d24ce39c91b6624ccd3416",
      "type": "text",
      "enabled": false,
      "description": "Demo header; Reqres does not require an API key"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_at": "2025-12-18T07:37:07.341845Z",
  "_postman_exported_using": "Postman/10.x"
}
```

***

## Endpoints Covered

### Users

*   **List users**  
    `GET {{base_url}}/api/users?page=2`  
    Validates array item keys and the `support` / `_meta` objects.

*   **Get single user**  
    `GET {{base_url}}/api/users/2`  
    Validates exact values (`id`, `email`, `first_name`, `last_name`, `avatar`) and structure.

*   **User not found**  
    `GET {{base_url}}/api/users/23`  
    Expects **404** and an empty `{}` body.

*   **Create user (random name)**  
    `POST {{base_url}}/api/users`  
    Body uses `{{randomName}}`; expects **201** with `id` and `createdAt`.

*   **Update user (PUT)**  
    `PUT {{base_url}}/api/users/2`  
    Expects **200** with `name`, `job`, and `updatedAt`.

*   **Partial update (PATCH)**  
    `PATCH {{base_url}}/api/users/2`  
    Expects **200** with `name`, `job`, and `updatedAt`.

*   **Delete user**  
    `DELETE {{base_url}}/api/users/2`  
    Expects **204** and checks for `Etag` header.

*   **Delayed response**  
    `GET {{base_url}}/api/users?delay=3`  
    Validates list schema, `support`, and `_meta`.

### Resources

*   **List resources**  
    `GET {{base_url}}/api/unknown`  
    Validates pagination fields and structure of `data[]` items.

*   **Get single resource**  
    `GET {{base_url}}/api/unknown/2`  
    Validates `id`, `name`, `year`, `color`, `pantone_value`.

*   **Resource not found**  
    `GET {{base_url}}/api/unknown/23`  
    Expects **404** and an empty `{}` body.

### Authentication

*   **Register (success)**  
    `POST {{base_url}}/api/register`  
    Expects `id` and `token`.

*   **Register (failure: missing password)**  
    `POST {{base_url}}/api/register`  
    Expects **400** and `error: "Missing password"`.

*   **Login (success)**  
    `POST {{base_url}}/api/login`  
    Expects `token`.

*   **Login (failure: missing password)**  
    `POST {{base_url}}/api/login`  
    Expects **400** and `error: "Missing password"`.

***

## Test Coverage

*   **Status codes**  
    Success paths: `200`, `201`, `204`  
    Negative paths: `400`, `404`

*   **Headers**  
    All JSON responses validate `Content-Type: application/json`; delete flow also checks `Etag`.

*   **Body schema & types**  
    Strict checks for keys on `data`, `support`, and `_meta`; field‑type validations and specific value assertions where applicable.

*   **Pre‑request script**  
    Generates a random name: `Ibrahim_<1–100>` and sets it to the environment variable `{{randomName}}` before create‑user requests.

***

## How to Run the Suite

1.  Open the collection in Postman.
2.  Click **Runner** (Collection Runner).
3.  Select the **Reqres Environment**.
4.  Run all requests; Postman will show pass/fail counts for each test.

***

## Variables

*   `{{base_url}}` — API host (defaults to `https://reqres.in`).
*   `{{randomName}}` — dynamically set by the pre‑request script (e.g., `Ibrahim_57`).
*   `{{userId}}` — optional; defaults to `2` for single‑user examples.
*   `x-api-key` — demo header (disabled by default); Reqres does **not** require an API key.

***

## Notes

*   The API key auth scheme (`x-api-key`) is included only to demonstrate header‑based auth patterns; it’s not needed for Reqres.
*   Error‑path tests assert specific messages like `"Missing password"` for clarity and consistency.
*   You can freely tweak variables (e.g., change `userId`, set custom delays) to explore other scenarios.

***

