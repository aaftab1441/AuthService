# Product Catalog Application

## Objective
This application demonstrates the ability to use modern development frameworks and tools. It includes a .NET API using FastEndpoints, a PostgreSQL database with Dapper as the ORM, integration tests using TestContainers, and a Blazor application for data interaction.

## Requirements

### API
- **Endpoints to manage Products**:
  ```json
  {
    "id": 1,
    "name": "Sample Product",
    "price": 10.99,
    "description": "A sample product for testing."
  }
  ```
- **Endpoints**:
  - `GET /products` - Retrieve all products.
  - `GET /products/{id}` - Retrieve a single product by ID.
  - `POST /products` - Create a new product.
  - `PUT /products/{id}` - Update an existing product.
  - `DELETE /products/{id}` - Delete a product.

### Database
- **PostgreSQL** as the database.
- **Dapper** as the ORM.

### Integration Tests
- Integration tests for the API using **TestContainers** to spin up a PostgreSQL container during testing.

### Blazor Application
- A simple Blazor application to display and interact with the product data.
- **Features**:
  - A list view to display all products.
  - A form to create or edit a product (extra credit).
  - Buttons to delete a product (extra credit).

### Aspire Configuration
- Use **Aspire** to wire up the API, database, and Blazor application.
- Include service discovery to allow the Blazor app to interact with the API.
- Provide a script or documentation to set up and run the application locally using Aspire.

## Project Structure
ProductCatalog/
├── ProductApi/
│ ├── Program.cs
│ ├── Startup.cs
│ ├── Endpoints/
│ │ └── ProductEndpoints.cs
│ └── Models/
│ └── Product.cs
├── ProductApp/
│ ├── Program.cs
│ └── Pages/
│ └── ProductPages/
│ ├── Edit.razor
│ ├── Delete.razor
│ └── List.razor
├── Tests/
│ └── IntegrationTests.cs
├── appsettings.json




## Implementation Summary
- **API**: Built using FastEndpoints to handle CRUD operations for products.
- **Database**: Integrated PostgreSQL with Dapper for data access.
- **Blazor Application**: Created a Blazor application to interact with the API, allowing users to view, create, update, and delete products.
- **Integration Tests**: Implemented tests using TestContainers to ensure the API functions correctly with a PostgreSQL database.
- **Aspire**: Used Aspire for service discovery and wiring up the API and Blazor application.

## Setup Instructions

### Prerequisites
- .NET SDK installed
- PostgreSQL installed and running
- Aspire installed

### Configuration
1. **Database Connection**:
   - Update the `appsettings.json` in the `ProductApi` project with your PostgreSQL connection string:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Host=localhost;Port=5432;Database=your_database;Username=your_username;Password=your_password"
     }
   }
   ```

2. **Aspire Configuration**:
   - Ensure that Aspire is configured in both the API and Blazor projects to handle service discovery.

### Running the Application
1. **Set the Startup Project**:
   - Set `ProductCatalog.AppHost` as the startup project in your IDE.

2. **Run the Application**:
   - The application will automatically handle running both the API and Blazor application.

3. **Access the Application**:
   - Open your browser and navigate to `https://localhost:5001` for the Blazor application and `https://localhost:7031` for the API.

### Integration Tests
- To run the integration tests, navigate to the `ProductApi.Tests` directory and run:
```
dotnet test
```


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
