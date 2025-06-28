# Working with Multiple YAML Documents

<p align="center">
  <img src="https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white" alt="YAML" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Multi-Document-Advanced-blue?style=flat-square" alt="Multi-Document" />
  <img src="https://img.shields.io/badge/PyYAML-FF0000?style=flat-square" alt="PyYAML" />
</p>

---

> **Comprehensive guide to working with multiple YAML documents in single files, covering creation, parsing, manipulation, and real-world applications.**

## Table of Contents

- [Introduction](#introduction)
- [Understanding Multi-Document YAML](#understanding-multi-document-yaml)
- [Creating Multiple Documents](#creating-multiple-documents)
- [Reading Multiple Documents](#reading-multiple-documents)
- [Writing Multiple Documents](#writing-multiple-documents)
- [Advanced Operations](#advanced-operations)
- [Real-World Examples](#real-world-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Performance Considerations](#performance-considerations)

## Introduction

YAML supports multiple documents within a single file, separated by `---` (three hyphens). This feature is incredibly useful for organizing related configurations, managing different environments, or storing multiple data structures in one file. This tutorial covers everything you need to know about working with multiple YAML documents.

### Why Use Multiple Documents?

- **Organization**: Group related configurations together
- **Environment Management**: Store dev, staging, and production configs
- **Modularity**: Separate concerns into different documents
- **Efficiency**: Reduce file management overhead
- **Version Control**: Track changes to related configurations together

## Understanding Multi-Document YAML

### Document Separators

YAML uses `---` to separate documents within a single file:

```yaml
# Document 1
---
database:
  host: localhost
  port: 5432

# Document 2
---
settings:
  debug: true
  log_level: info

# Document 3
---
users:
  - name: alice
    role: admin
  - name: bob
    role: user
```

### Document Structure Rules

1. **First document**: No leading `---` required (but recommended)
2. **Subsequent documents**: Must start with `---`
3. **Empty documents**: Can be represented by `---` alone
4. **Comments**: Can appear between documents
5. **Indentation**: Each document is independent

### Valid Multi-Document Examples

```yaml
# Valid: First document without separator
database:
  host: localhost
  port: 5432

---
# Valid: Second document with separator
settings:
  debug: true

---
# Valid: Empty document
---

---
# Valid: Another document
users:
  - name: alice
```

## Creating Multiple Documents

### Basic Multi-Document Creation

```python
import yaml

def create_multi_document_yaml():
    """Create a YAML file with multiple documents."""
    
    # Define multiple documents
    documents = [
        {
            'database': {
                'host': 'localhost',
                'port': 5432,
                'name': 'myapp'
            }
        },
        {
            'server': {
                'host': '0.0.0.0',
                'port': 8080,
                'debug': True
            }
        },
        {
            'users': [
                {'name': 'alice', 'role': 'admin'},
                {'name': 'bob', 'role': 'user'},
                {'name': 'charlie', 'role': 'guest'}
            ]
        }
    ]
    
    # Write to file
    with open('multi_doc.yaml', 'w', encoding='utf-8') as file:
To read multiple documents from a single YAML file in Python, you can use the `yaml.safe_load_all` function provided by PyYAML.

#### Example Code to Read Multiple YAML Documents

```python
import yaml

# Read the YAML file containing multiple documents
with open('multiple_docs.yaml', 'r') as file:
    documents = list(yaml.safe_load_all(file))

# Access each document
database_config = documents[0]
settings_config = documents[1]
users_config = documents[2]

# Print the contents of each document
print("Database Configuration:", database_config)
print("Settings Configuration:", settings_config)
print("Users Configuration:", users_config)
```

### Explanation

- **`yaml.safe_load_all(file)`**: Loads all documents in the YAML file as an iterator. Converting it to a list allows you to access each document individually.
- **Accessing documents**: Each document can be accessed using list indexing (`documents[0]`, `documents[1]`, etc.).

### Step 6: Writing Multiple YAML Documents in Python

To write multiple documents to a single YAML file in Python, you can use the `yaml.dump_all` function.

#### Example Code to Write Multiple YAML Documents

```python
import yaml

# Define multiple documents as separate dictionaries
database_config = {
    'database': {
        'host': 'localhost',
        'port': 5432,
    }
}

settings_config = {
    'settings': {
        'debug': True,
        'log_level': 'info'
    }
}

users_config = {
    'users': [
        {'name': 'alice', 'role': 'admin'},
        {'name': 'bob', 'role': 'user'},
        {'name': 'charlie', 'role': 'guest'}
    ]
}

# List of all documents
documents = [database_config, settings_config, users_config]

# Write the documents to a YAML file
with open('multiple_docs_output.yaml', 'w') as file:
    yaml.dump_all(documents, file)
```

### Explanation

- **Define documents**: Create dictionaries for each document.
- **`yaml.dump_all(documents, file)`**: Writes all documents to the specified file, separating them with `---`.

### Conclusion

Creating multiple YAML documents within a single file is a useful feature for organizing related configurations or data. By separating documents with `---`, you can easily manage multiple YAML structures in one file. Using PyYAML, you can read and write these multi-document files in Python, making it a powerful tool for configuration management and data serialization.