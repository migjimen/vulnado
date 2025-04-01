# Documentation: `Postgres.java`

## Overview
The `Postgres` class provides functionality for interacting with a PostgreSQL database. It includes methods for establishing a database connection, setting up the database schema, inserting seed data, and hashing passwords using the MD5 algorithm. This class is designed to initialize and manage a database for a user and comment system.

---

## Features
### 1. **Database Connection**
The `connection()` method establishes a connection to a PostgreSQL database using credentials and connection details provided via environment variables:
- `PGHOST`: Hostname of the PostgreSQL server.
- `PGDATABASE`: Name of the database.
- `PGUSER`: Username for authentication.
- `PGPASSWORD`: Password for authentication.

### 2. **Database Setup**
The `setup()` method:
- Creates two tables (`users` and `comments`) if they do not already exist.
- Clears any existing data in these tables.
- Inserts seed data into the tables for testing or initialization purposes.

### 3. **Password Hashing**
The `md5()` method generates an MD5 hash for a given input string. This is used to securely store user passwords in the database.

### 4. **Data Insertion**
The class provides two private methods for inserting data into the database:
- `insertUser(String username, String password)`: Inserts a new user into the `users` table with a hashed password.
- `insertComment(String username, String body)`: Inserts a new comment into the `comments` table.

---

## Database Schema
The `setup()` method defines the following schema:

### `users` Table
| Column Name   | Data Type      | Constraints                          |
|---------------|----------------|---------------------------------------|
| `user_id`     | `VARCHAR(36)`  | Primary Key                          |
| `username`    | `VARCHAR(50)`  | Unique, Not Null                     |
| `password`    | `VARCHAR(50)`  | Not Null                             |
| `created_on`  | `TIMESTAMP`    | Not Null                             |
| `last_login`  | `TIMESTAMP`    | Optional                             |

### `comments` Table
| Column Name   | Data Type      | Constraints                          |
|---------------|----------------|---------------------------------------|
| `id`          | `VARCHAR(36)`  | Primary Key                          |
| `username`    | `VARCHAR(36)`  | Foreign Key (linked to `users.username`) |
| `body`        | `VARCHAR(500)` | Not Null                             |
| `created_on`  | `TIMESTAMP`    | Not Null                             |

---

## Seed Data
The `setup()` method inserts the following seed data:

### Users
| Username | Password              |
|----------|-----------------------|
| `admin`  | `!!SuperSecretAdmin!!`|
| `alice`  | `AlicePassword!`      |
| `bob`    | `BobPassword!`        |
| `eve`    | `$EVELknev^l`         |
| `rick`   | `!GetSchwifty!`       |

### Comments
| Username | Comment              |
|----------|-----------------------|
| `rick`   | `cool dog m8`         |
| `alice`  | `OMG so cute!`        |

---

## Insights
### Security Concerns
1. **MD5 Hashing**: The use of MD5 for password hashing is considered insecure due to vulnerabilities such as collision attacks. It is recommended to use stronger hashing algorithms like bcrypt or Argon2 for password storage.
2. **Environment Variables**: Sensitive database credentials are retrieved from environment variables, which is a good practice. Ensure these variables are securely managed and not exposed in logs or error messages.

### Scalability
1. **Prepared Statements**: The use of `PreparedStatement` helps prevent SQL injection attacks and improves query performance.
2. **UUIDs**: The use of UUIDs for primary keys ensures uniqueness across distributed systems.

### Error Handling
1. **Connection Failures**: The program exits (`System.exit(1)`) on connection errors, which may not be ideal for production systems. Consider implementing retry mechanisms or logging errors without terminating the application.
2. **Exception Handling**: Exceptions are printed to the console, which may expose sensitive information. Use proper logging frameworks to manage error messages.

### Code Maintainability
1. **Hardcoded Seed Data**: Seed data is hardcoded in the `setup()` method. Consider externalizing this data into configuration files or scripts for easier updates.
2. **Database Schema Management**: The schema creation logic is embedded in the code. For production systems, use database migration tools like Flyway or Liquibase for schema management.

---

## Usage
### Setting Up the Database
1. Ensure the PostgreSQL server is running and the required environment variables (`PGHOST`, `PGDATABASE`, `PGUSER`, `PGPASSWORD`) are set.
2. Call the `Postgres.setup()` method to initialize the database schema and insert seed data.

### Adding Users and Comments
- Use the `insertUser(String username, String password)` method to add new users.
- Use the `insertComment(String username, String body)` method to add new comments.


