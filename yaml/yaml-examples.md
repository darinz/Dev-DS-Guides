# YAML Examples

A collection of practical YAML examples for common use cases in DevOps, configuration management, and application deployment.

## Table of Contents

1. [Application Configuration](#application-configuration)
2. [Kubernetes Manifests](#kubernetes-manifests)
3. [Docker Compose](#docker-compose)
4. [CI/CD Pipelines](#cicd-pipelines)
5. [Infrastructure as Code](#infrastructure-as-code)
6. [API Documentation](#api-documentation)
7. [Data Serialization](#data-serialization)

## Application Configuration

### Basic Application Config

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
  max_connections: 1000
  
database:
  type: postgresql
  host: localhost
  port: 5432
  name: myapp
  username: admin
  password: ${DB_PASSWORD}
  pool:
    min: 5
    max: 20
    timeout: 30
    
logging:
  level: info
  format: json
  output: file
  file: /var/log/myapp.log
  rotation:
    max_size: 100MB
    max_files: 10
    
features:
  authentication: true
  rate_limiting: true
  monitoring: true
  caching: true
  
cache:
  type: redis
  host: localhost
  port: 6379
  ttl: 3600
```

### Environment-Specific Configs

```yaml
# config/environments.yaml
base_config: &base
  app_name: MyApp
  version: 1.0.0
  database:
    type: postgresql
    pool:
      min: 5
      max: 20
      timeout: 30
  logging:
    format: json
    output: file

development:
  <<: *base
  environment: development
  database:
    <<: *base
    host: localhost
    port: 5432
    name: myapp_dev
    username: dev_user
    password: dev_password
  logging:
    <<: *base
    level: debug
    file: /tmp/myapp.log
  features:
    authentication: false
    rate_limiting: false
    monitoring: false

staging:
  <<: *base
  environment: staging
  database:
    <<: *base
    host: staging-db.example.com
    port: 5432
    name: myapp_staging
    username: ${STAGING_DB_USER}
    password: ${STAGING_DB_PASSWORD}
  logging:
    <<: *base
    level: info
    file: /var/log/myapp-staging.log
  features:
    authentication: true
    rate_limiting: true
    monitoring: true

production:
  <<: *base
  environment: production
  database:
    <<: *base
    host: prod-db.example.com
    port: 5432
    name: myapp_prod
    username: ${PROD_DB_USER}
    password: ${PROD_DB_PASSWORD}
    ssl: true
  logging:
    <<: *base
    level: warn
    file: /var/log/myapp-prod.log
    rotation:
      max_size: 1GB
      max_files: 30
  features:
    authentication: true
    rate_limiting: true
    monitoring: true
    caching: true
```

## Kubernetes Manifests

### Deployment

```yaml
# k8s/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: production
  labels:
    app: myapp
    version: v1.0.0
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
        version: v1.0.0
    spec:
      containers:
      - name: myapp
        image: myapp:1.0.0
        ports:
        - containerPort: 8080
          name: http
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: myapp-secrets
              key: database-url
        - name: API_KEY
          valueFrom:
            secretKeyRef:
              name: myapp-secrets
              key: api-key
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
```

### Service

```yaml
# k8s/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
  namespace: production
spec:
  selector:
    app: myapp
  ports:
  - name: http
    port: 80
    targetPort: 8080
    protocol: TCP
  type: ClusterIP
```

### Ingress

```yaml
# k8s/ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
  namespace: production
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
  - hosts:
    - myapp.example.com
    secretName: myapp-tls
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              number: 80
```

### ConfigMap

```yaml
# k8s/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: myapp-config
  namespace: production
data:
  app.yaml: |
    application:
      name: MyApp
      version: 1.0.0
      environment: production
    
    server:
      host: 0.0.0.0
      port: 8080
      timeout: 30
    
    database:
      type: postgresql
      host: postgres-service
      port: 5432
      name: myapp
      pool:
        min: 5
        max: 20
        timeout: 30
    
    logging:
      level: info
      format: json
      output: stdout
    
    features:
      authentication: true
      rate_limiting: true
      monitoring: true
```

### Secret

```yaml
# k8s/secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: myapp-secrets
  namespace: production
type: Opaque
data:
  database-url: cG9zdGdyZXM6Ly9hZG1pbjpzZWNyZXRAcG9zdGdyZXMtc2VydmljZTU0MzIvbXlhcHA=
  api-key: c2VjcmV0LWFwaS1rZXk=
  jwt-secret: c2VjcmV0LWp3dC1zZWNyZXQ=
```

## Docker Compose

### Basic Application Stack

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    image: myapp:1.0.0
    container_name: myapp
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=postgresql://admin:secret@postgres:5432/myapp
      - REDIS_URL=redis://redis:6379
      - NODE_ENV=production
    depends_on:
      - postgres
      - redis
    volumes:
      - ./logs:/var/log
    networks:
      - app-network
    restart: unless-stopped

  postgres:
    image: postgres:15
    container_name: postgres
    environment:
      - POSTGRES_DB=myapp
      - POSTGRES_USER=admin
      - POSTGRES_PASSWORD=secret
    volumes:
      - postgres_data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    ports:
      - "5432:5432"
    networks:
      - app-network
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    container_name: redis
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data
    networks:
      - app-network
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - app
    networks:
      - app-network
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:

networks:
  app-network:
    driver: bridge
```

### Development Environment

```yaml
# docker-compose.dev.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    container_name: myapp-dev
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://dev:dev@postgres:5432/myapp_dev
    volumes:
      - .:/app
      - /app/node_modules
    depends_on:
      - postgres
    networks:
      - dev-network
    command: npm run dev

  postgres:
    image: postgres:15
    container_name: postgres-dev
    environment:
      - POSTGRES_DB=myapp_dev
      - POSTGRES_USER=dev
      - POSTGRES_PASSWORD=dev
    volumes:
      - postgres_dev_data:/var/lib/postgresql/data
    ports:
      - "5433:5432"
    networks:
      - dev-network

  redis:
    image: redis:7-alpine
    container_name: redis-dev
    ports:
      - "6380:6379"
    networks:
      - dev-network

volumes:
  postgres_dev_data:

networks:
  dev-network:
    driver: bridge
```

## CI/CD Pipelines

### GitHub Actions

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432

    steps:
    - uses: actions/checkout@v4
    
    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
      env:
        DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test_db
    
    - name: Run linting
      run: npm run lint
    
    - name: Run security audit
      run: npm audit

  build:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v3
    
    - name: Log in to Container Registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    
    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
        tags: |
          type=ref,event=branch
          type=ref,event=pr
          type=semver,pattern={{version}}
          type=semver,pattern={{major}}.{{minor}}
          type=sha
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        push: true
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v4
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-west-2
    
    - name: Update kubeconfig
      run: aws eks update-kubeconfig --name my-cluster --region us-west-2
    
    - name: Deploy to Kubernetes
      run: |
        kubectl set image deployment/myapp myapp=${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
        kubectl rollout status deployment/myapp
```

### GitLab CI

```yaml
# .gitlab-ci.yml
stages:
  - test
  - build
  - deploy

variables:
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: "/certs"

test:
  stage: test
  image: node:18
  services:
    - postgres:15
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: postgres
    POSTGRES_PASSWORD: postgres
    DATABASE_URL: postgresql://postgres:postgres@postgres:5432/test_db
  script:
    - npm ci
    - npm test
    - npm run lint
    - npm audit
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'

build:
  stage: build
  image: docker:20.10.16
  services:
    - docker:20.10.16-dind
  variables:
    DOCKER_HOST: tcp://docker:2376
    DOCKER_TLS_CERTDIR: "/certs"
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - |
      if [ "$CI_COMMIT_BRANCH" = "$CI_DEFAULT_BRANCH" ]; then
        docker tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA $CI_REGISTRY_IMAGE:latest
        docker push $CI_REGISTRY_IMAGE:latest
      fi
  only:
    - main
    - develop

deploy:staging:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache curl
    - curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    - chmod +x kubectl
    - mv kubectl /usr/local/bin/
  script:
    - kubectl config set-cluster k8s --server="$KUBE_URL" --insecure-skip-tls-verify=true
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default
    - kubectl set image deployment/myapp myapp=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA -n staging
    - kubectl rollout status deployment/myapp -n staging
  environment:
    name: staging
    url: https://staging.myapp.com
  only:
    - develop

deploy:production:
  stage: deploy
  image: alpine:latest
  before_script:
    - apk add --no-cache curl
    - curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
    - chmod +x kubectl
    - mv kubectl /usr/local/bin/
  script:
    - kubectl config set-cluster k8s --server="$KUBE_URL" --insecure-skip-tls-verify=true
    - kubectl config set-credentials admin --token="$KUBE_TOKEN"
    - kubectl config set-context default --cluster=k8s --user=admin
    - kubectl config use-context default
    - kubectl set image deployment/myapp myapp=$CI_REGISTRY_IMAGE:$CI_COMMIT_SHA -n production
    - kubectl rollout status deployment/myapp -n production
  environment:
    name: production
    url: https://myapp.com
  only:
    - main
  when: manual
```

## Infrastructure as Code

### Terraform Configuration

```yaml
# terraform/variables.yaml
# Note: This is a YAML representation of Terraform variables
# Actual Terraform uses .tf files, but YAML can be used for variable definitions

variables:
  project_name:
    description: "Name of the project"
    type: string
    default: "myapp"
  
  environment:
    description: "Environment (dev, staging, prod)"
    type: string
    default: "dev"
  
  region:
    description: "AWS region"
    type: string
    default: "us-west-2"
  
  instance_type:
    description: "EC2 instance type"
    type: string
    default: "t3.micro"
  
  vpc_cidr:
    description: "VPC CIDR block"
    type: string
    default: "10.0.0.0/16"
  
  subnet_cidrs:
    description: "Subnet CIDR blocks"
    type: list(string)
    default:
      - "10.0.1.0/24"
      - "10.0.2.0/24"
      - "10.0.3.0/24"
  
  database_instance_class:
    description: "RDS instance class"
    type: string
    default: "db.t3.micro"
  
  database_name:
    description: "Database name"
    type: string
    default: "myapp"
  
  database_username:
    description: "Database username"
    type: string
    default: "admin"
  
  tags:
    description: "Common tags"
    type: map(string)
    default:
      Environment: "dev"
      Project: "myapp"
      ManagedBy: "terraform"
```

### Ansible Playbook

```yaml
# ansible/playbook.yaml
---
- name: Deploy MyApp
  hosts: webservers
  become: yes
  vars:
    app_name: myapp
    app_version: "1.0.0"
    app_port: 8080
    app_user: myapp
    app_group: myapp
    app_dir: /opt/{{ app_name }}
    app_log_dir: /var/log/{{ app_name }}
    
  tasks:
    - name: Update package cache
      apt:
        update_cache: yes
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
    
    - name: Install required packages
      package:
        name:
          - curl
          - wget
          - unzip
          - nginx
        state: present
    
    - name: Create application user
      user:
        name: "{{ app_user }}"
        group: "{{ app_group }}"
        system: yes
        shell: /bin/false
        home: "{{ app_dir }}"
    
    - name: Create application directories
      file:
        path: "{{ item }}"
        state: directory
        owner: "{{ app_user }}"
        group: "{{ app_group }}"
        mode: '0755'
      loop:
        - "{{ app_dir }}"
        - "{{ app_log_dir }}"
        - "{{ app_dir }}/config"
        - "{{ app_dir }}/logs"
    
    - name: Download application
      get_url:
        url: "https://github.com/myorg/{{ app_name }}/releases/download/v{{ app_version }}/{{ app_name }}-{{ app_version }}.tar.gz"
        dest: "/tmp/{{ app_name }}-{{ app_version }}.tar.gz"
        mode: '0644'
    
    - name: Extract application
      unarchive:
        src: "/tmp/{{ app_name }}-{{ app_version }}.tar.gz"
        dest: "{{ app_dir }}"
        remote_src: yes
        owner: "{{ app_user }}"
        group: "{{ app_group }}"
    
    - name: Copy configuration file
      template:
        src: config.yaml.j2
        dest: "{{ app_dir }}/config/config.yaml"
        owner: "{{ app_user }}"
        group: "{{ app_group }}"
        mode: '0644'
    
    - name: Create systemd service file
      template:
        src: myapp.service.j2
        dest: /etc/systemd/system/{{ app_name }}.service
        mode: '0644'
      notify: restart myapp
    
    - name: Configure nginx
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/{{ app_name }}
        mode: '0644'
      notify: restart nginx
    
    - name: Enable nginx site
      file:
        src: /etc/nginx/sites-available/{{ app_name }}
        dest: /etc/nginx/sites-enabled/{{ app_name }}
        state: link
      notify: restart nginx
    
    - name: Start and enable myapp service
      systemd:
        name: "{{ app_name }}"
        state: started
        enabled: yes
        daemon_reload: yes
    
    - name: Start and enable nginx service
      systemd:
        name: nginx
        state: started
        enabled: yes
  
  handlers:
    - name: restart myapp
      systemd:
        name: "{{ app_name }}"
        state: restarted
    
    - name: restart nginx
      systemd:
        name: nginx
        state: restarted
```

## API Documentation

### OpenAPI/Swagger Specification

```yaml
# api/openapi.yaml
openapi: 3.0.3
info:
  title: MyApp API
  description: API for MyApp application
  version: 1.0.0
  contact:
    name: API Support
    email: api@myapp.com
  license:
    name: MIT
    url: https://opensource.org/licenses/MIT

servers:
  - url: https://api.myapp.com/v1
    description: Production server
  - url: https://staging-api.myapp.com/v1
    description: Staging server
  - url: http://localhost:8080/v1
    description: Development server

paths:
  /users:
    get:
      summary: Get users
      description: Retrieve a list of users
      operationId: getUsers
      tags:
        - Users
      parameters:
        - name: page
          in: query
          description: Page number
          required: false
          schema:
            type: integer
            minimum: 1
            default: 1
        - name: limit
          in: query
          description: Number of items per page
          required: false
          schema:
            type: integer
            minimum: 1
            maximum: 100
            default: 10
        - name: search
          in: query
          description: Search term
          required: false
          schema:
            type: string
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserList'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          description: Internal server error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
    
    post:
      summary: Create user
      description: Create a new user
      operationId: createUser
      tags:
        - Users
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserRequest'
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '409':
          description: User already exists
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

  /users/{userId}:
    get:
      summary: Get user by ID
      description: Retrieve a specific user by their ID
      operationId: getUserById
      tags:
        - Users
      parameters:
        - name: userId
          in: path
          required: true
          description: User ID
          schema:
            type: string
            format: uuid
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '404':
          description: User not found
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
          format: uuid
          description: User ID
        email:
          type: string
          format: email
          description: User email
        name:
          type: string
          description: User name
        role:
          type: string
          enum: [user, admin, moderator]
          description: User role
        createdAt:
          type: string
          format: date-time
          description: Creation timestamp
        updatedAt:
          type: string
          format: date-time
          description: Last update timestamp
      required:
        - id
        - email
        - name
        - role
        - createdAt
        - updatedAt
    
    CreateUserRequest:
      type: object
      properties:
        email:
          type: string
          format: email
          description: User email
        name:
          type: string
          description: User name
        password:
          type: string
          format: password
          description: User password
          minLength: 8
        role:
          type: string
          enum: [user, admin, moderator]
          default: user
          description: User role
      required:
        - email
        - name
        - password
    
    UserList:
      type: object
      properties:
        users:
          type: array
          items:
            $ref: '#/components/schemas/User'
        pagination:
          $ref: '#/components/schemas/Pagination'
    
    Pagination:
      type: object
      properties:
        page:
          type: integer
          description: Current page number
        limit:
          type: integer
          description: Items per page
        total:
          type: integer
          description: Total number of items
        pages:
          type: integer
          description: Total number of pages
    
    Error:
      type: object
      properties:
        code:
          type: string
          description: Error code
        message:
          type: string
          description: Error message
        details:
          type: object
          description: Additional error details
      required:
        - code
        - message

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - BearerAuth: []
```

## Data Serialization

### Complex Data Structure

```yaml
# data/company.yaml
company:
  name: TechCorp Inc.
  founded: 2020
  headquarters:
    address: "123 Innovation Drive"
    city: "San Francisco"
    state: "CA"
    zip: "94105"
    country: "USA"
  
  employees:
    - id: "EMP001"
      name: "John Doe"
      position: "CEO"
      department: "Executive"
      email: "john.doe@techcorp.com"
      phone: "+1-555-0101"
      hire_date: "2020-01-15"
      salary: 150000
      skills:
        - "Leadership"
        - "Strategy"
        - "Management"
        - "Business Development"
      projects:
        - name: "Company Vision 2025"
          role: "Lead"
          status: "In Progress"
        - name: "Market Expansion"
          role: "Strategic Advisor"
          status: "Planning"
    
    - id: "EMP002"
      name: "Jane Smith"
      position: "CTO"
      department: "Engineering"
      email: "jane.smith@techcorp.com"
      phone: "+1-555-0102"
      hire_date: "2020-02-01"
      salary: 140000
      skills:
        - "Software Architecture"
        - "System Design"
        - "Team Leadership"
        - "Python"
        - "JavaScript"
        - "DevOps"
      projects:
        - name: "Platform Migration"
          role: "Technical Lead"
          status: "Completed"
        - name: "Microservices Architecture"
          role: "Architect"
          status: "In Progress"
    
    - id: "EMP003"
      name: "Bob Johnson"
      position: "Senior Developer"
      department: "Engineering"
      email: "bob.johnson@techcorp.com"
      phone: "+1-555-0103"
      hire_date: "2020-03-15"
      salary: 120000
      skills:
        - "Python"
        - "Django"
        - "PostgreSQL"
        - "Docker"
        - "AWS"
      projects:
        - name: "API Development"
          role: "Developer"
          status: "In Progress"
        - name: "Database Optimization"
          role: "Lead Developer"
          status: "Completed"
  
  departments:
    executive:
      name: "Executive"
      head: "John Doe"
      budget: 500000
      employee_count: 3
      location: "Floor 10"
      description: "Executive leadership team"
    
    engineering:
      name: "Engineering"
      head: "Jane Smith"
      budget: 2000000
      employee_count: 25
      location: "Floors 5-7"
      description: "Software development and technical operations"
      teams:
        - name: "Backend"
          lead: "Alice Brown"
          size: 8
        - name: "Frontend"
          lead: "Charlie Wilson"
          size: 6
        - name: "DevOps"
          lead: "Diana Davis"
          size: 4
        - name: "QA"
          lead: "Eve Miller"
          size: 7
    
    marketing:
      name: "Marketing"
      head: "Frank Garcia"
      budget: 800000
      employee_count: 12
      location: "Floor 4"
      description: "Marketing and communications"
    
    sales:
      name: "Sales"
      head: "Grace Lee"
      budget: 1200000
      employee_count: 18
      location: "Floor 3"
      description: "Sales and customer success"
  
  projects:
    - id: "PROJ001"
      name: "E-commerce Platform"
      description: "Modern e-commerce platform with microservices architecture"
      status: "In Progress"
      priority: "High"
      start_date: "2023-01-15"
      end_date: "2024-06-30"
      budget: 500000
      spent: 350000
      team:
        - "Jane Smith"
        - "Bob Johnson"
        - "Alice Brown"
        - "Charlie Wilson"
      technologies:
        - "Python"
        - "Django"
        - "React"
        - "PostgreSQL"
        - "Redis"
        - "Docker"
        - "AWS"
      milestones:
        - name: "Requirements Analysis"
          date: "2023-02-15"
          status: "Completed"
        - name: "Architecture Design"
          date: "2023-04-15"
          status: "Completed"
        - name: "Core Development"
          date: "2024-01-15"
          status: "In Progress"
        - name: "Testing & QA"
          date: "2024-04-15"
          status: "Pending"
        - name: "Deployment"
          date: "2024-06-30"
          status: "Pending"
    
    - id: "PROJ002"
      name: "Mobile App"
      description: "Cross-platform mobile application"
      status: "Planning"
      priority: "Medium"
      start_date: "2024-03-01"
      end_date: "2024-12-31"
      budget: 300000
      spent: 0
      team:
        - "Diana Davis"
        - "Eve Miller"
      technologies:
        - "React Native"
        - "TypeScript"
        - "Firebase"
        - "AWS"
      milestones:
        - name: "Design & Prototyping"
          date: "2024-04-01"
          status: "Pending"
        - name: "Development"
          date: "2024-07-01"
          status: "Pending"
        - name: "Testing"
          date: "2024-11-01"
          status: "Pending"
        - name: "App Store Submission"
          date: "2024-12-15"
          status: "Pending"
  
  financials:
    fiscal_year: 2024
    revenue: 5000000
    expenses: 3500000
    profit: 1500000
    growth_rate: 0.25
    key_metrics:
      customer_acquisition_cost: 150
      lifetime_value: 2500
      churn_rate: 0.05
      monthly_recurring_revenue: 416667
  
  technology_stack:
    backend:
      - "Python 3.11"
      - "Django 4.2"
      - "FastAPI"
      - "PostgreSQL 15"
      - "Redis 7"
      - "Celery"
    
    frontend:
      - "React 18"
      - "TypeScript"
      - "Next.js 13"
      - "Tailwind CSS"
    
    infrastructure:
      - "AWS"
      - "Docker"
      - "Kubernetes"
      - "Terraform"
      - "GitHub Actions"
      - "Prometheus"
      - "Grafana"
    
    tools:
      - "Git"
      - "VS Code"
      - "Postman"
      - "Jira"
      - "Slack"
      - "Notion"
```

These YAML examples demonstrate practical usage across various domains including application configuration, container orchestration, CI/CD pipelines, infrastructure management, API documentation, and complex data serialization. 