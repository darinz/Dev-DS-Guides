# JSON Examples

A collection of practical JSON examples for common use cases in web development, APIs, configuration, and data interchange.

## Table of Contents

1. [Basic Objects and Arrays](#basic-objects-and-arrays)
2. [Configuration Files](#configuration-files)
3. [API Payloads](#api-payloads)
4. [Data Interchange](#data-interchange)
5. [JSON Schema Validation](#json-schema-validation)
6. [Advanced Structures](#advanced-structures)

## Basic Objects and Arrays

### Simple Object
```json
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}
```

### Array of Values
```json
["apple", "banana", "orange"]
```

### Array of Objects
```json
[
  { "id": 1, "name": "Alice" },
  { "id": 2, "name": "Bob" },
  { "id": 3, "name": "Charlie" }
]
```

### Nested Objects
```json
{
  "user": {
    "id": 1,
    "profile": {
      "firstName": "Alice",
      "lastName": "Smith"
    },
    "roles": ["admin", "user"]
  }
}
```

## Configuration Files

### Application Config
```json
{
  "app": {
    "name": "MyApp",
    "version": "1.0.0",
    "environment": "production"
  },
  "server": {
    "host": "0.0.0.0",
    "port": 8080,
    "timeout": 30
  },
  "database": {
    "type": "postgresql",
    "host": "localhost",
    "port": 5432,
    "name": "myapp",
    "username": "admin",
    "password": "secret"
  },
  "logging": {
    "level": "info",
    "format": "json",
    "output": "file",
    "file": "/var/log/myapp.log"
  },
  "features": {
    "authentication": true,
    "rateLimiting": true,
    "monitoring": true
  }
}
```

### Environment Variables
```json
{
  "NODE_ENV": "production",
  "PORT": 8080,
  "DATABASE_URL": "postgres://admin:secret@localhost:5432/myapp"
}
```

## API Payloads

### User Registration Request
```json
{
  "email": "alice@example.com",
  "name": "Alice Smith",
  "password": "securepassword123"
}
```

### User Registration Response
```json
{
  "id": "a1b2c3d4",
  "email": "alice@example.com",
  "name": "Alice Smith",
  "createdAt": "2024-06-01T12:00:00Z"
}
```

### Error Response
```json
{
  "error": {
    "code": "USER_EXISTS",
    "message": "A user with this email already exists."
  }
}
```

### Paginated List Response
```json
{
  "users": [
    { "id": 1, "name": "Alice" },
    { "id": 2, "name": "Bob" }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 25,
    "pages": 3
  }
}
```

## Data Interchange

### List of Products
```json
[
  {
    "id": "P001",
    "name": "Laptop",
    "price": 999.99,
    "inStock": true
  },
  {
    "id": "P002",
    "name": "Smartphone",
    "price": 599.99,
    "inStock": false
  }
]
```

### Complex Nested Data
```json
{
  "company": {
    "name": "TechCorp Inc.",
    "departments": [
      {
        "name": "Engineering",
        "head": "Jane Smith",
        "employees": [
          { "id": 1, "name": "Alice" },
          { "id": 2, "name": "Bob" }
        ]
      },
      {
        "name": "Marketing",
        "head": "Frank Garcia",
        "employees": [
          { "id": 3, "name": "Charlie" }
        ]
      }
    ]
  }
}
```

## JSON Schema Validation

### Basic Schema
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "age": { "type": "integer", "minimum": 0 },
    "email": { "type": "string", "format": "email" },
    "roles": {
      "type": "array",
      "items": { "type": "string" }
    },
    "isActive": { "type": "boolean" }
  },
  "required": ["name", "email"]
}
```

### Using $ref for Reusable Definitions
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "definitions": {
    "address": {
      "type": "object",
      "properties": {
        "street": { "type": "string" },
        "city": { "type": "string" },
        "zip": { "type": "string" }
      },
      "required": ["street", "city", "zip"]
    }
  },
  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "address": { "$ref": "#/definitions/address" }
  },
  "required": ["name", "address"]
}
```

### Conditional Validation
```json
{
  "type": "object",
  "properties": {
    "type": { "enum": ["A", "B"] },
    "value": { "type": "string" }
  },
  "if": { "properties": { "type": { "const": "A" } } },
  "then": { "properties": { "value": { "pattern": "^A" } } },
  "else": { "properties": { "value": { "pattern": "^B" } } }
}
```

## Advanced Structures

### OneOf, AnyOf, AllOf
```json
{
  "type": "object",
  "properties": {
    "value": {
      "oneOf": [
        { "type": "string" },
        { "type": "number" }
      ]
    }
  }
}
```

### Example with Nulls and Booleans
```json
{
  "id": 123,
  "name": null,
  "isActive": false,
  "roles": []
}
```

These JSON examples demonstrate practical usage for configuration, APIs, data interchange, and schema validation in modern development. 