# Git Ignore List - Complete Reference

> Comprehensive list of all file patterns to ignore in Git repositories for security

---

## 📚 Quick Navigation

- [Environment & Configuration](#-environment--configuration)
- [Keys & Certificates](#-keys--certificates)
- [Cloud Provider Credentials](#-cloud-provider-credentials)
- [Infrastructure as Code](#-infrastructure-as-code)
- [Databases](#-databases)
- [Logs & Temporary](#-logs--temporary)
- [IDE & Tools](#-ide--tools)
- [Node.js & JavaScript](#-nodejs--javascript)
- [Python](#-python)
- [Docker & Containers](#-docker--containers)
- [Kubernetes](#-kubernetes)
- [Build Artifacts](#-build-artifacts)

---

## 🔐 Environment & Configuration

### Pattern: Environment Files

```gitignore
# Local environment variables
.env
.env.local
.env.*.local
.env.production.local
.env.test.local
.env.development.local

# Environment files with extensions
.env.*
!.env.example

# Config files with secrets
config/*.local.js
config/*.local.yaml
config/*.local.yml
config/secrets.json
```

**Reason:** `.env` files contain database passwords, API keys, tokens. Use `.env.example` instead.

### Pattern: Local Override Files

```gitignore
# Local overrides
local.properties
local.gradle
.private

# User-specific
.user
```

**Reason:** Team members might have different configurations.

---

## 🔑 Keys & Certificates

### Pattern: SSH Keys

```gitignore
# SSH private keys
id_rsa
id_rsa.pub
id_dsa
id_dsa.pub
id_ecdsa
id_ecdsa.pub
id_ed25519
id_ed25519.pub

# SSH config
.ssh/*
!.ssh/*.pub
!.ssh/*.config

# Known hosts
known_hosts
```

**Reason:** SSH private keys give full server access.

**Risk:** Attacker can access all systems with these keys.

### Pattern: PEM & Key Files

```gitignore
# Certificate files
*.pem
*.key
*.crt
*.cert
*.cer
*.p7b
*.p7s

# Private keys
*.pfx
*.p12
*.pkcs12
*.jks
*.keystore

# Encrypted keys
*.encryptedkey
private*.pem
```

**Reason:** Certificate files contain encryption keys and credentials.

**Risk:** Infrastructure access, man-in-the-middle attacks.

### Pattern: GPG & Encryption Keys

```gitignore
# GPG keys
*.gpg
*.gnupg
.gnupg/

# Encryption files
*.encryptedkey
*.encrypted
```

---

## ☁️ Cloud Provider Credentials

### Pattern: AWS

```gitignore
# AWS credentials
.aws/
.aws/credentials
.aws/config
aws_access_key_id*
aws_secret_access_key*

# Temporary credentials
.aws-temp/

# AWS credentials in files
**/awscredentials.json
**/aws_credentials.json
**/aws/credentials

# AWS session tokens
.aws_session_token
```

**Reason:** AWS credentials = full account access.

**Risk:** $1000s in charges, data theft, ransomware.

### Pattern: GCP (Google Cloud)

```gitignore
# GCP credentials
.gcp/
google-cloud-key.json
**/service-account.json
**/service-account-key.json
**/gcp-key.json

# GCP tokens
.gcp_token
gc_token.json
```

**Reason:** GCP service accounts provide full project access.

**Risk:** Data exposure, compute resource theft.

### Pattern: Azure

```gitignore
# Azure credentials
.azure/
azure-credentials.json
**/azure-auth.json
**/azure-key.json

# Azure tokens
.azure_token
.azcli
```

**Reason:** Azure credentials = subscription access.

**Risk:** Cloud resource theft, data exposure.

### Pattern: Heroku

```gitignore
# Heroku
.heroku/
heroku_api_key.json

# Heroku local
.heroku_api_key
```

### Pattern: Firebase

```gitignore
# Firebase
firebase-credentials.json
**/serviceAccountKey.json
firebase-key.json
```

### Pattern: Other Cloud Services

```gitignore
# DigitalOcean
.digitalocean
digital_ocean_key.json

# Netlify
.netlify
netlify.toml.local

# Vercel
.vercel

# Auth0
auth0-credentials.json

# OAuth tokens
oauth_token.json
oauth_secret.json
```

---

## 🏗️ Infrastructure as Code

### Pattern: Terraform

```gitignore
# Terraform
*.tfvars
*.tfvars.json
.terraform/
.terraform.lock.hcl

# Terraform plans
*.tfplan
*.tfstate
*.tfstate.backup
*.tfstate.*.backup

# Override files
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Terraform variables
terraform.tfvars
terraform.tfvars.json
```

**Reason:** `.tfvars` files contain sensitive infrastructure configuration.

**Risk:** Full infrastructure compromise, cloud account exposure.

### Pattern: Ansible

```gitignore
# Ansible vault
*.vault
vault.yml
vault.yaml

# Ansible inventory
inventory.yml.local
hosts.local
group_vars/**/secrets.yml

# Ansible logs
*.ansible.log

# Encrypted files
*.encrypted
```

**Reason:** Ansible vault files contain encrypted credentials and secrets.

### Pattern: Kubernetes

```gitignore
# kubeconfig files
kubeconfig
kubeconfig.*
.kube/config
.kube/config.local

# Secret manifests (if storing locally)
**/secrets.yml
**/secrets.yaml
*secret*.yml
*secret*.yaml

# Kubernetes local dev
.k3d/
```

**Reason:** kubeconfig provides cluster access, secret manifests leak credentials.

### Pattern: Docker

```gitignore
# Docker credentials
.docker/config.json
.docker/config.local.json
docker-compose.override.yml

# Docker environment
.docker/.env
.docker/.env.*
```

**Reason:** Docker config.json contains registry credentials.

---

## 💾 Databases

### Pattern: Database Files

```gitignore
# SQLite
*.db
*.db3
*.sqlite
*.sqlite3

# Database dumps
*.sql
dump.sql
backup.sql
database.sql
database_dump.sql

# MongoDB
*.bson
dump/
```

**Reason:** Database files may contain customer data, PII.

**Risk:** GDPR fines, customer data exposure.

### Pattern: Database Backups

```gitignore
# Backup files
*.backup
*.bak
*.bkp
*backup*
dump/
backups/
```

### Pattern: Database Configuration

```gitignore
# Connection strings
database.yml
database.yaml
**/database.config.yml

# Root passwords
mysql_root_password.txt
db_password.txt
```

---

## 📝 Logs & Temporary

### Pattern: Log Files

```gitignore
# Log files
*.log
*.log.*
logs/
log/

# Specific logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
pip-log.txt
pip-delete-this-directory.txt

# Application logs
app.log
application.log
server.log
debug.log
error.log
warn.log

# Build logs
build.log
test.log
```

**Reason:** Logs may contain sensitive information (tokens, usernames, IPs).

### Pattern: Temporary Files

```gitignore
# Temporary
tmp/
temp/
*.tmp
*.temp
*.swp
*.swo
*~

# Backup files
*.bak
*.backup

# OS files
.DS_Store
Thumbs.db
```

---

## 🛠️ IDE & Tools

### Pattern: VS Code

```gitignore
# VS Code
.vscode/
.vscode/settings.json
.vscode/launch.json
.vscode/extensions.json
.vscode/*.code-workspace
```

### Pattern: JetBrains IDE

```gitignore
# JetBrains
.idea/
*.iml
*.iws
*.ipr
out/

# IntelliJ
.IntelliJIdea*/
.vscode/
```

### Pattern: Other IDEs

```gitignore
# Eclipse
.classpath
.project
.settings/

# Sublime
*.sublime-workspace
*.sublime-project

# Vim
.*.swp
.*.swo
*~

# Emacs
*~
\#*\#
.\#*
```

### Pattern: Build Tools

```gitignore
# Gradle
build/
.gradle/
gradle.properties.local

# Maven
target/
pom.xml.tag
pom.xml.releaseBackup
release.properties
dependency-reduced-pom.xml

# Cargo (Rust)
Cargo.lock
target/
```

---

## 📦 Node.js & JavaScript

### Pattern: npm

```gitignore
# Dependencies
node_modules/
package-lock.json

# npm cache
.npm

# Local .npm
.npmrc.local
npm-debug.log*

# Yarn
yarn.lock
.yarn-cache/
```

### Pattern: Development

```gitignore
# IDE
.vscode/
.idea/

# Environment
.env
.env.local
.env.*.local

# Logs
npm-debug.log*
yarn-debug.log*
yarn-error.log*
```

---

## 🐍 Python

### Pattern: Virtual Environments

```gitignore
# Virtual environments
venv/
env/
ENV/
.venv
.env
virtualenv/

# Pyenv
.python-version

# Poetry
poetry.lock
```

### Pattern: Python Artifacts

```gitignore
# Distribution
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/

# Python cache
*.py[cod]
__pycache__/
*.egg-info/
.Python

# Coverage
.coverage
.coverage.*
htmlcov/
.tox/
```

---

## 🐳 Docker & Containers

### Pattern: Docker

```gitignore
# Docker
.docker/config.json
docker-compose.override.yml
.dockerignore.local

# Container artifacts
.container-id
container.log
```

### Pattern: Container Registry

```gitignore
# Registry credentials
.registry
registry-credentials.json
docker-credentials.json
```

---

## 🎯 Kubernetes

### Pattern: Kubeconfig

```gitignore
# Kubernetes
.kube/config
kubeconfig
kubeconfig.*

# Kubernetes local
.k3d/
kind-config.yaml
```

### Pattern: Kubernetes Secrets

```gitignore
# Secret manifests
**/secrets.yml
**/secrets.yaml
**/*secret*.yml
**/*secret*.yaml

# Registry
pull-secret.json
image-pull-secret.yaml
```

---

## 🏗️ Build Artifacts

### Pattern: Build Output

```gitignore
# Build
build/
dist/
*.min.js
*.min.css

# Compiled
*.o
*.a
*.so
*.dll
*.dylib
```

### Pattern: Version Control

```gitignore
# CVS
CVS/
.cvsignore

# Subversion
.svn/

# Mercurial
.hg/
```

---

## 📋 Complete Production-Ready .gitignore

Here's a complete template combining all sections:

```gitignore
# ENVIRONMENT & CONFIG
.env
.env.local
.env.*.local
.env.*
!.env.example
config/*.local.js
config/*.local.yaml
local.properties

# KEYS & CERTIFICATES
id_rsa
id_rsa.pub
*.pem
*.key
*.crt
*.cert
*.p12
*.pfx

# AWS
.aws/
aws_access_key*
aws_secret_access_key*

# GCP
.gcp/
google-cloud-key.json
**/service-account*.json
**/gcp-key.json

# AZURE
.azure/
azure-credentials.json

# TERRAFORM
*.tfvars
*.tfvars.json
.terraform/
*.tfstate*
terraform.lock.hcl

# DATABASES
*.db
*.sqlite
*.sqlite3
*.sql
dump.sql
backup.sql

# KUBERNETES
kubeconfig
.kube/config
**/secrets.yml
**/secrets.yaml

# LOGS
*.log
logs/
*.log.*

# TEMPORARY
tmp/
temp/
*.tmp
.swp
*~

# IDE
.vscode/
.idea/
.DS_Store
Thumbs.db

# NODE
node_modules/
npm-debug.log*

# PYTHON
venv/
env/
__pycache__/
*.egg-info/

# DOCKER
.docker/config.json
docker-compose.override.yml

# GRADLE
.gradle/
build/

# MAVEN
target/

# BUILD
dist/
```

---

## 🔍 How to Use This List

### Quick: Use Production-Ready Template
Copy the "Complete Production-Ready .gitignore" section above.

### Comprehensive: Use by Category
Pick categories relevant to your project:
- Web app? Use Node.js + Python sections
- Infrastructure? Use Terraform + Kubernetes sections
- Microservices? Use Docker + Kubernetes sections

### Custom: Use Specific Patterns
If you have unique needs, use individual patterns above.

---

## ✅ Verification

After adding patterns, verify they work:

```bash
# Check if files are ignored
git status | grep .env    # Should be empty
git status | grep *.pem   # Should be empty
git status | grep *.tfvars # Should be empty

# Check if ignored files are listed
git ls-files -o --exclude-standard | head -20

# Never commit ignored files
git add .
git status --short | wc -l  # Count files to commit
```

---

## 🚀 Next Steps

1. Copy appropriate patterns for your project
2. Test with `git status`
3. Commit `.gitignore`
4. Use `git-secrets` for extra protection
5. Enable CI/CD secret scanning

---

**Remember:** When in doubt, ignore it. You can always un-ignore later, but once it's committed, it's in the history forever.

