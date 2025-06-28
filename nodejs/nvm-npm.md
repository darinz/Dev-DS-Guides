# Node.js and NPM Command Line Reference

A comprehensive guide for managing Node.js versions, packages, and development workflows using NVM, NPM, and related tools.


## Table of Contents

1. [Understanding Node.js and NPM](#understanding-nodejs-and-npm)
2. [Installing NVM](#installing-nvm)
3. [Using NVM](#using-nvm)
4. [Installing and Managing Node.js](#installing-and-managing-nodejs)
5. [Working with NPM](#working-with-npm)
6. [Advanced NPM Usage](#advanced-npm-usage)
7. [Using Yarn](#using-yarn)
8. [Using pnpm](#using-pnpm)
9. [Using npx](#using-npx)
10. [Package Management](#package-management)
11. [Security Best Practices](#security-best-practices)
12. [Troubleshooting](#troubleshooting)
13. [Deployment Considerations](#deployment-considerations)
14. [Best Practices](#best-practices)


## Understanding Node.js and NPM

### What is Node.js?

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine that allows you to run JavaScript on the server-side.

### What is NPM?

NPM (Node Package Manager) is the default package manager for Node.js, used for installing and managing packages and dependencies.

### What is NVM?

NVM (Node Version Manager) is a tool that allows you to install and manage multiple versions of Node.js on the same machine.

### Key Benefits

- **Version Management**: Switch between Node.js versions easily
- **Package Management**: Install and manage dependencies
- **Ecosystem**: Access to millions of packages
- **Development Tools**: Build, test, and deploy applications


## Installing NVM

### macOS Installation

**Using Homebrew:**
```bash
brew install nvm
```

**Using curl:**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

### Linux Installation

**Using curl:**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

**Using wget:**
```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

### Windows Installation

**Using nvm-windows:**
1. Download from [nvm-windows releases](https://github.com/coreybutler/nvm-windows/releases)
2. Run the installer
3. Restart your terminal

### Shell Configuration

Add to your shell profile (`.bashrc`, `.zshrc`, `.bash_profile`):

```bash
# NVM configuration
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

**For Homebrew installation:**
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$HOMEBREW_PREFIX/opt/nvm/nvm.sh" ] && \. "$HOMEBREW_PREFIX/opt/nvm/nvm.sh"
[ -s "$HOMEBREW_PREFIX/opt/nvm/etc/bash_completion.d/nvm" ] && \. "$HOMEBREW_PREFIX/opt/nvm/etc/bash_completion.d/nvm"
```

### Apply Changes

```bash
source ~/.zshrc   # or ~/.bashrc, ~/.bash_profile, etc.
```

### Verify Installation

```bash
nvm --version
```


## Using NVM

### Basic Commands

```bash
# Install Node.js versions
nvm install node        # Install latest version
nvm install 22          # Install specific version
nvm install --lts       # Install latest LTS version
nvm install 22.0.0      # Install exact version

# List versions
nvm ls                  # List installed versions
nvm ls-remote           # List all available versions
nvm ls-remote --lts     # List LTS versions only

# Switch versions
nvm use 22              # Use specific version
nvm use node            # Use latest version
nvm use --lts           # Use latest LTS version

# Set default version
nvm alias default 22    # Set default version
nvm alias default node  # Set latest as default
nvm alias default lts/* # Set latest LTS as default

# Show current version
nvm current             # Show current version
node --version          # Alternative way to check
```

### Advanced NVM Usage

```bash
# Install with specific options
nvm install 22 --latest-npm  # Install with latest npm
nvm install 22 --insecure    # Install without SSL verification

# Run commands with specific version
nvm exec 22 node app.js      # Run with specific version
nvm run 22 app.js            # Alternative syntax

# Manage aliases
nvm alias production 22      # Create alias
nvm use production           # Use alias
nvm alias default production # Set alias as default
nvm unalias production       # Remove alias

# Clean up
nvm uninstall 22             # Remove specific version
nvm cache clear              # Clear cache

# Examples:
nvm install 22.0.0
nvm use 22.0.0
nvm alias default 22.0.0
```

### Project-Specific Versions

```bash
# Create .nvmrc file
echo "22" > .nvmrc
echo "lts/*" > .nvmrc
echo "22.0.0" > .nvmrc

# Use .nvmrc file
nvm use                     # Use version from .nvmrc
nvm install                 # Install version from .nvmrc

# Examples:
cd myproject
echo "22" > .nvmrc
nvm use  # Automatically uses Node.js 22
```


## Installing and Managing Node.js

### Installation Options

```bash
# Install latest version
nvm install node

# Install LTS version
nvm install --lts

# Install specific version
nvm install 22.0.0

# Install with specific npm version
nvm install 22 --latest-npm

# Install multiple versions
nvm install 20
nvm install 22
nvm install --lts
```

### Version Management

```bash
# Check current version
node --version
npm --version

# List all installed versions
nvm ls

# Switch between versions
nvm use 20
nvm use 22
nvm use --lts

# Set default version
nvm alias default 22

# Examples:
nvm install 22
nvm use 22
node --version  # Should show v22.x.x
```

### Environment Setup

```bash
# Set up for development
nvm install --lts
nvm use --lts
nvm alias default lts/*

# Set up for specific project
cd myproject
echo "22" > .nvmrc
nvm install
nvm use

# Examples:
nvm install --lts
nvm use --lts
npm install -g yarn pnpm
```


## Working with NPM

### Basic Commands

```bash
# Initialize project
npm init                  # Interactive initialization
npm init -y               # Quick init with defaults
npm init --yes            # Alternative syntax

# Install packages
npm install               # Install all dependencies
npm install express       # Install specific package
npm install express@4.18.2 # Install specific version
npm install -g nodemon    # Install globally

# Remove packages
npm uninstall express     # Remove package
npm uninstall -g nodemon  # Remove global package

# Update packages
npm update                # Update all packages
npm update express        # Update specific package
npm update -g nodemon     # Update global package

# List packages
npm list                  # List installed packages
npm list -g               # List global packages
npm list --depth=0        # List top-level packages only
```

### Package Installation Options

```bash
# Install with specific flags
npm install --save express        # Save to dependencies (default)
npm install --save-dev jest       # Save to devDependencies
npm install --save-exact express  # Save exact version
npm install --save-optional express # Save to optionalDependencies

# Install with specific registry
npm install express --registry https://registry.npmjs.org/

# Install with specific tag
npm install express@latest
npm install express@stable

# Install with specific scope
npm install @types/node
npm install @babel/core

# Examples:
npm install express cors helmet
npm install --save-dev jest @types/jest
npm install --save-exact lodash@4.17.21
```

### Package.json Management

```bash
# View package.json
cat package.json
npm list --json

# Add scripts
npm pkg set scripts.start="node index.js"
npm pkg set scripts.test="jest"
npm pkg set scripts.build="webpack"

# Set package properties
npm pkg set name="my-app"
npm pkg set version="1.0.0"
npm pkg set description="My awesome app"

# Examples:
npm pkg set scripts.dev="nodemon index.js"
npm pkg set scripts.lint="eslint ."
```


## Advanced NPM Usage

### Scripts and Automation

```bash
# Run scripts
npm start                # Run start script
npm test                 # Run test script
npm run build            # Run build script
npm run dev              # Run dev script

# Run scripts with arguments
npm run test -- --watch
npm run build -- --mode production

# Pre and post scripts
npm run build            # Runs prebuild, build, postbuild
npm run test             # Runs pretest, test, posttest

# Examples:
npm run dev
npm run test -- --coverage
npm run build -- --analyze
```

### Workspaces (Monorepos)

```bash
# Initialize workspace
npm init -w packages/app
npm init -w packages/api

# Install in specific workspace
npm install express --workspace=packages/api
npm install react --workspace=packages/app

# Run scripts in workspace
npm run build --workspace=packages/app
npm run test --workspaces

# Examples:
npm install lodash --workspace=packages/shared
npm run dev --workspaces
```

### Publishing Packages

```bash
# Login to npm
npm login

# Publish package
npm publish

# Publish with specific tag
npm publish --tag beta

# Unpublish package (within 72 hours)
npm unpublish package-name

# Update package
npm version patch        # 1.0.0 -> 1.0.1
npm version minor        # 1.0.0 -> 1.1.0
npm version major        # 1.0.0 -> 2.0.0

# Examples:
npm login
npm version patch
npm publish
```

### Configuration

```bash
# Set npm configuration
npm config set registry https://registry.npmjs.org/
npm config set init-author-name "Your Name"
npm config set init-author-email "your.email@example.com"

# View configuration
npm config list
npm config get registry

# Edit configuration
npm config edit

# Examples:
npm config set save-exact true
npm config set package-lock true
```


## Using Yarn

### Installation

```bash
# Install Yarn
npm install -g yarn

# Verify installation
yarn --version
```

### Basic Commands

```bash
# Initialize project
yarn init
yarn init -y

# Install packages
yarn install              # Install all dependencies
yarn add express          # Add dependency
yarn add -D jest          # Add dev dependency
yarn add -g nodemon       # Add global package

# Remove packages
yarn remove express
yarn global remove nodemon

# Update packages
yarn upgrade
yarn upgrade express

# List packages
yarn list
yarn list --depth=0
```

### Advanced Yarn Usage

```bash
# Workspaces
yarn workspaces info
yarn workspace app add react
yarn workspaces run build

# Scripts
yarn start
yarn test
yarn build

# Examples:
yarn add express cors helmet
yarn add -D jest @types/jest
yarn workspace api add mongoose
```


## Using pnpm

### Installation

```bash
# Install pnpm
npm install -g pnpm

# Using curl
curl -fsSL https://get.pnpm.io/install.sh | sh -

# Verify installation
pnpm --version
```

### Basic Commands

```bash
# Initialize project
pnpm init

# Install packages
pnpm install              # Install all dependencies
pnpm add express          # Add dependency
pnpm add -D jest          # Add dev dependency
pnpm add -g nodemon       # Add global package

# Remove packages
pnpm remove express
pnpm remove -g nodemon

# Update packages
pnpm update
pnpm update express

# List packages
pnpm list
pnpm list --depth=0
```

### Advanced pnpm Usage

```bash
# Workspaces
pnpm -r run build
pnpm --filter app add react
pnpm --filter api add express

# Scripts
pnpm start
pnpm test
pnpm build

# Examples:
pnpm add express cors helmet
pnpm add -D jest @types/jest
pnpm --filter api add mongoose
```


## Using npx

### Basic Usage

```bash
# Run packages without installation
npx create-react-app my-app
npx cowsay "Hello World"
npx http-server

# Run specific version
npx express@4.18.2 --version

# Run with specific Node.js version
npx --node-version 16 create-react-app my-app

# Examples:
npx create-next-app@latest my-app
npx prettier --write .
npx eslint --fix .
```

### Advanced npx Usage

```bash
# Run local packages
npx jest
npx webpack

# Run with options
npx --yes create-react-app my-app
npx --no-install http-server

# Examples:
npx --yes create-react-app my-app --template typescript
npx --no-install serve -s build
```


## Package Management

### Dependency Types

```json
{
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  },
  "peerDependencies": {
    "react": "^18.0.0"
  },
  "optionalDependencies": {
    "fsevents": "^2.3.0"
  }
}
```

### Version Ranges

```bash
# Version specifiers
"package": "1.2.3"        # Exact version
"package": "^1.2.3"       # Compatible with 1.2.3 (>=1.2.3,<2.0.0)
"package": "~1.2.3"       # Patch updates (>=1.2.3,<1.3.0)
"package": ">=1.2.3"      # Minimum version
"package": "<=1.2.3"      # Maximum version
"package": "1.2.3 - 1.8.0" # Version range
"package": "*"            # Any version
"package": "latest"       # Latest version
```

### Lock Files

```bash
# package-lock.json (npm)
npm install              # Generates package-lock.json

# yarn.lock (Yarn)
yarn install             # Generates yarn.lock

# pnpm-lock.yaml (pnpm)
pnpm install             # Generates pnpm-lock.yaml

# Examples:
npm install              # Creates/updates package-lock.json
git add package-lock.json
```

### Dependency Resolution

```bash
# Check for conflicts
npm ls
npm ls express

# Check outdated packages
npm outdated
npm outdated --depth=0

# Update packages
npm update
npm update express

# Examples:
npm outdated
npm update --save
```


## Security Best Practices

### Security Auditing

```bash
# Check for vulnerabilities
npm audit
npm audit --audit-level moderate

# Fix vulnerabilities
npm audit fix
npm audit fix --force

# Generate security report
npm audit --json > audit-report.json

# Examples:
npm audit
npm audit fix
```

### Package Security

```bash
# Use trusted packages
npm install express      # Official package
npm install @types/node  # Official types

# Check package integrity
npm ci                   # Clean install
npm install --package-lock-only

# Use specific versions
npm install express@4.18.2

# Examples:
npm ci
npm audit fix
```

### Environment Security

```bash
# Use .npmrc for configuration
echo "registry=https://registry.npmjs.org/" > .npmrc
echo "save-exact=true" >> .npmrc

# Use environment variables
export NPM_CONFIG_REGISTRY=https://registry.npmjs.org/

# Examples:
echo "save-exact=true" > .npmrc
npm install express
```


## Troubleshooting

### Common Issues

```bash
# Permission errors
sudo chown -R $USER:$GROUP ~/.npm
sudo chown -R $USER:$GROUP ~/.nvm

# Clear cache
npm cache clean --force
yarn cache clean
pnpm store prune

# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Check npm configuration
npm config list
npm config get registry

# Examples:
rm -rf node_modules package-lock.json
npm install
```

### Version Conflicts

```bash
# Check Node.js version
node --version
nvm current

# Check npm version
npm --version

# Update npm
npm install -g npm@latest

# Use specific Node.js version
nvm use 22
nvm alias default 22

# Examples:
nvm use 22
npm install -g npm@latest
```

### Network Issues

```bash
# Use different registry
npm config set registry https://registry.npmjs.org/
npm config set registry https://registry.npm.taobao.org/

# Use proxy
npm config set proxy http://proxy.company.com:8080
npm config set https-proxy http://proxy.company.com:8080

# Examples:
npm config set registry https://registry.npmjs.org/
npm install express
```


## Deployment Considerations

### Production Setup

```bash
# Install production dependencies only
npm install --production
npm ci --only=production

# Set NODE_ENV
export NODE_ENV=production
NODE_ENV=production npm start

# Use specific Node.js version
nvm use 22
nvm alias default 22

# Examples:
NODE_ENV=production npm ci --only=production
```

### Docker Integration

```dockerfile
# Dockerfile example
FROM node:22-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Start application
CMD ["npm", "start"]
```

### CI/CD Integration

```yaml
# GitHub Actions example
name: Node.js CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Run security audit
      run: npm audit
```


## Best Practices

### Project Structure

```
myproject/
├── src/                 # Source code
├── tests/               # Test files
├── docs/                # Documentation
├── package.json         # Package configuration
├── package-lock.json    # Dependency lock file
├── .nvmrc              # Node.js version
├── .npmrc              # NPM configuration
├── .gitignore          # Git ignore file
└── README.md           # Project documentation
```

### Version Management

```bash
# Use .nvmrc for project-specific versions
echo "22" > .nvmrc

# Use LTS versions for production
nvm install --lts
nvm use --lts

# Pin exact versions for production
npm install --save-exact express

# Examples:
echo "22" > .nvmrc
nvm use
npm install --save-exact express@4.18.2
```

### Dependency Management

```bash
# Use package-lock.json
git add package-lock.json

# Regular updates
npm update
npm audit fix

# Use specific package managers consistently
# Choose npm, yarn, or pnpm and stick with it

# Examples:
npm update
npm audit fix
git add package-lock.json
```

### Security Practices

```bash
# Regular security audits
npm audit
npm audit fix

# Use trusted packages
npm install express
npm install @types/node

# Keep dependencies updated
npm update
npm outdated

# Examples:
npm audit
npm audit fix
npm update
```

### Development Workflow

```bash
# Use scripts in package.json
npm run dev
npm run test
npm run build

# Use pre-commit hooks
npm install --save-dev husky
npx husky install

# Use linting and formatting
npm install --save-dev eslint prettier
npm run lint
npm run format

# Examples:
npm run dev
npm run test
npm run build
```

---

**Note**: This guide covers the most commonly used Node.js and NPM commands and best practices. For more detailed information, refer to the official documentation of Node.js, NPM, and related tools.