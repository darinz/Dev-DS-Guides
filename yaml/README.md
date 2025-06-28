# YAML Guides

<p align="center">
  <img src="https://img.shields.io/github/license/darinz/Dev-DS-Guides?style=flat-square" alt="License" />
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square" alt="PRs Welcome" />
  <img src="https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white" alt="YAML" />
  <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white" alt="Kubernetes" />
  <img src="https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Docker" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Tutorials-Interactive-blue?style=flat-square" alt="Interactive Tutorials" />
</p>

---

> **Comprehensive YAML guides covering syntax, data structures, configuration files, best practices, and hands-on tutorials for DevOps, Kubernetes, and application configuration.**

## Contents

### Core Guides
- **[yaml-complete-guide.md](yaml-complete-guide.md)**: Complete YAML reference covering syntax, data types, anchors, aliases, and advanced features.
- **[yaml-examples.md](yaml-examples.md)**: Practical YAML examples for configuration files, Kubernetes manifests, Docker Compose, CI/CD pipelines, and application settings.

### Tutorial Collection
- **[tutorial/](tutorial/)**: Comprehensive step-by-step tutorials for mastering YAML with Python
  - **[tutorial/README.md](tutorial/README.md)**: Overview and learning path for all tutorials
  - **[tutorial/Step-by-Step-Tutorial.md](tutorial/Step-by-Step-Tutorial.md)**: Complete beginner's guide to YAML file creation and manipulation
  - **[tutorial/YAML-in-Python.md](tutorial/YAML-in-Python.md)**: Advanced Python integration with PyYAML library
  - **[tutorial/Multiple-Docs.md](tutorial/Multiple-Docs.md)**: Working with multiple YAML documents in single files

## Learning Path

### Beginner Level
Start with the core guides to understand YAML fundamentals:
1. **yaml-complete-guide.md**: Master YAML syntax and data structures
2. **yaml-examples.md**: Practice with real-world examples

### Intermediate Level
Move to the tutorial collection for hands-on learning:
1. **tutorial/Step-by-Step-Tutorial.md**: Create and manipulate YAML files
2. **tutorial/YAML-in-Python.md**: Integrate YAML with Python applications

### Advanced Level
Master advanced concepts and real-world applications:
1. **tutorial/Multiple-Docs.md**: Work with multi-document YAML files
2. Apply knowledge to Kubernetes, Docker Compose, and CI/CD pipelines

## What You'll Learn

### YAML Fundamentals
- **Syntax and Data Types**: Strings, numbers, booleans, lists, and dictionaries
- **Advanced Features**: Anchors, aliases, custom tags, and document separation
- **Best Practices**: Indentation, naming conventions, and security considerations

### Python Integration
- **PyYAML Library**: Reading, writing, and manipulating YAML files
- **Error Handling**: Robust exception handling and validation
- **Performance Optimization**: Caching, streaming, and memory management
- **Security**: Safe loading and input validation

### Real-World Applications
- **Configuration Management**: Application settings and environment-specific configs
- **Infrastructure as Code**: Kubernetes manifests and Docker Compose files
- **CI/CD Pipelines**: GitHub Actions, GitLab CI, and Jenkins configurations
- **API Documentation**: OpenAPI/Swagger specifications

## Quick Start

### Basic YAML Example
```yaml
# config.yaml
application:
  name: MyApp
  version: 1.0.0

server:
  host: localhost
  port: 8080

database:
  type: postgresql
  host: db.example.com
  port: 5432
```

### Python Integration
```python
import yaml

# Read YAML file
with open('config.yaml', 'r') as file:
    config = yaml.safe_load(file)

# Access configuration
print(f"App: {config['application']['name']}")
print(f"Server: {config['server']['host']}:{config['server']['port']}")
```

### Multiple Documents
```yaml
# multi-doc.yaml
---
database:
  host: localhost
  port: 5432

---
settings:
  debug: true
  log_level: info

---
users:
  - name: alice
    role: admin
```

## Use Cases

### Configuration Files
- Application settings and environment variables
- Database connection parameters
- API endpoints and authentication
- Feature flags and business logic

### Infrastructure as Code
- Kubernetes deployments and services
- Docker Compose configurations
- Terraform variables and outputs
- Ansible playbooks and inventories

### Data Serialization
- API request/response payloads
- Logging and monitoring data
- Test fixtures and mock data
- Documentation and specifications

## Best Practices

### File Organization
```
project/
├── config/
│   ├── development.yaml
│   ├── staging.yaml
│   └── production.yaml
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
└── docker/
    └── docker-compose.yaml
```

### Security Guidelines
- Use `yaml.safe_load()` instead of `yaml.load()`
- Validate input data before processing
- Use environment variables for sensitive data
- Avoid executing arbitrary code from YAML

### Naming Conventions
- Use snake_case for keys
- Be descriptive and consistent
- Include comments for complex configurations
- Use version numbers for API specifications

## Tools and Resources

### Online Validators
- [YAML Lint](https://www.yamllint.com/)
- [Code Beautify YAML Validator](https://codebeautify.org/yaml-validator)
- [YAML Validator](https://yamlvalidator.com/)

### Python Libraries
- **PyYAML**: Standard YAML library for Python
- **ruamel.yaml**: Enhanced YAML library with better performance
- **yamale**: Schema validation for YAML files
- **pyyaml-include**: Include other YAML files

### IDE Support
- **VS Code**: Excellent YAML support with extensions
- **Sublime Text**: Lightweight editor with YAML syntax highlighting
- **PyCharm**: Advanced IDE with YAML validation
- **Vim/Emacs**: Terminal editors with YAML plugins

## Contributing

We welcome contributions to improve these guides! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

### Areas for Improvement
- Additional real-world examples
- More advanced use cases
- Performance optimization techniques
- Security best practices
- Integration with other tools

## Related Resources

### Documentation
- [YAML Official Specification](https://yaml.org/spec/)
- [PyYAML Documentation](https://pyyaml.org/wiki/PyYAMLDocumentation)
- [Kubernetes YAML Reference](https://kubernetes.io/docs/concepts/overview/working-with-objects/yaml/)

### Community
- [YAML GitHub Discussions](https://github.com/yaml/yaml/discussions)
- [Stack Overflow YAML Tag](https://stackoverflow.com/questions/tagged/yaml)
- [Reddit r/YAML](https://www.reddit.com/r/YAML/)

---

**Ready to master YAML?** Start with the [tutorial collection](tutorial/) for hands-on learning, then explore the [core guides](yaml-complete-guide.md) for comprehensive reference material.

Use these guides to master YAML for configuration management, infrastructure as code, and application deployment across your development workflow. 