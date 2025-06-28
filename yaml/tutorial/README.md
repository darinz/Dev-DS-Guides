# YAML Tutorials

<p align="center">
  <img src="https://img.shields.io/github/license/darinz/Dev-DS-Guides?style=flat-square" alt="License" />
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square" alt="PRs Welcome" />
  <img src="https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white" alt="YAML" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/PyYAML-FF0000?style=flat-square" alt="PyYAML" />
  <img src="https://img.shields.io/badge/Tutorials-Interactive-blue?style=flat-square" alt="Interactive Tutorials" />
</p>

---

> **Comprehensive step-by-step tutorials for mastering YAML with Python, covering syntax, file operations, multiple documents, and practical applications.**

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Tutorial Files](#tutorial-files)
- [Learning Path](#learning-path)
- [Quick Start](#quick-start)
- [Advanced Topics](#advanced-topics)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Introduction

This tutorial collection provides hands-on learning experiences for working with YAML (YAML Ain't Markup Language) in Python applications. YAML is a human-readable data serialization standard commonly used for configuration files, data exchange, and application settings.

### What You'll Learn

- **YAML Syntax Fundamentals**: Master the core syntax and data structures
- **Python Integration**: Use PyYAML for reading and writing YAML files
- **Advanced Features**: Work with anchors, aliases, and multiple documents
- **Real-world Applications**: Apply YAML in configuration management and data processing
- **Best Practices**: Follow industry standards for YAML development

## Prerequisites

Before starting these tutorials, ensure you have:

- **Python 3.6+** installed on your machine
- **PyYAML library** installed (`pip install pyyaml`)
- **Text editor** with YAML support (VS Code, Sublime Text, etc.)
- **Basic Python knowledge** (dictionaries, lists, file operations)

### Installation

```bash
# Install PyYAML
pip install pyyaml

# Verify installation
python -c "import yaml; print(yaml.__version__)"
```

## Tutorial Files

### [Step-by-Step-Tutorial.md](Step-by-Step-Tutorial.md)
**Complete beginner's guide to YAML file creation and manipulation**

- **YAML Syntax Basics**: Indentation, key-value pairs, lists, and nested structures
- **Text Editor Setup**: Choosing and configuring your development environment
- **File Creation Process**: Step-by-step guide to creating your first YAML file
- **Validation Techniques**: Tools and methods for ensuring YAML correctness
- **Python Integration**: Reading and writing YAML files with PyYAML
- **Practical Examples**: Real-world configuration scenarios

### [YAML-in-Python.md](YAML-in-Python.md)
**Advanced Python integration with YAML**

- **PyYAML Library**: Complete overview of the Python YAML library
- **File Operations**: Reading, writing, and manipulating YAML files
- **Advanced Features**: Working with complex data structures and references
- **Error Handling**: Proper exception handling for YAML operations
- **Performance Optimization**: Best practices for large YAML files
- **Security Considerations**: Safe loading and validation techniques

### [Multiple-Docs.md](Multiple-Docs.md)
**Working with multiple YAML documents in single files**

- **Document Separation**: Understanding the `---` separator syntax
- **Multi-Document Creation**: Creating files with multiple YAML documents
- **Python Processing**: Using `yaml.safe_load_all()` and `yaml.dump_all()`
- **Document Management**: Organizing and accessing multiple documents
- **Use Cases**: When and why to use multiple documents
- **Best Practices**: Structuring multi-document files effectively

## Learning Path

### Beginner Level
1. Start with **[Step-by-Step-Tutorial.md](Step-by-Step-Tutorial.md)** to understand YAML fundamentals
2. Practice creating simple YAML files with basic data structures
3. Learn to validate your YAML syntax using online tools

### Intermediate Level
1. Move to **[YAML-in-Python.md](YAML-in-Python.md)** for Python integration
2. Practice reading and writing YAML files programmatically
3. Experiment with complex data structures and error handling

### Advanced Level
1. Explore **[Multiple-Docs.md](Multiple-Docs.md)** for multi-document workflows
2. Implement YAML in real-world projects
3. Master advanced features like anchors, aliases, and custom tags

## Quick Start

### 1. Create Your First YAML File

```yaml
# config.yaml
app:
  name: MyApplication
  version: 1.0.0

database:
  host: localhost
  port: 5432
  name: myapp_db

settings:
  debug: true
  log_level: info
```

### 2. Read YAML in Python

```python
import yaml

# Read configuration
with open('config.yaml', 'r') as file:
    config = yaml.safe_load(file)

# Access data
print(f"App: {config['app']['name']}")
print(f"Database: {config['database']['host']}:{config['database']['port']}")
```

### 3. Write YAML from Python

```python
import yaml

# Create data structure
data = {
    'user': {
        'name': 'John Doe',
        'email': 'john@example.com',
        'roles': ['admin', 'user']
    }
}

# Write to file
with open('user.yaml', 'w') as file:
    yaml.dump(data, file, default_flow_style=False)
```

## Advanced Topics

### Anchors and Aliases
```yaml
# Define reusable content
defaults: &defaults
  timeout: 30
  retries: 3

# Use in multiple places
service1:
  <<: *defaults
  name: api-service

service2:
  <<: *defaults
  name: web-service
```

### Multiple Documents
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
```

### Custom Tags
```yaml
# Using custom tags
timestamp: !timestamp 2023-12-01T10:30:00Z
binary_data: !binary |
  SGVsbG8gV29ybGQ=
```

## Best Practices

### Do's
- Use consistent indentation (2 spaces recommended)
- Include comments for complex configurations
- Validate YAML syntax before deployment
- Use descriptive key names
- Keep files organized and well-structured

### Don'ts
- Mix tabs and spaces for indentation
- Use overly complex nested structures
- Include sensitive data in YAML files
- Ignore YAML validation errors
- Use inconsistent naming conventions

### Security Guidelines
- Use `yaml.safe_load()` instead of `yaml.load()`
- Validate input data before processing
- Avoid executing arbitrary code from YAML
- Use environment variables for sensitive data

## Troubleshooting

### Common Issues

**Indentation Errors**
```yaml
# ❌ Wrong - mixed indentation
parent:
  child:
   grandchild: value  # Inconsistent spacing

# ✅ Correct - consistent indentation
parent:
  child:
    grandchild: value  # 2 spaces each level
```

**Special Character Escaping**
```yaml
# ❌ Wrong - unescaped special characters
message: "Hello: World"

# ✅ Correct - properly quoted
message: "Hello: World"
# or
message: 'Hello: World'
```

**List vs Dictionary Confusion**
```yaml
# ❌ Wrong - mixing list and dict syntax
items:
  - key: value
  key2: value2

# ✅ Correct - consistent structure
items:
  - key: value
  - key2: value2
```

### Debugging Tips

1. **Use Online Validators**: [YAML Lint](https://www.yamllint.com/), [Code Beautify](https://codebeautify.org/yaml-validator)
2. **Enable Verbose Output**: Use `yaml.dump(data, default_flow_style=False, indent=2)`
3. **Check File Encoding**: Ensure files are saved as UTF-8
4. **Validate Step by Step**: Test each section of your YAML file separately

### Performance Optimization

- Use `yaml.safe_load_all()` for large multi-document files
- Consider streaming for very large files
- Cache parsed YAML data when possible
- Use appropriate data structures for your use case

---

**Ready to start?** Begin with the [Step-by-Step Tutorial](Step-by-Step-Tutorial.md) and work your way through each guide to master YAML with Python!