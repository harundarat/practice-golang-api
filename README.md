# Workout Management API

A RESTful API service for managing workout routines and tracking fitness activities. Built with Go, this application provides a robust backend for fitness applications with user authentication, workout management, and detailed exercise tracking capabilities.

## 🎯 Features

- **User Management**: Secure user registration and authentication with JWT tokens
- **Workout Tracking**: Create, read, update, and delete workout sessions
- **Exercise Details**: Track individual exercises with sets, reps, duration, weight, and custom notes
- **Authorization**: Role-based access control ensuring users can only manage their own workouts
- **Database Migrations**: Automated schema management using Goose
- **Docker Support**: Containerized development and deployment with Docker Compose
- **RESTful API**: Clean, intuitive API design following REST principles

## 🛠️ Technology Stack

- **Language**: Go 1.25.0
- **Web Framework**: Chi Router (lightweight, idiomatic HTTP router)
- **Database**: PostgreSQL 12.4
- **Authentication**: JWT (JSON Web Tokens) using golang-jwt/jwt
- **Password Security**: bcrypt hashing
- **Database Migrations**: Goose
- **Database Driver**: pgx/v4
- **Containerization**: Docker & Docker Compose
- **Environment Configuration**: godotenv

## 📊 Database Schema

### Tables

#### Users
- User credentials and profile information
- Secure password hashing
- Unique username and email constraints

#### Workouts
- Workout session metadata
- Duration and calories burned tracking
- User ownership relationship

#### Workout Entries
- Individual exercise details (name, sets, reps/duration, weight)
- Flexible tracking for both strength training and cardio exercises
- Ordered exercise sequences
- Cascade deletion with parent workouts

## 🚀 Getting Started

### Prerequisites

- Go 1.25.0 or higher
- Docker and Docker Compose
- PostgreSQL 12.4 (if running without Docker)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/harundarat/practice-golang-api.git
   cd practice-golang-api
   ```

2. **Start the database**
   ```bash
   docker-compose up -d
   ```

3. **Install dependencies**
   ```bash
   go mod download
   ```

4. **Run the application**
   ```bash
   go run main.go
   ```

   The server will start on `http://localhost:8080` by default.

   To run on a custom port:
   ```bash
   go run main.go -port=3000
   ```

### Running Tests

```bash
go test ./...
```

## 📡 API Endpoints

### Authentication

#### Register a New User
```http
POST /users
Content-Type: application/json

{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "securepassword123",
  "bio": "Fitness enthusiast"
}
```

#### Login
```http
POST /tokens/authentication
Content-Type: application/json

{
  "username": "johndoe",
  "password": "securepassword123"
}
```

**Response:**
```json
{
  "auth_token": {
    "plaintext": "YOUR_JWT_TOKEN",
    "expiry": "2024-01-01T12:00:00Z"
  }
}
```

### Workouts (Requires Authentication)

All workout endpoints require the `Authorization` header with a valid JWT token:
```
Authorization: Bearer YOUR_JWT_TOKEN
```

#### Create a Workout
```http
POST /workouts
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "title": "Morning Strength Training",
  "description": "Full body workout routine",
  "duration_minutes": 60,
  "calories_burned": 400,
  "entries": [
    {
      "exercise_name": "Bench Press",
      "sets": 3,
      "reps": 10,
      "weight": 80.5,
      "notes": "Focus on form",
      "order_index": 1
    },
    {
      "exercise_name": "Running",
      "sets": 1,
      "duration_seconds": 1800,
      "notes": "Warm-up cardio",
      "order_index": 2
    }
  ]
}
```

#### Get a Workout by ID
```http
GET /workouts/{id}
Authorization: Bearer YOUR_JWT_TOKEN
```

#### Update a Workout
```http
PUT /workouts/{id}
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json

{
  "title": "Updated Morning Routine",
  "duration_minutes": 75,
  "calories_burned": 450
}
```

#### Delete a Workout
```http
DELETE /workouts/{id}
Authorization: Bearer YOUR_JWT_TOKEN
```

### Health Check
```http
GET /health
```

## 🔧 Configuration

The application uses the following database configuration (hardcoded in `internal/store/database.go`):

```
Host: localhost
Port: 5432
Database: postgres
User: postgres
Password: postgres
```

For production deployments, it's recommended to use environment variables for configuration.

## 🏗️ Project Structure

```
.
├── internal/
│   ├── api/                # HTTP handlers
│   │   ├── workout_handler.go
│   │   ├── user_handler.go
│   │   └── token_handler.go
│   ├── app/                # Application initialization
│   ├── middleware/         # HTTP middleware (authentication)
│   ├── routes/             # Route definitions
│   ├── store/              # Database operations
│   │   ├── workout_store.go
│   │   ├── user_store.go
│   │   └── tokens.go
│   ├── tokens/             # JWT token management
│   └── utils/              # Utility functions
├── migrations/             # Database migrations
├── docker-compose.yml      # Docker services configuration
├── main.go                 # Application entry point
└── go.mod                  # Go module dependencies
```

## 🔐 Security Features

- Password hashing using bcrypt
- JWT-based authentication with 24-hour token expiry
- User authorization checks on all protected endpoints
- SQL injection protection through prepared statements
- Secure password requirements (minimum 8 characters)

## 🧪 Testing

The project includes unit tests for critical components:

```bash
# Run all tests
go test ./...

# Run tests with coverage
go test -cover ./...

# Run tests for a specific package
go test ./internal/store/...
```

## 📝 Development Notes

### Database Migrations

Migrations are automatically applied on application startup. Migration files are located in the `migrations/` directory and follow the Goose naming convention.

To create a new migration:
```bash
goose -dir migrations create migration_name sql
```

### Docker Compose Services

The `docker-compose.yml` file defines two PostgreSQL instances:
- `workoutDB` (port 5432): Main development database
- `workoutDB_test` (port 5433): Test database

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is available for portfolio and educational purposes.

## 👤 Author

**Harun Darat**
- GitHub: [@harundarat](https://github.com/harundarat)

---

Built with ❤️ using Go
