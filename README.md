# TypeScript CRUD API

A fully-typed TypeScript REST API with Express, Sequelize ORM, and MySQL for user management.

## Features

- Full TypeScript implementation with strict typing
- RESTful CRUD operations for user management
- MySQL database with Sequelize ORM
- Password hashing with bcryptjs
- Request validation using Joi
- Role-based user system (Admin/User)
- Centralized error handling
- Auto-generated database schema

## Tech Stack

- **Runtime**: Node.js
- **Language**: TypeScript
- **Framework**: Express 5
- **Database**: MySQL
- **ORM**: Sequelize
- **Validation**: Joi
- **Security**: bcryptjs for password hashing

## Project Structure

```
typescript-crud-api/
├── src/
│   ├── helpers/
│   │   ├── db.ts              # Database initialization & connection
│   │   └── role.ts            # Role enum (Admin, User)
│   ├── middleware/
│   │   ├── errorHandler.ts    # Global error handling
│   │   └── validateRequest.ts # Joi validation middleware
│   ├── users/
│   │   ├── user.model.ts      # User Sequelize model
│   │   ├── user.service.ts    # Business logic layer
│   │   └── users.controller.ts # Route handlers
│   ├── config.json            # Database & JWT configuration
│   └── server.ts              # Application entry point
├── package.json
└── tsconfig.json
```

## Prerequisites

- Node.js (v16 or higher)
- MySQL (v5.7 or higher)
- npm or yarn

## Setup Instructions

### 1. Install Dependencies

```bash
cd typescript-crud-api
npm install
```

### 2. Configure Database

Update `src/config.json` with your MySQL credentials:

```json
{
  "database": {
    "host": "localhost",
    "port": 3306,
    "user": "root",
    "password": "your_password",
    "database": "typescript_crud_api"
  },
  "jwstSecret": "change-this-in-production-123!"
}
```

### 3. Start MySQL Server

Ensure your MySQL server is running. The application will automatically create the database if it doesn't exist.

### 4. Run the Application

**Development mode** (with auto-reload):
```bash
npm run start:dev
```

**Production mode**:
```bash
npm run build
npm start
```

The server will start on `http://localhost:4000`

## API Endpoints

### Users

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/users` | Get all users | - |
| GET | `/users/:id` | Get user by ID | - |
| POST | `/users` | Create new user | See below |
| PUT | `/users/:id` | Update user | See below |
| DELETE | `/users/:id` | Delete user | - |

### Request Body Examples

**Create User** (POST `/users`):
```json
{
  "title": "Mr",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com",
  "password": "password123",
  "confirmPassword": "password123",
  "role": "User"
}
```

**Update User** (PUT `/users/:id`):
```json
{
  "title": "Dr",
  "firstName": "Jane",
  "email": "jane.doe@example.com"
}
```

All fields are optional for updates. Password requires `confirmPassword` if provided.

## Testing the API

### Using cURL

**Create a user**:
```bash
curl -X POST http://localhost:4000/users \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Mr",
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "password": "password123",
    "confirmPassword": "password123",
    "role": "User"
  }'
```

**Get all users**:
```bash
curl http://localhost:4000/users
```

**Get user by ID**:
```bash
curl http://localhost:4000/users/1
```

**Update user**:
```bash
curl -X PUT http://localhost:4000/users/1 \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Jane",
    "lastName": "Smith"
  }'
```

**Delete user**:
```bash
curl -X DELETE http://localhost:4000/users/1
```

### Using Postman

1. Import the following endpoints into Postman
2. Set base URL: `http://localhost:4000`
3. Set headers: `Content-Type: application/json`
4. Use the request body examples above

### Using the Test Script

Run the included test script:
```bash
npm test
```

## Type Definitions

### User Interface

```typescript
interface UserAttributes {
  id: number;
  email: string;
  passwordHash: string;
  title: string;
  firstName: string;
  lastName: string;
  role: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### Role Enum

```typescript
enum Role {
  Admin = 'Admin',
  User = 'User'
}
```

## Validation Rules

- `email`: Must be valid email format, unique
- `password`: Minimum 6 characters
- `confirmPassword`: Must match password
- `role`: Must be either "Admin" or "User" (defaults to "User")
- `title`, `firstName`, `lastName`: Required strings

## Error Handling

The API returns consistent error responses:

```json
{
  "message": "Error description"
}
```

Status codes:
- `200`: Success
- `400`: Bad request / Validation error
- `404`: Resource not found
- `500`: Internal server error

## Security Features

- Passwords are hashed using bcryptjs (10 salt rounds)
- Password hashes are excluded from default query results
- Input validation on all endpoints
- SQL injection protection via Sequelize ORM

## Development

### Build TypeScript

```bash
npm run build
```

Output will be in the `dist/` directory.

### TypeScript Configuration

The project uses strict TypeScript settings:
- Strict mode enabled
- ES2020 target
- CommonJS modules
- Full type checking

## Troubleshooting

**Database connection fails**:
- Verify MySQL is running
- Check credentials in `config.json`
- Ensure MySQL port 3306 is accessible

**Port 4000 already in use**:
- Set `PORT` environment variable: `PORT=3000 npm start`

**Module not found errors**:
- Run `npm install` to ensure all dependencies are installed
- Clear `node_modules` and reinstall: `rm -rf node_modules && npm install`

## License

ISC
