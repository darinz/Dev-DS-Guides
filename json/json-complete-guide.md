# JSON Complete Reference Guide

A comprehensive guide to JSON (JavaScript Object Notation), covering syntax, data types, schema validation, best practices, and advanced features for APIs, configuration, and data interchange.

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Syntax](#basic-syntax)
3. [Data Types](#data-types)
4. [Objects and Arrays](#objects-and-arrays)
5. [Best Practices](#best-practices)
6. [JSON Schema Validation](#json-schema-validation)
7. [Advanced Features](#advanced-features)
8. [Common Use Cases](#common-use-cases)
9. [Tools and Libraries](#tools-and-libraries)

## Introduction

JSON (JavaScript Object Notation) is a lightweight, text-based data interchange format that is easy for humans to read and write, and easy for machines to parse and generate. It is widely used for APIs, configuration files, and data storage.

### Key Features
- **Human-readable** and easy to write
- **Language-independent** (supported in most programming languages)
- **Hierarchical** (supports nested objects and arrays)
- **Strict syntax** (no comments, strict quoting)
- **Widely used** for web APIs, configuration, and data exchange

### JSON vs Other Formats
| Feature         | JSON | YAML | XML  |
|-----------------|------|------|------|
| Readability     | High | High | Low  |
| Comments        | No   | Yes  | Yes  |
| Data Types      | Basic| Rich | Rich |
| Verbosity       | Low  | Low  | High |
| Schema Support  | Yes  | Yes  | Yes  |
| Language Native | JS   | No   | No   |

## Basic Syntax

- JSON is always UTF-8 encoded text
- Data is represented as key-value pairs (objects) and ordered lists (arrays)
- Keys must be double-quoted strings
- Values can be strings, numbers, objects, arrays, booleans, or null
- No comments allowed
- No trailing commas

### Example
```json
{
  "name": "John Doe",
  "age": 30,
  "isActive": true,
  "email": "john@example.com",
  "roles": ["admin", "user"],
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001"
  },
  "projects": null
}
```

## Data Types

### String
- Double-quoted
- Supports Unicode and escape sequences (\n, \t, \", \\)

### Number
- Integer or floating point
- No leading zeros
- No NaN or Infinity

### Boolean
- true or false

### Null
- null

### Array
- Ordered list of values
- Example: `[1, 2, 3, "a", true, null]`

### Object
- Unordered set of key-value pairs
- Example: `{ "key": "value", "num": 42 }`

## Objects and Arrays

### Objects
```json
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}
```

### Arrays
```json
[
  "apple",
  "banana",
  "orange"
]
```

### Nested Structures
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

## Best Practices

- Always use double quotes for keys and string values
- Avoid trailing commas
- Use consistent indentation (2 or 4 spaces)
- Use meaningful key names (camelCase or snake_case)
- Keep structures simple and avoid deep nesting
- Validate JSON before use
- Do not include comments (use documentation instead)
- Use null for missing values, not empty strings or 0
- Use arrays for ordered data, objects for key-value pairs

## JSON Schema Validation

JSON Schema is a vocabulary for validating the structure and content of JSON data.

### Basic Example
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

### Common Schema Keywords
- `type`: string, number, integer, boolean, object, array, null
- `properties`: object properties
- `items`: array item schema
- `required`: required properties
- `enum`: allowed values
- `minimum`, `maximum`: numeric limits
- `pattern`: regex for strings
- `format`: email, date-time, uri, etc.
- `default`: default value
- `description`: property description

### Validation Tools
- [ajv](https://ajv.js.org/) (JavaScript)
- [jsonschema](https://python-jsonschema.readthedocs.io/) (Python)
- [quicktype](https://quicktype.io/) (TypeScript, C#, Go, etc.)
- [JSON Schema Validator](https://www.jsonschemavalidator.net/)

## Advanced Features

### References and $ref
- Use `$ref` to reuse schema definitions
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

## Common Use Cases

### API Payloads
```json
{
  "userId": 123,
  "action": "login",
  "timestamp": "2024-06-01T12:00:00Z"
}
```

### Configuration Files
```json
{
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
  }
}
```

### Data Interchange
```json
[
  { "id": 1, "name": "Alice" },
  { "id": 2, "name": "Bob" },
  { "id": 3, "name": "Charlie" }
]
```

### Environment Variables (as JSON)
```json
{
  "NODE_ENV": "production",
  "PORT": 8080,
  "DATABASE_URL": "postgres://admin:secret@localhost:5432/myapp"
}
```

## Tools and Libraries

### Command Line Tools
- `jq`: Command-line JSON processor
- `jsonlint`: JSON linter and validator
- `yq`: YAML/JSON processor

### Programming Libraries
- **JavaScript**: `JSON.parse()`, `JSON.stringify()`, [ajv](https://ajv.js.org/)
- **Python**: `json` module, [jsonschema](https://python-jsonschema.readthedocs.io/)
- **Java**: `org.json`, Jackson, Gson
- **Go**: `encoding/json`
- **Ruby**: `json` module

### Online Tools
- [JSONLint](https://jsonlint.com/)
- [JSON Formatter & Validator](https://jsonformatter.curiousconcept.com/)
- [JSON Schema Validator](https://www.jsonschemavalidator.net/)
- [Quicktype](https://quicktype.io/)

This comprehensive JSON guide covers all essential aspects of JSON syntax, data types, schema validation, and best practices for modern development. 