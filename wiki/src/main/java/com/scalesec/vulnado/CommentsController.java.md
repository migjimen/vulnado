# Documentation: CommentsController.java

## Overview
The `CommentsController` class is a RESTful controller implemented using Spring Boot. It provides endpoints for managing comments, including fetching, creating, and deleting comments. The controller also includes mechanisms for authentication and error handling.

## Features
- **Authentication**: Requires an `x-auth-token` header for all endpoints to validate user access.
- **Cross-Origin Resource Sharing (CORS)**: Allows requests from any origin (`origins = "*"`) for all endpoints.
- **Error Handling**: Custom exceptions (`BadRequest` and `ServerError`) are defined to handle specific HTTP error responses.

---

## Endpoints

### 1. **GET `/comments`**
Fetches all comments.

| **Attribute**       | **Details**                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **HTTP Method**      | `GET`                                                                      |
| **Produces**         | `application/json`                                                        |
| **Headers**          | `x-auth-token` (required for authentication)                              |
| **Response**         | List of `Comment` objects                                                 |
| **Authentication**   | Validates the token using `User.assertAuth(secret, token)`                |

---

### 2. **POST `/comments`**
Creates a new comment.

| **Attribute**       | **Details**                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **HTTP Method**      | `POST`                                                                     |
| **Produces**         | `application/json`                                                        |
| **Consumes**         | `application/json`                                                        |
| **Headers**          | `x-auth-token` (required for authentication)                              |
| **Request Body**     | `CommentRequest` object containing `username` and `body`                  |
| **Response**         | A newly created `Comment` object                                           |
| **Authentication**   | Validates the token using `User.assertAuth(secret, token)`                |

---

### 3. **DELETE `/comments/{id}`**
Deletes a comment by its ID.

| **Attribute**       | **Details**                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **HTTP Method**      | `DELETE`                                                                   |
| **Produces**         | `application/json`                                                        |
| **Headers**          | `x-auth-token` (required for authentication)                              |
| **Path Variable**    | `id` (ID of the comment to be deleted)                                     |
| **Response**         | `Boolean` indicating success or failure of the deletion                   |
| **Authentication**   | Validates the token using `User.assertAuth(secret, token)`                |

---

## Supporting Classes

### 1. **CommentRequest**
A data structure used for creating comments. Implements `Serializable`.

| **Field**       | **Type**   | **Description**                     |
|------------------|------------|-------------------------------------|
| `username`       | `String`   | Username of the comment author      |
| `body`           | `String`   | Content of the comment              |

---

### 2. **BadRequest**
A custom exception for handling HTTP 400 (Bad Request) errors.

| **Attribute**       | **Details**                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **HTTP Status**      | `HttpStatus.BAD_REQUEST`                                                   |
| **Constructor**      | Accepts a `String` message describing the error                           |

---

### 3. **ServerError**
A custom exception for handling HTTP 500 (Internal Server Error) errors.

| **Attribute**       | **Details**                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| **HTTP Status**      | `HttpStatus.INTERNAL_SERVER_ERROR`                                         |
| **Constructor**      | Accepts a `String` message describing the error                           |

---

## Insights

1. **Authentication**: The controller relies on a secret value (`app.secret`) for token validation. This secret is injected using the `@Value` annotation, making it configurable via application properties.

2. **CORS Configuration**: The `@CrossOrigin` annotation allows unrestricted access from any origin. While convenient for development, this may pose security risks in production environments.

3. **Error Handling**: Custom exceptions (`BadRequest` and `ServerError`) are mapped to specific HTTP status codes, ensuring consistent error responses.

4. **Scalability**: The `Comment.fetch_all()`, `Comment.create()`, and `Comment.delete()` methods suggest that the `Comment` class encapsulates database operations. This design promotes separation of concerns and scalability.

5. **Serialization**: The `CommentRequest` class implements `Serializable`, which is useful for object serialization during network communication or persistence.

6. **Potential Improvements**:
   - **Token Validation**: The `User.assertAuth()` method is used for authentication, but its implementation is not provided. Ensure robust token validation to prevent unauthorized access.
   - **CORS Restrictions**: Consider restricting origins in production to enhance security.
   - **Error Logging**: Add logging mechanisms for exceptions to aid debugging and monitoring.
