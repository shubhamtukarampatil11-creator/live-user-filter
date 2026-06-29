# Live User Filter

A small full-stack Spring Boot demo app with a plain HTML, CSS, and JavaScript frontend.

The app starts with a login/register screen. After login, users can search, add, edit, and delete profile records from an in-memory Java data store.

## Features

- Login and registration screen
- Simple token-based demo authentication
- Protected user management API
- Live search by name or location
- Add, update, and delete users
- Responsive card-style UI
- In-memory data storage for easy local development

## Tech Stack

- Java 17
- Spring Boot 3.5
- Spring Web
- Gradle wrapper
- Plain HTML, CSS, and JavaScript
- In-memory Java collections

## Project Structure

```text
src/main/java/com/example/liveuserfilter
├── LiveUserFilterApplication.java
├── auth
│   ├── AuthController.java
│   ├── AuthFilter.java
│   ├── AuthService.java
│   ├── AuthRequest.java
│   ├── AuthResponse.java
│   └── AppUser.java
└── user
    ├── UserController.java
    ├── UserService.java
    ├── UserProfile.java
    └── UserRequest.java

src/main/resources/static
├── index.html
├── style.css
└── app.js
```

## Run Locally

From the project folder:

```bash
cd /Users/ramakrishna.patil/IdeaProjects/live-user-filter
./gradlew bootRun
```

Open:

```text
http://localhost:8080/
```

## Demo Login

```text
Username: demo
Password: demo123
```

You can also create a new account from the Register tab.

## Build

```bash
./gradlew clean build
```

The deployable JAR is created at:

```text
build/libs/live-user-filter-0.0.1-SNAPSHOT.jar
```

Run the JAR:

```bash
java -jar build/libs/live-user-filter-0.0.1-SNAPSHOT.jar
```

## API Endpoints

### Authentication

Register:

```http
POST /api/auth/register
Content-Type: application/json

{
  "username": "newuser",
  "password": "pass123"
}
```

Login:

```http
POST /api/auth/login
Content-Type: application/json

{
  "username": "demo",
  "password": "demo123"
}
```

Logout:

```http
POST /api/auth/logout
Authorization: Bearer <token>
```

Current user:

```http
GET /api/auth/me
Authorization: Bearer <token>
```

### Users

All `/api/users` endpoints require:

```http
Authorization: Bearer <token>
```

Get all users:

```http
GET /api/users
```

Search users:

```http
GET /api/users?search=india
```

Create user:

```http
POST /api/users
Content-Type: application/json
Authorization: Bearer <token>

{
  "name": "Jane Cooper",
  "location": "Austin, United States",
  "avatarUrl": "https://i.pravatar.cc/300"
}
```

Update user:

```http
PUT /api/users/1
Content-Type: application/json
Authorization: Bearer <token>

{
  "name": "Jane Cooper",
  "location": "Dallas, United States",
  "avatarUrl": "https://i.pravatar.cc/301"
}
```

Delete user:

```http
DELETE /api/users/1
Authorization: Bearer <token>
```

## Example Curl Flow

```bash
TOKEN=$(curl -s -X POST http://localhost:8080/api/auth/login \
  -H 'Content-Type: application/json' \
  -d '{"username":"demo","password":"demo123"}' \
  | sed -E 's/.*"token":"([^"]+)".*/\1/')

curl -s http://localhost:8080/api/users \
  -H "Authorization: Bearer $TOKEN"
```

## How Authentication Works

This project uses intentionally simple authentication for learning:

- Accounts are stored in memory.
- Sessions are stored in memory.
- Login returns a random token.
- The frontend stores the token in `localStorage`.
- Protected user APIs require `Authorization: Bearer <token>`.
- `AuthFilter` blocks unauthenticated requests to `/api/users`.

This is good for understanding the flow, but it is not production security.

## Important Limitations

- Users and login accounts reset when the app restarts.
- Passwords are stored as plain text in memory.
- No database is used yet.
- No Spring Security dependency is used yet.
- No role-based access control.

## Suggested Next Improvements

- Add H2 database for local persistence.
- Add PostgreSQL profile for cloud deployment.
- Replace demo auth with Spring Security.
- Hash passwords with BCrypt.
- Add validation annotations.
- Add unit and integration tests.
- Deploy the executable JAR to AWS Elastic Beanstalk.

## Interview Summary

Built a full-stack Java web app with Spring Boot REST APIs, a plain JavaScript frontend, simple token-based authentication, protected CRUD operations, and live search over in-memory data.
