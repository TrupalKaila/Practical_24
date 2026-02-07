# Practical-24

## Overview
This repository contains a .NET Core Web API and a DAL (class library) that implements a repository pattern for employee CRUD operations. The API supports creating, updating, soft-deleting, and retrieving employees with SQL Server as the data store. The DAL encapsulates all SQL operations and connection handling. The API also logs basic actions to `log.txt`. 

## Solution Structure
- `EmployeeAPI/` - ASP.NET Core Web API project that exposes employee endpoints.
- `EmployeeDAL/` - Class library that provides data access (`DbService`, `EmployeeRepository`) and models/DTOs.
- `Practical-24.sln` - Solution file.

## Database Schema
Create the `Employee` table with an identity primary key. Use the following schema as a reference:

```sql
CREATE TABLE Employee (
    EmployeeId INT IDENTITY(1,1) PRIMARY KEY,
    EmployeeName NVARCHAR(100) NOT NULL,
    EmployeeSalary DECIMAL(18,2) NOT NULL,
    DepartmentId INT NOT NULL,
    EmployeeEmail NVARCHAR(150) NOT NULL,
    EmployeeJoiningDate DATETIME NOT NULL,
    EmployeeStatus NVARCHAR(20) NOT NULL,
    EmployeeNotes NVARCHAR(500) NULL
);
```

## Connection String
The DAL uses `DbService` for SQL connections. Update the connection string in `EmployeeDAL/Data/DbService.cs` if needed:

```
Server=(LocalDb)\MSSQLLocalDb;Database=Practical22;Trusted_Connection=True;Encrypt=False;
```

## API Endpoints
Base route: `/api/employee`

### Create Employee
- **Method:** `POST`
- **Route:** `/api/employee`
- **Body:** `AddEmployeeDTO`
```json
{
  "employeeName": "Alex",
  "employeeSalary": 50000,
  "departmentId": 1,
  "employeeEmail": "alex@example.com",
  "employeeJoiningDate": "2024-01-15"
}
```

### Update Employee
- **Method:** `PUT`
- **Route:** `/api/employee`
- **Body:** `Employee`
```json
{
  "employeeId": 1,
  "employeeName": "Alex Updated",
  "employeeSalary": 55000,
  "departmentId": 2,
  "employeeEmail": "alex.updated@example.com",
  "employeeJoiningDate": "2024-01-15",
  "employeeStatus": "Active"
}
```

### Soft Delete (Deactivate Employee)
- **Method:** `DELETE`
- **Route:** `/api/employee/{id}`

### Get Employee(s)
- **Method:** `GET`
- **Route:** `/api/employee` (all employees)
- **Route:** `/api/employee/{id}` (single employee)

## Repository Pattern
All SQL operations are implemented in `EmployeeDAL/Data/EmployeeRepository.cs`, including:
- Create
- Update
- Soft delete (set `EmployeeStatus` to `InActive`)
- Get by ID
- Get all

## Running the API
From the repository root:

```bash
dotnet run --project EmployeeAPI
```

Swagger is enabled in development mode and will be available at:
`https://localhost:<port>/swagger`
