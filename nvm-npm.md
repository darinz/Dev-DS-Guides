# NVM and NPM Command Line Reference

A practical guide for managing Node.js versions and packages using NVM (Node Version Manager) and NPM (Node Package Manager), with instructions for macOS and Linux.


## Table of Contents

1. [Installing NVM (macOS & Linux)](#installing-nvm-macos--linux)
2. [Using NVM](#using-nvm)
3. [Installing and Managing Node.js](#installing-and-managing-nodejs)
4. [Working with NPM](#working-with-npm)
5. [Managing Dependencies](#managing-dependencies)
6. [Updating NPM](#updating-npm)
7. [NVM vs NPM](#nvm-vs-npm)
8. [Best Practices](#best-practices)


## Installing NVM (macOS & Linux)

### macOS (via Homebrew)

```bash
brew install nvm
```

Then add the following to your shell profile (e.g., `~/.zshrc`, `~/.bashrc`, or `~/.bash_profile`):

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$HOMEBREW_PREFIX/opt/nvm/nvm.sh" ] && \. "$HOMEBREW_PREFIX/opt/nvm/nvm.sh"
[ -s "$HOMEBREW_PREFIX/opt/nvm/etc/bash_completion.d/nvm" ] && \. "$HOMEBREW_PREFIX/opt/nvm/etc/bash_completion.d/nvm"
```

### Linux (via curl or wget)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
# OR
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

Then add the following to your shell profile:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
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

```bash
nvm install <version>    # Install a specific Node.js version
# Example: nvm install 22

nvm use <version>        # Use a specific Node.js version
# Example: nvm use 22

nvm ls                   # List installed versions
nvm ls-remote            # List all available versions

nvm alias default <ver>  # Set default version
# Example: nvm alias default 22
```


## Installing and Managing Node.js

```bash
nvm install node         # Install the latest version
nvm install --lts        # Install the latest LTS version

node -v                  # Check active Node.js version
```


## Working with NPM

```bash
npm init                 # Create package.json interactively
npm init -y              # Quick init with defaults

npm install <pkg>        # Install local package
# Example: npm install express

npm install -g <pkg>     # Install global package
# Example: npm install -g nodemon

npm uninstall <pkg>      # Remove local package
npm update               # Update local packages
```


## Managing Dependencies

```bash
npm list                 # Show installed packages
npm outdated             # Show outdated packages

npm audit                # Check for security issues
npm audit fix            # Fix known vulnerabilities
```


## Updating NPM

### Via NPM itself (macOS & Linux)

```bash
npm install -g npm@<version>
# Example: npm install -g npm@11

npm -v                   # Confirm update
```

### Using NVM

```bash
nvm install <node_version>   # Includes matching npm version
nvm use <node_version>
```


## NVM vs NPM

| Tool    | Purpose                               |
| ------- | ------------------------------------- |
| **NVM** | Manages Node.js versions              |
| **NPM** | Manages project packages/dependencies |

Use **NVM** when:

* Switching Node.js versions for different projects
* Managing project compatibility

Use **NPM** when:

* Installing packages like `express`, `lodash`, etc.
* Managing `package.json` dependencies


## Best Practices

* Use `.nvmrc` to specify project Node version:

  ```bash
  echo "22" > .nvmrc
  ```

* Use `package-lock.json` to lock package versions.

* Understand SemVer:

  * `^1.2.3`: allow minor and patch updates
  * `~1.2.3`: allow only patch updates

* Run `npm audit` regularly to stay secure.