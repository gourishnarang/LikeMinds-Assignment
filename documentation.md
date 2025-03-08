# API Documentation: User Management System

## Overview
This document provides structured API documentation for a User Management System, covering authentication, user registration, profile retrieval, updates, and deletion. It follows REST principles.

For more information on RESTful API principles, refer to [RESTful API Design](https://restfulapi.net/). Additionally, you can learn about JWT authentication from [JWT.io](https://jwt.io/).

## Base URL
`https://api.example.com`

## Authentication
All protected endpoints require a Bearer token in the `Authorization` header.

**Note:** All request body fields are required unless stated otherwise.

---

## API Endpoints

### 1. User Authentication
User authentication ensures secure access by validating user credentials and generating a JWT token for session management.

#### Login
**Endpoint:** `POST /api/auth/login`

**Request Headers:**
```json
{
  "Content-Type": "application/json"
}
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "token": "jwt-token-string",
  "user": {
    "id": 1,
    "name": "John Doe",
    "email": "user@example.com"
  }
}
```

**Error Responses:**
- `400 Bad Request`: Missing required parameters.
- `401 Unauthorized`: Invalid credentials.

#### Logout
Logs out the authenticated user by invalidating the current authentication token.

**Endpoint:** `POST /api/auth/logout`

**Request Headers:**
```json
{
  "Authorization": "Bearer jwt-token-string"
}
```

**Response:**
```json
{
  "message": "User logged out successfully"
}
```

**Error Responses:**
- `401 Unauthorized`: Invalid or expired token.

---

### 2. User Registration
Allows new users to create an account by providing necessary details such as name, email, and password. Ensures email uniqueness.

#### Register a new user
**Endpoint:** `POST /api/auth/register`

**Request Headers:**
```json
{
  "Content-Type": "application/json"
}
```

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "message": "User registered successfully",
  "userId": 1
}
```

**Error Responses:**
- `400 Bad Request`: Invalid input data.
- `409 Conflict`: Email already exists.

---

### 3. Fetch User Profile
Retrieves the details of a specific user based on their user ID. Requires authentication.

#### Get user details
**Endpoint:** `GET /api/users/{id}`

**Request Headers:**
```json
{
  "Authorization": "Bearer jwt-token-string"
}
```

**Response:**
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "user@example.com",
  "createdAt": "2023-05-01T10:00:00Z"
}
```

**Error Responses:**
- `404 Not Found`: User does not exist.

---

### 4. Update User Details
Allows users to update their profile information, such as name and email. Requires authentication.

#### Modify user profile
**Endpoint:** `PUT /api/users/{id}`

**Request Headers:**
```json
{
  "Authorization": "Bearer jwt-token-string",
  "Content-Type": "application/json"
}
```

**Request Body:**
```json
{
  "name": "John Updated",
  "email": "newemail@example.com"
}
```

**Response:**
```json
{
  "message": "User details updated successfully"
}
```

**Error Responses:**
- `400 Bad Request`: Invalid input.
- `403 Forbidden`: Unauthorized access.

---

### 5. Delete User Account
Permanently removes a user account from the system. Only authorized users can perform this action.

#### Remove a user from the system
**Endpoint:** `DELETE /api/users/{id}`

**Request Headers:**
```json
{
  "Authorization": "Bearer jwt-token-string"
}
```

**Response:**
```json
{
  "message": "User deleted successfully"
}
```

**Error Responses:**
- `404 Not Found`: User not found.
- `403 Forbidden`: Unauthorized access.

---

## Response Codes Summary
| Status Code | Meaning |
|------------|---------|
| 200 | OK – Request successful |
| 201 | Created – Resource successfully created |
| 400 | Bad Request – Invalid input |
| 401 | Unauthorized – Authentication required or failed |
| 403 | Forbidden – Access denied |
| 404 | Not Found – Resource not found |
| 409 | Conflict – Duplicate resource |

## Standard Error Response Format
For reference, the list-view error responses above can be converted into JSON format as follows:

**Example:**
- `400 Bad Request`: Missing required parameters.

**Equivalent JSON Representation:**
```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Missing required parameters"
}
```


## Assumptions
- JWT authentication is used for authorization.
- The system follows RESTful API design principles.
- Email uniqueness is enforced at the database level.
- Only authorized users can update or delete accounts.
- Logout invalidates the current authentication token and requires re-login for access.
- All error responses are returned as bullet points to match documentation standards.
- **All request body fields are required unless stated otherwise.**

---
