# Using YAML in Python

<p align="center">
  <img src="https://img.shields.io/badge/YAML-CB171E?style=flat-square&logo=yaml&logoColor=white" alt="YAML" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/PyYAML-FF0000?style=flat-square" alt="PyYAML" />
  <img src="https://img.shields.io/badge/Advanced-Intermediate-orange?style=flat-square" alt="Advanced" />
</p>

---

> **Comprehensive guide to integrating YAML with Python applications, covering PyYAML library usage, advanced features, error handling, and best practices.**

## Table of Contents

- [Introduction](#introduction)
- [Installation and Setup](#installation-and-setup)
- [Basic Operations](#basic-operations)
- [Advanced Features](#advanced-features)
- [Error Handling and Validation](#error-handling-and-validation)
- [Performance Optimization](#performance-optimization)
- [Security Considerations](#security-considerations)
- [Real-World Examples](#real-world-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Introduction

YAML (YAML Ain't Markup Language) is a human-readable data serialization format that works seamlessly with Python through the PyYAML library. This tutorial covers everything you need to know about using YAML in Python applications, from basic operations to advanced features and best practices.

### Why Use YAML with Python?

- **Configuration Management**: Easy-to-read configuration files
- **Data Serialization**: Human-readable data exchange format
- **API Documentation**: OpenAPI/Swagger specifications
- **Infrastructure as Code**: Kubernetes, Docker Compose, Ansible
- **Testing**: Test data and fixtures
- **Logging**: Structured log configuration

## Installation and Setup

### Installing PyYAML

```bash
# Install PyYAML
pip install pyyaml

# Verify installation
python -c "import yaml; print(yaml.__version__)"

# For development (optional)
pip install pyyaml[all]  # Includes additional features
```

### Alternative Libraries

```bash
# For better performance
pip install ruamel.yaml

# For schema validation
pip install pyyaml-include
pip install yamale
```

### Basic Import

```python
import yaml

# Check version
print(f"PyYAML version: {yaml.__version__}")

# Test basic functionality
test_data = {'name': 'test', 'value': 42}
yaml_string = yaml.dump(test_data)
print(f"YAML output: {yaml_string}")
```

## Basic Operations

### Reading YAML Files

#### Simple File Reading

```python
import yaml

def read_yaml_file(filename):
    """Read a YAML file and return its content."""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            data = yaml.safe_load(file)
        return data
    except FileNotFoundError:
        print(f"File {filename} not found")
        return None
    except yaml.YAMLError as e:
        print(f"Error parsing YAML: {e}")
        return None

# Usage
config = read_yaml_file('config.yaml')
if config:
    print(f"Configuration loaded: {config}")
```

#### Reading with Default Values

```python
import yaml
from typing import Any, Dict

def read_yaml_with_defaults(filename: str, defaults: Dict[str, Any] = None) -> Dict[str, Any]:
    """Read YAML file with default values."""
    if defaults is None:
        defaults = {}
    
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            data = yaml.safe_load(file) or {}
        
        # Merge with defaults
        merged = defaults.copy()
        merged.update(data)
        return merged
    
    except FileNotFoundError:
        print(f"File {filename} not found, using defaults")
        return defaults
    except yaml.YAMLError as e:
        print(f"Error parsing YAML: {e}")
        return defaults

# Usage with defaults
default_config = {
    'server': {'host': 'localhost', 'port': 8080},
    'database': {'host': 'localhost', 'port': 5432},
    'logging': {'level': 'info'}
}

config = read_yaml_with_defaults('config.yaml', default_config)
print(f"Final config: {config}")
```

### Writing YAML Files

#### Basic Writing

```python
import yaml

def write_yaml_file(data, filename):
    """Write data to a YAML file."""
    try:
        with open(filename, 'w', encoding='utf-8') as file:
            yaml.dump(data, file, default_flow_style=False, indent=2)
        print(f"Data written to {filename}")
        return True
    except Exception as e:
        print(f"Error writing file: {e}")
        return False

# Example usage
app_config = {
    'application': {
        'name': 'MyApp',
        'version': '1.0.0',
        'description': 'A sample application'
    },
    'server': {
        'host': '0.0.0.0',
        'port': 8000,
        'debug': True
    },
    'database': {
        'type': 'postgresql',
        'host': 'localhost',
        'port': 5432,
        'name': 'myapp'
    }
}

write_yaml_file(app_config, 'app_config.yaml')
```

#### Writing with Custom Formatting

```python
import yaml

def write_yaml_formatted(data, filename, **kwargs):
    """Write YAML with custom formatting options."""
    default_options = {
        'default_flow_style': False,
        'indent': 2,
        'width': 80,
        'allow_unicode': True,
        'sort_keys': False
    }
    default_options.update(kwargs)
    
    try:
        with open(filename, 'w', encoding='utf-8') as file:
            yaml.dump(data, file, **default_options)
        return True
    except Exception as e:
        print(f"Error writing file: {e}")
        return False

# Usage with custom formatting
write_yaml_formatted(
    app_config, 
    'formatted_config.yaml',
    indent=4,
    width=120,
    sort_keys=True
)
```

### Working with YAML Strings

#### String to Object

```python
import yaml

# Parse YAML string
yaml_string = """
application:
  name: MyApp
  version: 1.0.0
server:
  host: localhost
  port: 8080
"""

try:
    data = yaml.safe_load(yaml_string)
    print(f"Parsed data: {data}")
except yaml.YAMLError as e:
    print(f"Error parsing YAML string: {e}")
```

#### Object to String

```python
import yaml

# Convert Python object to YAML string
data = {
    'user': {
        'name': 'John Doe',
        'email': 'john@example.com',
        'roles': ['admin', 'user']
    }
}

yaml_string = yaml.dump(data, default_flow_style=False, indent=2)
print("YAML String:")
print(yaml_string)
```

## Advanced Features

### Multiple Documents

#### Reading Multiple Documents

```python
import yaml

def read_multiple_documents(filename):
    """Read multiple YAML documents from a single file."""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            documents = list(yaml.safe_load_all(file))
        return documents
    except Exception as e:
        print(f"Error reading documents: {e}")
        return []

# Example multi-document file content
multi_doc_content = """
---
# Document 1: Database configuration
database:
  host: localhost
  port: 5432
  name: myapp

---
# Document 2: Application settings
settings:
  debug: true
  log_level: info

---
# Document 3: User list
users:
  - name: alice
    role: admin
  - name: bob
    role: user
"""

# Write multi-document file
with open('multi_doc.yaml', 'w') as f:
    f.write(multi_doc_content)

# Read multi-document file
documents = read_multiple_documents('multi_doc.yaml')
for i, doc in enumerate(documents):
    print(f"Document {i+1}: {doc}")
```

#### Writing Multiple Documents

```python
import yaml

def write_multiple_documents(documents, filename):
    """Write multiple documents to a YAML file."""
    try:
        with open(filename, 'w', encoding='utf-8') as file:
            yaml.dump_all(documents, file, default_flow_style=False, indent=2)
        print(f"Multiple documents written to {filename}")
        return True
    except Exception as e:
        print(f"Error writing documents: {e}")
        return False

# Example documents
doc1 = {'database': {'host': 'localhost', 'port': 5432}}
doc2 = {'settings': {'debug': True, 'log_level': 'info'}}
doc3 = {'users': [{'name': 'alice', 'role': 'admin'}]}

documents = [doc1, doc2, doc3]
write_multiple_documents(documents, 'output_multi.yaml')
```

### Anchors and Aliases

#### Using Anchors and Aliases

```python
import yaml

# YAML with anchors and aliases
yaml_with_anchors = """
# Define reusable content
defaults: &defaults
  timeout: 30
  retries: 3
  cache: true

# Use in multiple services
api_service:
  <<: *defaults  # Merge defaults
  name: api
  port: 8000

web_service:
  <<: *defaults  # Merge defaults
  name: web
  port: 8080
"""

# Parse and use
data = yaml.safe_load(yaml_with_anchors)
print("API Service:", data['api_service'])
print("Web Service:", data['web_service'])
```

#### Creating Anchors Programmatically

```python
import yaml

def create_yaml_with_anchors():
    """Create YAML content with anchors programmatically."""
    # Define base configurations
    base_config = {
        'timeout': 30,
        'retries': 3,
        'cache': True
    }
    
    # Create services with shared configuration
    services = {
        'defaults': base_config,
        'api_service': {
            'name': 'api',
            'port': 8000,
            **base_config  # Merge base config
        },
        'web_service': {
            'name': 'web',
            'port': 8080,
            **base_config  # Merge base config
        }
    }
    
    return yaml.dump(services, default_flow_style=False, indent=2)

# Generate YAML with anchors
yaml_content = create_yaml_with_anchors()
print("Generated YAML:")
print(yaml_content)
```

### Custom Tags and Types

#### Working with Custom Tags

```python
import yaml
from datetime import datetime
import base64

# Custom tag constructors
def timestamp_constructor(loader, node):
    """Convert timestamp string to datetime object."""
    value = loader.construct_scalar(node)
    return datetime.fromisoformat(value.replace('Z', '+00:00'))

def binary_constructor(loader, node):
    """Convert base64 string to bytes."""
    value = loader.construct_scalar(node)
    return base64.b64decode(value)

# Register custom constructors
yaml.add_constructor('!timestamp', timestamp_constructor)
yaml.add_constructor('!binary', binary_constructor)

# YAML with custom tags
yaml_with_tags = """
created_at: !timestamp 2023-12-01T10:30:00Z
binary_data: !binary SGVsbG8gV29ybGQ=
"""

# Parse with custom tags
data = yaml.safe_load(yaml_with_tags)
print(f"Created at: {data['created_at']} (type: {type(data['created_at'])})")
print(f"Binary data: {data['binary_data']} (type: {type(data['binary_data'])})")
```

#### Custom Representers

```python
import yaml
from datetime import datetime

# Custom representers
def datetime_representer(dumper, data):
    """Represent datetime as ISO format string."""
    return dumper.represent_scalar('!timestamp', data.isoformat())

# Register custom representers
yaml.add_representer(datetime, datetime_representer)

# Test custom representer
data = {
    'created_at': datetime.now(),
    'message': 'Hello World'
}

yaml_string = yaml.dump(data, default_flow_style=False)
print("YAML with custom datetime representation:")
print(yaml_string)
```

## Error Handling and Validation

### Comprehensive Error Handling

```python
import yaml
from typing import Dict, Any, Tuple, Optional

class YAMLError(Exception):
    """Custom YAML error class."""
    pass

def safe_load_yaml(filename: str) -> Tuple[Optional[Dict[str, Any]], Optional[str]]:
    """Safely load YAML file with detailed error handling."""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            content = yaml.safe_load(file)
        
        if content is None:
            return None, "File is empty or contains only comments"
        
        return content, None
    
    except FileNotFoundError:
        return None, f"File '{filename}' not found"
    except PermissionError:
        return None, f"Permission denied accessing '{filename}'"
    except yaml.YAMLError as e:
        return None, f"YAML parsing error: {e}"
    except UnicodeDecodeError as e:
        return None, f"Encoding error: {e}"
    except Exception as e:
        return None, f"Unexpected error: {e}"

# Usage
data, error = safe_load_yaml('config.yaml')
if error:
    print(f"Error loading YAML: {error}")
else:
    print(f"Successfully loaded: {data}")
```

### YAML Validation

```python
import yaml
from typing import Dict, Any, List, Tuple

def validate_yaml_structure(data: Dict[str, Any], schema: Dict[str, Any]) -> Tuple[bool, List[str]]:
    """Validate YAML data against a simple schema."""
    errors = []
    
    def validate_field(data_dict: Dict, schema_dict: Dict, path: str = ""):
        for key, expected_type in schema_dict.items():
            current_path = f"{path}.{key}" if path else key
            
            if key not in data_dict:
                errors.append(f"Missing required field: {current_path}")
                continue
            
            value = data_dict[key]
            
            if isinstance(expected_type, type):
                if not isinstance(value, expected_type):
                    errors.append(f"Field {current_path} must be {expected_type.__name__}, got {type(value).__name__}")
            elif isinstance(expected_type, dict):
                if not isinstance(value, dict):
                    errors.append(f"Field {current_path} must be a dictionary")
                else:
                    validate_field(value, expected_type, current_path)
            elif isinstance(expected_type, list):
                if not isinstance(value, list):
                    errors.append(f"Field {current_path} must be a list")
                elif expected_type and not all(isinstance(item, expected_type[0]) for item in value):
                    errors.append(f"All items in {current_path} must be {expected_type[0].__name__}")
    
    validate_field(data, schema)
    return len(errors) == 0, errors

# Example schema
config_schema = {
    'application': {
        'name': str,
        'version': str
    },
    'server': {
        'host': str,
        'port': int
    },
    'database': {
        'host': str,
        'port': int,
        'name': str
    }
}

# Test validation
test_config = {
    'application': {
        'name': 'MyApp',
        'version': '1.0.0'
    },
    'server': {
        'host': 'localhost',
        'port': 8080
    },
    'database': {
        'host': 'localhost',
        'port': 5432,
        'name': 'myapp'
    }
}

is_valid, errors = validate_yaml_structure(test_config, config_schema)
if is_valid:
    print("Configuration is valid!")
else:
    print("Validation errors:")
    for error in errors:
        print(f"  - {error}")
```

## Performance Optimization

### Caching and Optimization

```python
import yaml
from functools import lru_cache
import os
from typing import Dict, Any

class YAMLConfigManager:
    """Optimized YAML configuration manager with caching."""
    
    def __init__(self, config_dir: str = "config"):
        self.config_dir = config_dir
        self._cache = {}
        self._file_timestamps = {}
    
    def _get_file_timestamp(self, filename: str) -> float:
        """Get file modification timestamp."""
        try:
            return os.path.getmtime(filename)
        except OSError:
            return 0
    
    def _is_cache_valid(self, filename: str) -> bool:
        """Check if cached data is still valid."""
        if filename not in self._cache:
            return False
        
        current_timestamp = self._get_file_timestamp(filename)
        cached_timestamp = self._file_timestamps.get(filename, 0)
        
        return current_timestamp <= cached_timestamp
    
    def load_config(self, filename: str, use_cache: bool = True) -> Dict[str, Any]:
        """Load configuration with optional caching."""
        full_path = os.path.join(self.config_dir, filename)
        
        if use_cache and self._is_cache_valid(full_path):
            return self._cache[full_path]
        
        try:
            with open(full_path, 'r', encoding='utf-8') as file:
                data = yaml.safe_load(file) or {}
            
            if use_cache:
                self._cache[full_path] = data
                self._file_timestamps[full_path] = self._get_file_timestamp(full_path)
            
            return data
        
        except Exception as e:
            print(f"Error loading config {filename}: {e}")
            return {}
    
    def clear_cache(self):
        """Clear the configuration cache."""
        self._cache.clear()
        self._file_timestamps.clear()
    
    @lru_cache(maxsize=10)
    def get_cached_value(self, filename: str, key_path: str, default=None):
        """Get a specific value from cached configuration."""
        config = self.load_config(filename)
        
        keys = key_path.split('.')
        value = config
        
        for key in keys:
            if isinstance(value, dict) and key in value:
                value = value[key]
            else:
                return default
        
        return value

# Usage
config_manager = YAMLConfigManager()

# Load with caching
app_config = config_manager.load_config('app.yaml')
db_config = config_manager.load_config('database.yaml')

# Get cached values
server_host = config_manager.get_cached_value('app.yaml', 'server.host', 'localhost')
db_port = config_manager.get_cached_value('database.yaml', 'database.port', 5432)
```

### Streaming for Large Files

```python
import yaml
from typing import Iterator, Dict, Any

def stream_yaml_documents(filename: str) -> Iterator[Dict[str, Any]]:
    """Stream YAML documents from a large file."""
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            for document in yaml.safe_load_all(file):
                if document is not None:
                    yield document
    except Exception as e:
        print(f"Error streaming YAML: {e}")

# Usage for large files
def process_large_yaml_file(filename: str):
    """Process a large YAML file document by document."""
    for i, document in enumerate(stream_yaml_documents(filename)):
        print(f"Processing document {i+1}: {type(document)}")
        # Process each document individually
        # This prevents loading the entire file into memory
```

## Security Considerations

### Safe Loading

```python
import yaml

# ❌ Dangerous - allows code execution
def unsafe_load_yaml(filename):
    with open(filename, 'r') as file:
        return yaml.load(file)  # Can execute arbitrary code

# ✅ Safe - prevents code execution
def safe_load_yaml(filename):
    with open(filename, 'r') as file:
        return yaml.safe_load(file)  # Only loads basic data types

# ✅ Even safer - with custom safe loader
class SafeLoader(yaml.SafeLoader):
    """Custom safe loader with additional security."""
    
    def construct_python_object(self, node):
        """Prevent loading of custom Python objects."""
        raise yaml.ConstructorError(
            None, None, 
            "Custom Python objects are not allowed", 
            node.start_mark
        )

def extra_safe_load_yaml(filename):
    with open(filename, 'r') as file:
        return yaml.load(file, Loader=SafeLoader)
```

### Input Validation

```python
import yaml
import re
from typing import Dict, Any

def validate_yaml_security(yaml_content: str) -> Tuple[bool, List[str]]:
    """Validate YAML content for security issues."""
    security_issues = []
    
    # Check for potentially dangerous patterns
    dangerous_patterns = [
        r'!python/object',  # Custom Python objects
        r'!python/name',    # Python names
        r'!python/module',  # Python modules
        r'__import__',      # Import statements
        r'eval\s*\(',       # Eval function calls
        r'exec\s*\(',       # Exec function calls
    ]
    
    for pattern in dangerous_patterns:
        if re.search(pattern, yaml_content, re.IGNORECASE):
            security_issues.append(f"Potentially dangerous pattern found: {pattern}")
    
    # Check for suspicious file operations
    file_operations = [
        r'open\s*\(',       # File opening
        r'file\s*\(',       # File operations
        r'os\.',            # OS module usage
        r'subprocess',      # Subprocess execution
    ]
    
    for pattern in file_operations:
        if re.search(pattern, yaml_content, re.IGNORECASE):
            security_issues.append(f"Suspicious operation found: {pattern}")
    
    return len(security_issues) == 0, security_issues

# Usage
yaml_content = """
# Safe content
database:
  host: localhost
  port: 5432

# Potentially dangerous content
dangerous: !python/object:builtins.eval
  args: ["__import__('os').system('rm -rf /')"]
"""

is_safe, issues = validate_yaml_security(yaml_content)
if not is_safe:
    print("Security issues found:")
    for issue in issues:
        print(f"  - {issue}")
```

## Real-World Examples

### Configuration Management System

```python
import yaml
import os
from typing import Dict, Any, Optional
from pathlib import Path

class ConfigManager:
    """Advanced configuration management system."""
    
    def __init__(self, config_dir: str = "config"):
        self.config_dir = Path(config_dir)
        self.config_dir.mkdir(exist_ok=True)
        self._configs = {}
        self._environment = os.getenv('ENVIRONMENT', 'development')
    
    def load_environment_config(self, config_name: str) -> Dict[str, Any]:
        """Load configuration for specific environment."""
        # Try environment-specific config first
        env_config_file = self.config_dir / f"{config_name}.{self._environment}.yaml"
        base_config_file = self.config_dir / f"{config_name}.yaml"
        
        config = {}
        
        # Load base configuration
        if base_config_file.exists():
            config.update(self._load_yaml_file(base_config_file))
        
        # Override with environment-specific configuration
        if env_config_file.exists():
            env_config = self._load_yaml_file(env_config_file)
            config = self._deep_merge(config, env_config)
        
        return config
    
    def _load_yaml_file(self, file_path: Path) -> Dict[str, Any]:
        """Load YAML file safely."""
        try:
            with open(file_path, 'r', encoding='utf-8') as file:
                return yaml.safe_load(file) or {}
        except Exception as e:
            print(f"Error loading {file_path}: {e}")
            return {}
    
    def _deep_merge(self, base: Dict[str, Any], override: Dict[str, Any]) -> Dict[str, Any]:
        """Deep merge two dictionaries."""
        result = base.copy()
        
        for key, value in override.items():
            if key in result and isinstance(result[key], dict) and isinstance(value, dict):
                result[key] = self._deep_merge(result[key], value)
            else:
                result[key] = value
        
        return result
    
    def save_config(self, config_name: str, data: Dict[str, Any], environment: Optional[str] = None):
        """Save configuration to file."""
        if environment is None:
            environment = self._environment
        
        config_file = self.config_dir / f"{config_name}.{environment}.yaml"
        
        try:
            with open(config_file, 'w', encoding='utf-8') as file:
                yaml.dump(data, file, default_flow_style=False, indent=2)
            print(f"Configuration saved to {config_file}")
        except Exception as e:
            print(f"Error saving configuration: {e}")
    
    def get_config(self, config_name: str) -> Dict[str, Any]:
        """Get configuration with caching."""
        if config_name not in self._configs:
            self._configs[config_name] = self.load_environment_config(config_name)
        return self._configs[config_name]

# Usage example
config_manager = ConfigManager()

# Create sample configurations
base_config = {
    'database': {
        'host': 'localhost',
        'port': 5432,
        'name': 'myapp'
    },
    'server': {
        'host': '0.0.0.0',
        'port': 8080
    }
}

dev_config = {
    'database': {
        'host': 'dev-db.local',
        'name': 'myapp_dev'
    },
    'server': {
        'debug': True
    }
}

# Save configurations
config_manager.save_config('app', base_config, 'base')
config_manager.save_config('app', dev_config, 'development')

# Load configuration
app_config = config_manager.get_config('app')
print(f"Loaded config: {app_config}")
```

### API Configuration System

```python
import yaml
from typing import Dict, Any, List
from dataclasses import dataclass
from pathlib import Path

@dataclass
class DatabaseConfig:
    host: str
    port: int
    name: str
    user: str
    password: str

@dataclass
class ServerConfig:
    host: str
    port: int
    debug: bool
    workers: int

@dataclass
class APIConfig:
    database: DatabaseConfig
    server: ServerConfig
    features: Dict[str, bool]

class APIConfigManager:
    """API configuration manager with type safety."""
    
    def __init__(self, config_file: str = "api_config.yaml"):
        self.config_file = Path(config_file)
        self._config = None
    
    def load_config(self) -> APIConfig:
        """Load and validate API configuration."""
        if not self.config_file.exists():
            raise FileNotFoundError(f"Configuration file {self.config_file} not found")
        
        with open(self.config_file, 'r', encoding='utf-8') as file:
            data = yaml.safe_load(file)
        
        # Validate and create typed configuration
        return self._create_typed_config(data)
    
    def _create_typed_config(self, data: Dict[str, Any]) -> APIConfig:
        """Create typed configuration from dictionary."""
        # Validate database configuration
        db_data = data.get('database', {})
        database = DatabaseConfig(
            host=db_data.get('host', 'localhost'),
            port=db_data.get('port', 5432),
            name=db_data.get('name', 'api'),
            user=db_data.get('user', 'api_user'),
            password=db_data.get('password', '')
        )
        
        # Validate server configuration
        server_data = data.get('server', {})
        server = ServerConfig(
            host=server_data.get('host', '0.0.0.0'),
            port=server_data.get('port', 8000),
            debug=server_data.get('debug', False),
            workers=server_data.get('workers', 4)
        )
        
        # Get features
        features = data.get('features', {})
        
        return APIConfig(database=database, server=server, features=features)
    
    def save_config(self, config: APIConfig):
        """Save typed configuration to YAML file."""
        data = {
            'database': {
                'host': config.database.host,
                'port': config.database.port,
                'name': config.database.name,
                'user': config.database.user,
                'password': config.database.password
            },
            'server': {
                'host': config.server.host,
                'port': config.server.port,
                'debug': config.server.debug,
                'workers': config.server.workers
            },
            'features': config.features
        }
        
        with open(self.config_file, 'w', encoding='utf-8') as file:
            yaml.dump(data, file, default_flow_style=False, indent=2)

# Usage
api_config_manager = APIConfigManager()

# Create sample configuration
sample_config = {
    'database': {
        'host': 'db.example.com',
        'port': 5432,
        'name': 'api_prod',
        'user': 'api_user',
        'password': '${DB_PASSWORD}'
    },
    'server': {
        'host': '0.0.0.0',
        'port': 8000,
        'debug': False,
        'workers': 8
    },
    'features': {
        'enable_auth': True,
        'enable_rate_limiting': True,
        'enable_caching': True
    }
}

# Save sample configuration
with open('api_config.yaml', 'w') as f:
    yaml.dump(sample_config, f, default_flow_style=False, indent=2)

# Load and use typed configuration
try:
    config = api_config_manager.load_config()
    print(f"Database: {config.database.host}:{config.database.port}")
    print(f"Server: {config.server.host}:{config.server.port}")
    print(f"Features: {config.features}")
except Exception as e:
    print(f"Error loading configuration: {e}")
```

## Best Practices

### Configuration Organization

```python
# config/
# ├── base/
# │   ├── database.yaml
# │   ├── server.yaml
# │   └── logging.yaml
# ├── environments/
# │   ├── development.yaml
# │   ├── staging.yaml
# │   └── production.yaml
# └── secrets/
#     └── .env (not in YAML, but referenced)

import yaml
import os
from pathlib import Path
from typing import Dict, Any

class BestPracticeConfigManager:
    """Configuration manager following best practices."""
    
    def __init__(self, config_dir: str = "config"):
        self.config_dir = Path(config_dir)
        self._config_cache = {}
    
    def load_complete_config(self, environment: str = None) -> Dict[str, Any]:
        """Load complete configuration following best practices."""
        if environment is None:
            environment = os.getenv('ENVIRONMENT', 'development')
        
        # Load base configurations
        base_config = self._load_base_configs()
        
        # Load environment-specific overrides
        env_config = self._load_environment_config(environment)
        
        # Merge configurations
        final_config = self._deep_merge(base_config, env_config)
        
        # Resolve environment variables
        final_config = self._resolve_environment_variables(final_config)
        
        return final_config
    
    def _load_base_configs(self) -> Dict[str, Any]:
        """Load all base configuration files."""
        base_dir = self.config_dir / "base"
        config = {}
        
        if base_dir.exists():
            for yaml_file in base_dir.glob("*.yaml"):
                config_name = yaml_file.stem
                config[config_name] = self._load_yaml_file(yaml_file)
        
        return config
    
    def _load_environment_config(self, environment: str) -> Dict[str, Any]:
        """Load environment-specific configuration."""
        env_file = self.config_dir / "environments" / f"{environment}.yaml"
        
        if env_file.exists():
            return self._load_yaml_file(env_file)
        
        return {}
    
    def _load_yaml_file(self, file_path: Path) -> Dict[str, Any]:
        """Load YAML file with error handling."""
        try:
            with open(file_path, 'r', encoding='utf-8') as file:
                return yaml.safe_load(file) or {}
        except Exception as e:
            print(f"Warning: Could not load {file_path}: {e}")
            return {}
    
    def _deep_merge(self, base: Dict[str, Any], override: Dict[str, Any]) -> Dict[str, Any]:
        """Deep merge two dictionaries."""
        result = base.copy()
        
        for key, value in override.items():
            if key in result and isinstance(result[key], dict) and isinstance(value, dict):
                result[key] = self._deep_merge(result[key], value)
            else:
                result[key] = value
        
        return result
    
    def _resolve_environment_variables(self, config: Dict[str, Any]) -> Dict[str, Any]:
        """Resolve environment variable references in configuration."""
        def resolve_value(value):
            if isinstance(value, str) and value.startswith('${') and value.endswith('}'):
                env_var = value[2:-1]
                return os.getenv(env_var, value)
            elif isinstance(value, dict):
                return {k: resolve_value(v) for k, v in value.items()}
            elif isinstance(value, list):
                return [resolve_value(item) for item in value]
            else:
                return value
        
        return resolve_value(config)

# Usage
config_manager = BestPracticeConfigManager()

# Load complete configuration
config = config_manager.load_complete_config('production')
print("Complete configuration loaded successfully")
```

### Error Handling and Logging

```python
import yaml
import logging
from typing import Dict, Any, Optional
from pathlib import Path

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class RobustYAMLManager:
    """YAML manager with comprehensive error handling and logging."""
    
    def __init__(self, config_dir: str = "config"):
        self.config_dir = Path(config_dir)
        self._loaded_files = set()
    
    def load_config_with_logging(self, filename: str) -> Optional[Dict[str, Any]]:
        """Load YAML configuration with comprehensive logging."""
        file_path = self.config_dir / filename
        
        logger.info(f"Attempting to load configuration from {file_path}")
        
        try:
            # Check if file exists
            if not file_path.exists():
                logger.error(f"Configuration file {file_path} does not exist")
                return None
            
            # Check file permissions
            if not os.access(file_path, os.R_OK):
                logger.error(f"No read permission for {file_path}")
                return None
            
            # Load and parse YAML
            with open(file_path, 'r', encoding='utf-8') as file:
                content = yaml.safe_load(file)
            
            if content is None:
                logger.warning(f"Configuration file {file_path} is empty or contains only comments")
                return {}
            
            # Validate basic structure
            if not isinstance(content, dict):
                logger.error(f"Configuration file {file_path} must contain a dictionary at root level")
                return None
            
            self._loaded_files.add(str(file_path))
            logger.info(f"Successfully loaded configuration from {file_path}")
            
            return content
        
        except yaml.YAMLError as e:
            logger.error(f"YAML parsing error in {file_path}: {e}")
            return None
        except UnicodeDecodeError as e:
            logger.error(f"Encoding error in {file_path}: {e}")
            return None
        except Exception as e:
            logger.error(f"Unexpected error loading {file_path}: {e}")
            return None
    
    def get_loaded_files(self) -> set:
        """Get list of successfully loaded files."""
        return self._loaded_files.copy()
    
    def validate_config_structure(self, config: Dict[str, Any], required_keys: list) -> bool:
        """Validate configuration structure."""
        missing_keys = [key for key in required_keys if key not in config]
        
        if missing_keys:
            logger.error(f"Missing required configuration keys: {missing_keys}")
            return False
        
        logger.info("Configuration structure validation passed")
        return True

# Usage
yaml_manager = RobustYAMLManager()

# Load configuration with logging
config = yaml_manager.load_config_with_logging('app.yaml')

if config:
    # Validate structure
    required_keys = ['database', 'server']
    if yaml_manager.validate_config_structure(config, required_keys):
        print("Configuration is valid and ready to use")
        print(f"Loaded files: {yaml_manager.get_loaded_files()}")
```

## Troubleshooting

### Common Issues and Solutions

```python
import yaml
import sys
from typing import Dict, Any

def troubleshoot_yaml_issues(filename: str):
    """Troubleshoot common YAML issues."""
    print(f"Troubleshooting YAML file: {filename}")
    print("=" * 50)
    
    try:
        with open(filename, 'r', encoding='utf-8') as file:
            content = file.read()
        
        # Check for common issues
        issues = []
        
        # Check for tabs
        if '\t' in content:
            issues.append("File contains tabs - use spaces for indentation")
        
        # Check for mixed indentation
        lines = content.split('\n')
        indent_sizes = set()
        for line in lines:
            if line.strip() and not line.startswith('#'):
                indent = len(line) - len(line.lstrip())
                if indent > 0:
                    indent_sizes.add(indent)
        
        if len(indent_sizes) > 1:
            issues.append("Mixed indentation detected - use consistent spacing")
        
        # Check for special characters
        special_chars = ['&', '*', '!', '|', '>', '"', "'"]
        for char in special_chars:
            if char in content and f"'{char}'" not in content and f'"{char}"' not in content:
                issues.append(f"Special character '{char}' found - consider quoting")
        
        # Try to parse
        try:
            parsed = yaml.safe_load(content)
            if parsed is None:
                issues.append("File is empty or contains only comments")
        except yaml.YAMLError as e:
            issues.append(f"YAML parsing error: {e}")
        
        # Report issues
        if issues:
            print("Issues found:")
            for issue in issues:
                print(f"  - {issue}")
        else:
            print("No issues detected - YAML file appears to be valid")
    
    except FileNotFoundError:
        print(f"Error: File {filename} not found")
    except Exception as e:
        print(f"Error: {e}")

# Usage
troubleshoot_yaml_issues('config.yaml')
```

### Debugging Tools

```python
import yaml
import json
from typing import Any

def debug_yaml_content(yaml_content: str):
    """Debug YAML content by showing parsed structure and types."""
    print("YAML Content Debug")
    print("=" * 30)
    
    try:
        # Parse YAML
        data = yaml.safe_load(yaml_content)
        
        print("Parsed Structure:")
        print(json.dumps(data, indent=2, default=str))
        
        print("\nData Types:")
        def print_types(obj, path=""):
            if isinstance(obj, dict):
                for key, value in obj.items():
                    current_path = f"{path}.{key}" if path else key
                    print(f"{current_path}: {type(value).__name__}")
                    print_types(value, current_path)
            elif isinstance(obj, list):
                for i, item in enumerate(obj):
                    current_path = f"{path}[{i}]"
                    print(f"{current_path}: {type(item).__name__}")
                    print_types(item, current_path)
        
        print_types(data)
        
    except yaml.YAMLError as e:
        print(f"YAML Error: {e}")
    except Exception as e:
        print(f"Error: {e}")

# Example usage
sample_yaml = """
application:
  name: MyApp
  version: 1.0.0
  settings:
    debug: true
    port: 8080
  features:
    - auth
    - logging
    - caching
"""

debug_yaml_content(sample_yaml)
```

---

**You've now mastered YAML integration with Python!** This comprehensive guide covers everything from basic operations to advanced features, security considerations, and real-world applications. 

**Next Steps:**
- Practice with the examples provided
- Implement YAML configuration in your own projects
- Explore advanced features like custom tags and schema validation
- Learn about YAML in other contexts (Kubernetes, Docker, etc.)
- Consider using alternative libraries like `ruamel.yaml` for specific use cases