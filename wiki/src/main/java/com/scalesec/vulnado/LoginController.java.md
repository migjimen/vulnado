# Documentation: `LoginController.java`

## Overview
The `LoginController` class is a RESTful controller designed to handle user login requests. It validates user credentials and generates a token for authenticated users. The controller leverages Spring Boot annotations and integrates with a database for user authentication.

---

## File Metadata
- **File Name**: `LoginController.java`

---

## Components

### 1. **LoginController**
The main controller class responsible for handling login requests.

#### Annotations:
- `@RestController`: Indicates that this class is a RESTful controller.
- `@EnableAutoConfiguration`: Enables Spring Boot's auto-configuration mechanism.
- `@CrossOrigin(origins = "*")`: Allows cross-origin requests from any domain.

#### Fields:
- `secret`: A private field injected with the application's secret key using the `@Value` annotation.

#### Endpoint:
| **HTTP Method** | **Path**   | **Consumes**       | **Produces**       | **Description**                     |
|------------------|------------|--------------------|--------------------|-------------------------------------|
| POST             | `/login`  | `application/json` | `application/json` | Handles user login requests.        |

#### Method: `login`
- **Parameters**: Accepts a `LoginRequest` object as the request body.
- **Logic**:
  1. Fetches the user details from the database using `User.fetch(input.username)`.
  2. Compares the hashed password from the request with the stored hashed password.
  3. If the credentials match, generates a token using `user.token(secret)` and returns it in a `LoginResponse`.
  4. Throws an `Unauthorized` exception if the credentials are invalid.

---

### 2. **LoginRequest**
A data structure representing the login request payload.

#### Fields:
| **Field**   | **Type**   | **Description**                     |
|-------------|------------|-------------------------------------|
| `username`  | `String`   | The username provided by the user. |
| `password`  | `String`   | The password provided by the user. |

#### Characteristics:
- Implements `Serializable` for object serialization.

---

### 3. **LoginResponse**
A data structure representing the login response payload.

#### Fields:
| **Field** | **Type**   | **Description**                     |
|-----------|------------|-------------------------------------|
| `token`   | `String`   | The authentication token generated. |

#### Constructor:
- Accepts a `String` message (token) and assigns it to the `token` field.

#### Characteristics:
- Implements `Serializable` for object serialization.

---

### 4. **Unauthorized**
A custom exception class used to handle unauthorized access.

#### Annotations:
- `@ResponseStatus(HttpStatus.UNAUTHORIZED)`: Maps this exception to the HTTP 401 Unauthorized status code.

#### Constructor:
- Accepts a `String` exception message and passes it to the superclass (`RuntimeException`).

---

## Insights

### Security Considerations:
1. **Hardcoded Secret**: The `secret` field is injected from the application's configuration. Ensure that the secret is securely stored and not exposed in the source code or logs.
2. **Password Hashing**: The password comparison uses `Postgres.md5`. Verify that this hashing mechanism is secure and up-to-date with modern cryptographic standards.
3. **Cross-Origin Requests**: The `@CrossOrigin` annotation allows requests from any domain. This could be a security risk if not properly restricted in production.

### Error Handling:
- The `Unauthorized` exception provides a clear mechanism for handling invalid login attempts, ensuring proper HTTP status codes are returned.

### Serialization:
- Both `LoginRequest` and `LoginResponse` implement `Serializable`, which is useful for object serialization during data transfer.

### Dependency on External Classes:
- The code relies on external classes (`User` and `Postgres`) for database interaction and password hashing. Ensure these classes are implemented securely and efficiently.

### Scalability:
- The current implementation fetches user details and performs password hashing synchronously. Consider optimizing for scalability in high-traffic scenarios.

---

## Summary Table

| **Component**       | **Type**           | **Purpose**                                      |
|----------------------|--------------------|-------------------------------------------------|
| `LoginController`    | REST Controller   | Handles login requests and validates credentials. |
| `LoginRequest`       | Data Structure    | Represents the login request payload.           |
| `LoginResponse`      | Data Structure    | Represents the login response payload.          |
| `Unauthorized`       | Exception Class   | Handles unauthorized access scenarios.          |
