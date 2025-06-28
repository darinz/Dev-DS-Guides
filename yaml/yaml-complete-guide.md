# YAML Complete Reference Guide

A comprehensive guide to YAML (YAML Ain't Markup Language), covering syntax, data structures, and best practices for configuration files and data serialization.

## Table of Contents

1. [Introduction](#introduction)
2. [Basic Syntax](#basic-syntax)
3. [Data Types](#data-types)
4. [Collections](#collections)
5. [Anchors and Aliases](#anchors-and-aliases)
6. [Advanced Features](#advanced-features)
7. [Best Practices](#best-practices)
8. [Common Use Cases](#common-use-cases)
9. [Validation](#validation)
10. [Tools and Libraries](#tools-and-libraries)

## Introduction

YAML is a human-readable data serialization format that is commonly used for configuration files, data exchange, and application settings. It is designed to be simple and easy to read.

### Key Features

- **Human-readable**: Easy to read and write
- **Hierarchical**: Supports nested data structures
- **Language-independent**: Works across programming languages
- **Extensible**: Supports custom data types
- **Comments**: Supports inline and block comments

### YAML vs Other Formats

| Feature | YAML | JSON | XML |
|---------|------|------|-----|
| Readability | High | Medium | Low |
| Comments | Yes | No | Yes |
| Anchors/Aliases | Yes | No | No |
| Schema Support | Yes | Yes | Yes |
| Verbosity | Low | Medium | High |

## Basic Syntax

### Document Structure

```yaml
# YAML document starts with --- (optional)
---
# Content here
---
# Multiple documents in one file
```

### Comments

```yaml
# Single line comment
# Another comment

# Multi-line comment
# can span multiple lines
# for longer explanations
```

### Key-Value Pairs

```yaml
# Simple key-value pair
name: John Doe
age: 30
email: john@example.com

# With quotes (optional)
name: "John Doe"
age: "30"
email: "john@example.com"
```

### Indentation

```yaml
# Use spaces for indentation (not tabs)
# Standard is 2 spaces
person:
  name: John Doe
  age: 30
  address:
    street: 123 Main St
    city: New York
    zip: 10001
```

## Data Types

### Scalars

```yaml
# String (default)
name: John Doe
title: "Software Engineer"

# Integer
age: 30
count: 100

# Float
price: 29.99
temperature: 98.6

# Boolean
active: true
enabled: false

# Null/None
middle_name: null
nickname: ~

# Date/Time
created_at: 2023-12-01T10:30:00Z
birth_date: 1993-05-15

# Binary (base64)
image_data: !!binary |
  SGVsbG8gV29ybGQh
```

### String Types

```yaml
# Literal scalar (preserves newlines)
description: |
  This is a multi-line
  string that preserves
  line breaks.

# Folded scalar (folds newlines to spaces)
summary: >
  This is a multi-line
  string that folds
  line breaks to spaces.

# Double-quoted (escapes special characters)
message: "Hello \"World\" with quotes"

# Single-quoted (no escaping)
message: 'Hello "World" with quotes'
```

## Collections

### Lists/Arrays

```yaml
# Simple list
fruits:
  - apple
  - banana
  - orange

# List of objects
people:
  - name: John Doe
    age: 30
    city: New York
  - name: Jane Smith
    age: 25
    city: Los Angeles

# Inline list
colors: [red, green, blue]
numbers: [1, 2, 3, 4, 5]
```

### Maps/Dictionaries

```yaml
# Simple map
person:
  name: John Doe
  age: 30
  email: john@example.com

# Nested map
company:
  name: Tech Corp
  address:
    street: 123 Business Ave
    city: San Francisco
    zip: 94105
  employees:
    - name: John Doe
      role: Developer
    - name: Jane Smith
      role: Designer

# Inline map
settings: {theme: dark, language: en, notifications: true}
```

### Mixed Collections

```yaml
# Complex nested structure
application:
  name: MyApp
  version: 1.0.0
  config:
    database:
      host: localhost
      port: 5432
      credentials:
        username: admin
        password: secret
    features:
      - authentication
      - logging
      - monitoring
    settings:
      debug: true
      timeout: 30
      retries: 3
```

## Anchors and Aliases

### Basic Anchors

```yaml
# Define an anchor
person: &person_anchor
  name: John Doe
  age: 30
  email: john@example.com

# Reference the anchor
employee: *person_anchor

# Override some values
manager:
  <<: *person_anchor  # Merge with anchor
  role: Manager
  department: Engineering
```

### Complex Anchors

```yaml
# Define reusable components
database_config: &db_config
  host: localhost
  port: 5432
  username: admin
  password: secret

# Use in multiple places
development:
  database: *db_config
  environment: dev

production:
  database:
    <<: *db_config  # Merge and override
    host: prod-db.example.com
    password: ${DB_PASSWORD}
  environment: prod
```

### Anchor with Aliases

```yaml
# Define base configuration
base_config: &base
  timeout: 30
  retries: 3
  logging:
    level: info
    format: json

# Extend base configuration
api_config:
  <<: *base
  endpoint: /api/v1
  rate_limit: 1000

# Another extension
worker_config:
  <<: *base
  queue: default
  concurrency: 5
```

## Advanced Features

### Tags

```yaml
# Built-in tags
binary_data: !!binary SGVsbG8=
timestamp: !!timestamp 2023-12-01T10:30:00Z
null_value: !!null

# Custom tags (implementation dependent)
custom_type: !CustomTag value
```

### Multi-line Strings

```yaml
# Literal style (preserves newlines)
script: |
  #!/bin/bash
  echo "Hello World"
  echo "This is a script"
  exit 0

# Folded style (folds newlines)
description: >
  This is a long description
  that spans multiple lines
  but will be folded into
  a single paragraph.

# Block scalar with indicators
code: |-
  function hello() {
    console.log("Hello World");
  }
```

### Flow Style

```yaml
# Flow style collections
simple_list: [apple, banana, orange]
simple_map: {name: John, age: 30}

# Mixed flow and block
complex:
  list: [item1, item2, item3]
  map: {key1: value1, key2: value2}
  nested:
    - {name: John, age: 30}
    - {name: Jane, age: 25}
```

### Explicit Typing

```yaml
# Explicit type declarations
string_value: !!str 123
integer_value: !!int "456"
float_value: !!float "78.9"
boolean_value: !!bool "true"
```

## Best Practices

### Structure and Organization

```yaml
# Use consistent indentation (2 spaces)
# Group related items together
# Use descriptive names

# Good structure
application:
  name: MyApp
  version: 1.0.0
  
  database:
    host: localhost
    port: 5432
    name: myapp_db
    
  api:
    port: 8080
    timeout: 30
    rate_limit: 1000
    
  logging:
    level: info
    format: json
    file: /var/log/myapp.log
```

### Comments and Documentation

```yaml
# Application configuration
# This file contains all settings for the application

application:
  # Application metadata
  name: MyApp
  version: 1.0.0
  
  # Database configuration
  # Supports PostgreSQL and MySQL
  database:
    type: postgresql  # Options: postgresql, mysql
    host: localhost
    port: 5432
    name: myapp_db
    # Connection pool settings
    pool:
      min: 5
      max: 20
      timeout: 30
```

### Validation and Constraints

```yaml
# Use comments to document constraints
server:
  port: 8080  # Must be between 1024 and 65535
  host: localhost  # Must be valid hostname or IP
  
database:
  pool_size: 10  # Must be positive integer
  timeout: 30  # Must be positive integer (seconds)
  
features:
  authentication: true  # Boolean flag
  logging: true  # Boolean flag
  monitoring: false  # Boolean flag
```

### Environment-Specific Configuration

```yaml
# Base configuration
base_config: &base
  app_name: MyApp
  version: 1.0.0
  database:
    type: postgresql
    pool_size: 10
    timeout: 30

# Development environment
development:
  <<: *base
  database:
    <<: *base
    host: localhost
    port: 5432
    name: myapp_dev
  logging:
    level: debug
    file: /tmp/myapp.log

# Production environment
production:
  <<: *base
  database:
    <<: *base
    host: prod-db.example.com
    port: 5432
    name: myapp_prod
    password: ${DB_PASSWORD}
  logging:
    level: warn
    file: /var/log/myapp.log
```

## Common Use Cases

### Configuration Files

```yaml
# config.yaml
application:
  name: MyApp
  version: 1.0.0
  environment: production
  
server:
  host: 0.0.0.0
  port: 8080
  timeout: 30
  
database:
  host: localhost
  port: 5432
  name: myapp
  username: admin
  password: secret
  
logging:
  level: info
  format: json
  output: file
  file: /var/log/myapp.log
  
features:
  authentication: true
  rate_limiting: true
  monitoring: true
```

### API Documentation

```yaml
# OpenAPI/Swagger specification
openapi: 3.0.0
info:
  title: My API
  version: 1.0.0
  description: API for MyApp

servers:
  - url: https://api.example.com/v1
    description: Production server
  - url: https://staging-api.example.com/v1
    description: Staging server

paths:
  /users:
    get:
      summary: Get users
      parameters:
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        '200':
          description: Success
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        name:
          type: string
        email:
          type: string
          format: email
```

### Data Serialization

```yaml
# Complex data structure
company:
  name: Tech Corp
  founded: 2020
  employees:
    - id: 1
      name: John Doe
      position: CEO
      department: Executive
      skills:
        - leadership
        - strategy
        - management
    - id: 2
      name: Jane Smith
      position: CTO
      department: Engineering
      skills:
        - software_architecture
        - system_design
        - team_leadership
  
  departments:
    engineering:
      head: Jane Smith
      size: 25
      budget: 500000
    marketing:
      head: Bob Johnson
      size: 10
      budget: 200000
    sales:
      head: Alice Brown
      size: 15
      budget: 300000
  
  projects:
    - name: Mobile App
      status: in_progress
      team: engineering
      budget: 100000
      deadline: 2024-06-01
    - name: Website Redesign
      status: completed
      team: marketing
      budget: 50000
      deadline: 2024-03-15
```

## Validation

### Schema Validation

```yaml
# JSON Schema for YAML validation
$schema: http://json-schema.org/draft-07/schema#
type: object
properties:
  application:
    type: object
    required: [name, version]
    properties:
      name:
        type: string
        minLength: 1
      version:
        type: string
        pattern: '^\d+\.\d+\.\d+$'
      environment:
        type: string
        enum: [development, staging, production]
  
  server:
    type: object
    required: [host, port]
    properties:
      host:
        type: string
      port:
        type: integer
        minimum: 1024
        maximum: 65535
      timeout:
        type: integer
        minimum: 1
        default: 30
  
  database:
    type: object
    required: [host, port, name]
    properties:
      host:
        type: string
      port:
        type: integer
        minimum: 1
        maximum: 65535
      name:
        type: string
      username:
        type: string
      password:
        type: string
```

### Custom Validation

```yaml
# Example with validation comments
application:
  # Required: application name (non-empty string)
  name: MyApp
  
  # Required: semantic version (x.y.z format)
  version: 1.0.0
  
  # Optional: environment (dev/staging/prod)
  environment: production

server:
  # Required: host (valid hostname or IP)
  host: localhost
  
  # Required: port (1024-65535)
  port: 8080
  
  # Optional: timeout in seconds (positive integer)
  timeout: 30

database:
  # Required: database host
  host: localhost
  
  # Required: database port (1-65535)
  port: 5432
  
  # Required: database name
  name: myapp
  
  # Optional: connection credentials
  username: admin
  password: secret  # Should be encrypted in production
```

## Tools and Libraries

### Command Line Tools

```bash
# yq - YAML processor
yq eval '.application.name' config.yaml
yq eval '.server.port = 9090' config.yaml

# jq - JSON processor (can work with YAML via conversion)
cat config.yaml | yq eval -o=json | jq '.application.name'

# yamllint - YAML linter
yamllint config.yaml

# yaml2json - Convert YAML to JSON
yaml2json config.yaml > config.json
```

### Programming Libraries

```python
# Python - PyYAML
import yaml

# Load YAML
with open('config.yaml', 'r') as file:
    config = yaml.safe_load(file)

# Dump YAML
with open('output.yaml', 'w') as file:
    yaml.dump(data, file, default_flow_style=False)
```

```javascript
// JavaScript - js-yaml
const yaml = require('js-yaml');
const fs = require('fs');

// Load YAML
const config = yaml.load(fs.readFileSync('config.yaml', 'utf8'));

// Dump YAML
const yamlString = yaml.dump(config);
fs.writeFileSync('output.yaml', yamlString);
```

```ruby
# Ruby - Psych
require 'yaml'

# Load YAML
config = YAML.load_file('config.yaml')

# Dump YAML
File.write('output.yaml', config.to_yaml)
```

### IDE Support

```yaml
# VS Code extensions
# - YAML (Red Hat)
# - YAML Language Support
# - YAML Formatter

# IntelliJ IDEA
# - Built-in YAML support
# - YAML validation
# - Schema validation

# Vim/Neovim
# - vim-yaml
# - yaml.vim
```

This comprehensive YAML guide covers all essential aspects of YAML syntax, data structures, and best practices for configuration management and data serialization. 