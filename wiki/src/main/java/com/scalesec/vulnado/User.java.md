# Documentation: `User.java`

## Overview
The `User` class is part of the `com.scalesec.vulnado` package and provides functionality for user management, including token generation, authentication, and database interaction. It encapsulates user-related data and operations, such as fetching user details from a database and verifying authentication tokens.

---

## Class: `User`

### Attributes
| Attribute       | Type     | Description                                      |
|------------------|----------|--------------------------------------------------|
| `id`            | `String` | Unique identifier for the user.                  |
| `username`      | `String` | Username of the user.                            |
| `hashedPassword`| `String` | Hashed password of the user for secure storage.  |

### Constructor
#### `User(String id, String username, String hashedPassword)`
Initializes a new `User` object with the provided `id`, `username`, and `hashedPassword`.

---

## Methods

### `String token(String secret)`
Generates a JSON Web Token (JWT) for the user using the provided secret key.

#### Parameters:
- `secret` (`String`): The secret key used for signing the JWT.

#### Returns:
- `String`: A compact JWT string representing the user's authentication token.

#### Implementation Details:
- Uses the `io.jsonwebtoken` library to create the JWT.
- The `username` is set as the subject of the token.
- The token is signed using the HMAC SHA algorithm.

---

### `static void assertAuth(String secret, String token)`
Validates the provided JWT token using the secret key.

#### Parameters:
- `secret` (`String`): The secret key used to verify the token.
- `token` (`String`): The JWT token to be validated.

#### Behavior:
- Parses and verifies the token using the `io.jsonwebtoken` library.
- Throws an `Unauthorized` exception if the token is invalid or verification fails.

#### Implementation Details:
- Uses `Keys.hmacShaKeyFor` to generate the signing key from the secret.
- The `JwtParser` is used to parse and validate the token.

---

### `static User fetch(String un)`
Fetches a user from the database based on the provided username.

#### Parameters:
- `un` (`String`): The username of the user to be fetched.

#### Returns:
- `User`: A `User` object populated with data from the database, or `null` if no user is found.

#### Implementation Details:
- Establishes a connection to the database using `Postgres.connection()`.
- Executes a SQL query to retrieve user details from the `users` table.
- Constructs a `User` object using the retrieved data.
- Closes the database connection after execution.

#### Notes:
- The SQL query is vulnerable to SQL injection due to direct concatenation of the `username` parameter. This should be mitigated by using prepared statements.

---

## Insights

### Security Concerns
1. **SQL Injection Vulnerability**:
   - The `fetch` method directly concatenates the `username` parameter into the SQL query, making it susceptible to SQL injection attacks. Use prepared statements to mitigate this risk.

2. **Hardcoded Secret Key Handling**:
   - The `token` and `assertAuth` methods rely on a secret key passed as a parameter. Ensure the secret is securely managed and not hardcoded or exposed in the codebase.

3. **Exception Handling**:
   - The `assertAuth` method catches all exceptions and throws a custom `Unauthorized` exception. While this is useful for abstraction, it may obscure specific error details. Consider logging the original exception for debugging purposes.

### Best Practices
- **Use Prepared Statements**:
  Replace the direct SQL query in the `fetch` method with a prepared statement to prevent SQL injection.

- **Secure Secret Management**:
  Store and retrieve the secret key securely using environment variables or a secrets management tool.

- **Token Expiry**:
  Add an expiration time to the JWT to enhance security and prevent misuse of stale tokens.

### Dependencies
- **Database Connection**:
  The `fetch` method relies on the `Postgres.connection()` method, which is assumed to provide a valid database connection. Ensure this method is implemented securely.

- **JWT Library**:
  The class uses the `io.jsonwebtoken` library for token generation and validation. Ensure the library version is up-to-date to avoid vulnerabilities.

### Potential Enhancements
- Add input validation for the `username` parameter in the `fetch` method.
- Implement logging for better traceability of errors and operations.
- Introduce unit tests to validate the functionality of token generation, authentication, and database interaction.
