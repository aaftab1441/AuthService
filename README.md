# AuthService

**AuthService** is a simple authentication service built using Clean Architecture principles with .NET 9, providing user registration, login, and JWT-based authentication for web applications.

## Features
- User registration with password hashing
- User login with JWT token generation
- Authentication with JWT Bearer tokens
- Clean Architecture (separation of concerns)

## Technologies
- **.NET 9**: Framework used for the application
- **JWT**: For token-based authentication
- **Entity Framework Core**: For database interaction (SQL Server)
- **BCrypt**: For hashing user passwords

## Purpose in the Subscription Management System

The **AuthService** plays a critical role in the **Subscription Management System** by providing secure user authentication and authorization for managing user access to subscription-based services. Here's how it fits into the larger system:

1. **User Registration**: Users can register for the Subscription Management System by creating an account with a unique email and password. The service ensures that sensitive information such as passwords is securely hashed and stored.
   
2. **Login and Token Generation**: Once users have registered, they can log in with their credentials. The service generates a **JWT token** on successful login, which is used for authenticating requests to protected endpoints.

3. **User Authentication**: AuthService ensures that only authenticated users with valid JWT tokens can access the subscription management features. This includes managing subscription plans, payments, and user-specific settings within the system.

4. **Role-Based Access Control**: The service supports different user roles (e.g., `admin`, `user`), which can help in controlling access to different functionalities within the Subscription Management System. For example, an `admin` user can manage subscription plans, while a regular user can only view and manage their subscriptions.

By handling these authentication and authorization aspects, **AuthService** ensures that the Subscription Management System is secure and accessible only to authenticated users with the appropriate permissions.

## Prerequisites
Before running the project, ensure you have the following installed:

- .NET 9 SDK or higher
- SQL Server (or use a connection string for another database)
- Visual Studio or any IDE that supports .NET development

## Setup

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/AuthService.git
cd AuthService
```

### 2. Configure the Database

Make sure your SQL Server instance is running, and update the `appsettings.json` file with your connection string:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=AuthServiceDb;Trusted_Connection=True;"
  },
  "Jwt": {
    "Key": "supersecretkey12345",
    "Issuer": "AuthService",
    "Audience": "AuthService"
  }
}
```

### 3. Install Dependencies

Run the following command to restore the necessary packages:

```bash
dotnet restore
```

### 4. Create and Apply Database Migrations

If you have EF Core tools installed, you can run the following command to create and apply migrations:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

### 5. Run the Application

Now, run the project:

```bash
dotnet run --project AuthService.Api
```

The API will be available at `https://localhost:5001`.

## API Endpoints

### 1. **POST** `/api/auth/register`

Registers a new user with the provided email, password, and role.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securepassword",
  "role": "user"
}
```

**Response**:
```json
{
  "id": "guid",
  "email": "user@example.com",
  "passwordHash": "hashedpassword",
  "role": "user",
  "createdAt": "2024-12-05T00:00:00Z"
}
```

### 2. **POST** `/api/auth/login`

Logs in a user with the provided email and password, and returns a JWT token.

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response**:
```json
{
  "token": "your-jwt-token-here"
}
```

### 3. **Protected Endpoint Example**

Once you have the token, you can access protected endpoints by including the JWT in the `Authorization` header.

Example:
```bash
curl -X GET "https://localhost:5001/api/protected" -H "Authorization: Bearer <your-jwt-token>"
```

## Testing

You can test the AuthService API using tools like [Postman](https://www.postman.com/) or [Swagger](https://swagger.io/tools/swagger-ui/). The Swagger UI should automatically be available at `https://localhost:5001/swagger`.

## Future Improvements
- Implement password reset functionality.
- Add user roles and permissions.
- Add logging and better error handling.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

### Author
[Aftab Ahmed](https://github.com/aaftab1441)
