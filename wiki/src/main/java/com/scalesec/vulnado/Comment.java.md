# Documentation: `Comment` Class

## Overview

The `Comment` class is a Java implementation for managing user comments. It provides functionality to create, retrieve, and delete comments, as well as persist them in a database. The class interacts with a PostgreSQL database using JDBC for database operations.

---

## Class Details

### Package
The class is part of the `com.scalesec.vulnado` package.

### Dependencies
The class relies on the following imports:
- `org.apache.catalina.Server` (unused in the code)
- `java.sql.*` for database operations
- `java.util.*` for utility classes like `Date`, `List`, `ArrayList`, and `UUID`

---

## Attributes

| Attribute Name | Type           | Description                              |
|----------------|----------------|------------------------------------------|
| `id`           | `String`       | Unique identifier for the comment.       |
| `username`     | `String`       | Username of the comment's author.        |
| `body`         | `String`       | Content of the comment.                  |
| `created_on`   | `Timestamp`    | Timestamp indicating when the comment was created. |

---

## Constructor

### `Comment(String id, String username, String body, Timestamp created_on)`
Initializes a new `Comment` object with the provided attributes.

| Parameter      | Type        | Description                              |
|----------------|-------------|------------------------------------------|
| `id`           | `String`    | Unique identifier for the comment.       |
| `username`     | `String`    | Username of the comment's author.        |
| `body`         | `String`    | Content of the comment.                  |
| `created_on`   | `Timestamp` | Timestamp indicating when the comment was created. |

---

## Methods

### `static Comment create(String username, String body)`
Creates a new comment and persists it in the database.

| Parameter      | Type        | Description                              |
|----------------|-------------|------------------------------------------|
| `username`     | `String`    | Username of the comment's author.        |
| `body`         | `String`    | Content of the comment.                  |

**Returns:**  
A `Comment` object if the creation is successful. Throws a `BadRequest` exception if the comment cannot be saved, or a `ServerError` exception in case of other errors.

---

### `static List<Comment> fetch_all()`
Fetches all comments from the database.

**Returns:**  
A `List<Comment>` containing all comments retrieved from the database.

---

### `static Boolean delete(String id)`
Deletes a comment from the database based on its unique identifier.

| Parameter      | Type        | Description                              |
|----------------|-------------|------------------------------------------|
| `id`           | `String`    | Unique identifier of the comment to delete. |

**Returns:**  
`true` if the deletion is successful, otherwise `false`.

---

### `private Boolean commit()`
Commits the current `Comment` object to the database by inserting it into the `comments` table.

**Returns:**  
`true` if the insertion is successful, otherwise `false`.

**Throws:**  
`SQLException` if a database error occurs.

---

## Database Interaction

The class interacts with a PostgreSQL database through the following SQL operations:

| Operation      | SQL Query                                                                 |
|----------------|---------------------------------------------------------------------------|
| Insert Comment | `INSERT INTO comments (id, username, body, created_on) VALUES (?,?,?,?)` |
| Fetch All      | `SELECT * FROM comments;`                                                |
| Delete Comment | `DELETE FROM comments WHERE id = ?`                                      |

---

## Insights

1. **Error Handling**:  
   - The `create` method throws custom exceptions (`BadRequest` and `ServerError`) for better error reporting.
   - Other methods like `fetch_all` and `delete` use `try-catch` blocks but do not propagate exceptions, which may lead to silent failures.

2. **Database Connection**:  
   - The class relies on a `Postgres.connection()` method to establish database connections. This method is assumed to be implemented elsewhere in the project.

3. **UUID for IDs**:  
   - The `create` method generates a unique identifier for each comment using `UUID.randomUUID()`.

4. **Potential Issues**:  
   - The `delete` method always returns `false` due to the `finally` block overriding the return value.
   - The `fetch_all` method does not close the `Statement` or `ResultSet` objects, which could lead to resource leaks.

5. **Thread Safety**:  
   - The class is not thread-safe as it does not use synchronization mechanisms for shared resources like database connections.

6. **Unused Import**:  
   - The `org.apache.catalina.Server` import is not used in the code and can be removed to improve clarity.

7. **SQL Injection Risk**:  
   - The `fetch_all` method uses raw SQL queries, which could be vulnerable to SQL injection if user input is incorporated in the query in the future. Using prepared statements is recommended.

---
