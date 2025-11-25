# 🔒 Lista Completa de Arquivos para Ignorar no Git

> Uma guia completa focada em **DevSecOps** e **Cibersegurança** para proteger seus repositórios

---

## 📋 Índice
- [Credenciais e Secrets](#credenciais-e-secrets)
- [Variáveis de Ambiente](#variáveis-de-ambiente)
- [Infrastructure as Code (IaC)](#infrastructure-as-code-iac)
- [Chaves e Certificados](#chaves-e-certificados)
- [Arquivos de Configuração Sensível](#arquivos-de-configuração-sensível)
- [Build e Dependências](#build-e-dependências)
- [Logs e Cache](#logs-e-cache)
- [IDE e Editor](#ide-e-editor)
- [Cloud Providers](#cloud-providers)
- [Banco de Dados](#banco-de-dados)
- [Containers e Orquestração](#containers-e-orquestração)
- [Padrões Regex Úteis](#padrões-regex-úteis)

---

## 🔐 Credenciais e Secrets

Esses arquivos frequentemente contêm informações sensíveis de autenticação:

```gitignore
# Arquivos de secrets
.env
.env.local
.env.*.local
.env.production.local
.env.development.local
.env.test.local
secrets.txt
credentials.json
credentials.xml
secrets.yaml
secrets.yml
secrets.env
.secrets/

# Arquivos de autenticação
.apikeys
.api_keys
.auth
auth.json
authentication.json
token.json
tokens.json
api-keys.json
```

**Por quê:** Variáveis de ambiente frequentemente armazenam API keys, senhas e tokens de acesso.

---

## 🌍 Variáveis de Ambiente

```gitignore
# Arquivos de configuração ambiental
.env
.env.local
.env.*.local
.env.production
.env.development
.env.staging
.env.test
.env.example (INCLUIR - serve de template)
env.local
env.*.local
```

**Boas Práticas:**
- ✅ Crie um `.env.example` com variáveis vazias ou com valores fake
- ✅ Documente quais variáveis são necessárias
- ❌ Nunca comite arquivos `.env` reais

---

## 📦 Infrastructure as Code (IaC)

### Terraform
```gitignore
# Terraform
*.tfvars
*.tfvars.json
!example.tfvars
.terraform/
.terraform.lock.hcl
terraform.tfstate
terraform.tfstate.*
terraform.tfvars.json
crash.log
override.tf
override.tf.json
*_override.tf
*_override.tf.json
.terraformrc
.terraform/
*.auto.tfvars
```

**Crítico:** `*.tfvars` contém valores de variáveis incluindo secrets, credenciais AWS, etc.

### Ansible
```gitignore
# Ansible
*.vault
vault.yml
vault.yaml
vault.json
.ansible/inventory
```

### CloudFormation
```gitignore
# AWS CloudFormation
packaged.yaml
packaged.yml
.aws-sam/
```

---

## 🔑 Chaves e Certificados

```gitignore
# Chaves SSH
id_rsa
id_rsa.pub
id_dsa
id_dsa.pub
id_ecdsa
id_ecdsa.pub
id_ed25519
id_ed25519.pub
*.pem
*.key
*.crt
*.cert
*.cer
*.p12
*.pfx
*.jks
*.keystore

# Certificados
*.crt
*.cer
*.p7b
*.pkcs7
*.spc

# Chaves PGP/GPG
*.gpg
*.asc
secring.gpg
pubring.gpg
```

**Aviso:** Chaves privadas nunca devem estar no repositório!

---

## ⚙️ Arquivos de Configuração Sensível

```gitignore
# Configurações sensíveis
config.local.yml
config.local.yaml
config.local.json
config.production.yml
config.production.yaml
config.production.json
settings.local.py
settings.production.py
local_settings.py
local.properties
local.config

# Database configs
database.yml
database.yaml
database.json
connections.json

# Mail configs
mailer.yml
mailer.yaml
```

---

## 🏗️ Build e Dependências

```gitignore
# Node.js / npm / yarn
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
.npm
.yarn/cache
.yarn/unplugged
.yarn/build-state.yml
.yarn/install-state.gz
package-lock.json (considere usar)
yarn.lock (considere usar)

# Python
venv/
env/
ENV/
.venv
*.egg-info/
dist/
build/
pip-log.txt
pip-delete-this-directory.txt

# Java
target/
*.class
*.jar
*.war
*.ear
*.zip
*.tar.gz

# Go
vendor/
*.o
*.a

# Ruby
Gemfile.lock
vendor/bundle
.bundle/

# .NET
bin/
obj/
*.dll
*.exe
packages/
```

---

## 📜 Logs e Cache

```gitignore
# Logs
*.log
logs/
log/
*.log.*
debug.log
npm-debug.log
yarn-debug.log

# Cache
*.cache
cache/
.cache/
.tmp/
tmp/
temp/
.DS_Store
Thumbs.db
```

---

## 🖥️ IDE e Editor

```gitignore
# VS Code
.vscode/
.vscode/settings.json
.vscode/launch.json
.vscode/extensions.json
*.code-workspace

# JetBrains (IntelliJ, WebStorm, etc)
.idea/
*.iml
*.iws
*.ipr
.DS_Store

# Sublime Text
*.sublime-project
*.sublime-workspace

# Vim
*.swp
*.swo
*~
.vim/
.netrwhist

# Emacs
*~
\#*\#
.\#*
*.elc

# Visual Studio
.vs/
*.user
*.sln.docstates
.vscode/

# Eclipse
.classpath
.project
.settings/
```

---

## ☁️ Cloud Providers

### AWS
```gitignore
# AWS
.aws/
~/.aws/credentials
~/.aws/config
aws_credentials
aws_credentials.json
.aws-credentials
```

### Google Cloud
```gitignore
# GCP
.gcp/
google-cloud-key.json
gcp-key.json
service-account-key.json
```

### Azure
```gitignore
# Azure
.azure/
azure-credentials.json
azure_credentials
```

### Kubernetes
```gitignore
# Kubernetes
kubeconfig
kubeconfig.yaml
kubeconfig.yml
.kube/
```

---

## 🗄️ Banco de Dados

```gitignore
# Databases
*.db
*.sqlite
*.sqlite3
*.sqlcipher
*.mdb
*.accdb
dump.sql
dump.rds
backup.sql

# Database credentials
db_credentials.json
database.secrets.yml
```

---

## 🐳 Containers e Orquestração

```gitignore
# Docker
.dockerignore
docker-compose.override.yml
docker-compose.local.yml
.docker/
registry/
secrets.docker

# Docker registries credentials
~/.docker/config.json
~/.docker/config.js

# Kubernetes
helm-values-*.yaml
helm-values-*.yml
kustomization-*.yaml
```

---

## 🔍 Padrões Regex Úteis

Para `.gitignore`, você pode usar padrões mais avançados:

```gitignore
# Ignorar qualquer arquivo que comece com "secret"
secret*
secrets*

# Ignorar arquivos com extensões sensíveis
*.key
*.secret
*.cred
*.credentials
*.pwd
*.password

# Ignorar em qualquer nível de diretório
**/node_modules/
**/vendor/
**/.env
**/*.tfvars
**/secrets/
**/credentials/

# Ignorar com exceções (! inverte a regra)
*.log
!important.log
```

---

## 📋 Modelo Completo .gitignore

Salve este conteúdo em um arquivo `.gitignore` na raiz do seu projeto:

```gitignore
# ===================================
# CREDENCIAIS E SECRETS
# ===================================
.env
.env.local
.env.*.local
.env.production.local
.env.development.local
secrets.txt
credentials.json
.secrets/
.auth
.apikeys

# ===================================
# INFRAESTRUTURA COMO CÓDIGO
# ===================================
*.tfvars
*.tfvars.json
.terraform/
.terraform.lock.hcl
terraform.tfstate
terraform.tfstate.*

# ===================================
# CHAVES E CERTIFICADOS
# ===================================
*.pem
*.key
*.crt
*.cer
id_rsa
id_dsa
id_ecdsa
id_ed25519
*.p12
*.pfx
*.jks

# ===================================
# DEPENDÊNCIAS E BUILD
# ===================================
node_modules/
venv/
env/
ENV/
target/
dist/
build/
vendor/
.bundle/

# ===================================
# LOGS E CACHE
# ===================================
*.log
logs/
cache/
.tmp/
.cache/

# ===================================
# IDE E EDITORS
# ===================================
.vscode/
.idea/
*.sublime-workspace
*.swp
.DS_Store

# ===================================
# CLOUD PROVIDERS
# ===================================
.aws/
.gcp/
.azure/
google-cloud-key.json
service-account-key.json
kubeconfig

# ===================================
# BANCO DE DADOS
# ===================================
*.db
*.sqlite
*.sqlite3
dump.sql

# ===================================
# DOCKER
# ===================================
docker-compose.override.yml
docker-compose.local.yml
~/.docker/config.json
```

---

## ⚡ Checklist de Segurança

- [ ] `.env` está no `.gitignore`
- [ ] `*.tfvars` está no `.gitignore`
- [ ] `.aws/` está no `.gitignore`
- [ ] `*.pem`, `*.key` estão no `.gitignore`
- [ ] `kubeconfig` está no `.gitignore`
- [ ] Nenhuma chave privada está commitada
- [ ] Nenhum token está no código fonte
- [ ] Credenciais de banco de dados não estão visíveis
- [ ] `.env.example` existe como template
- [ ] Documentação de secrets está atualizada

---

## 🚨 Se Você Já Comitou Secrets

Se você acidentalmente commitou informações sensíveis:

```bash
# 1. Remova o arquivo do histórico
git rm --cached nome-do-arquivo

# 2. Add ao .gitignore
echo "nome-do-arquivo" >> .gitignore

# 3. Commit as mudanças
git add .gitignore
git commit -m "Remove sensitive file"

# 4. Force push (CUIDADO! Use apenas em repos privados)
git push --force-with-lease

# 5. Revogue imediatamente qualquer credential exposta
# (tokens, chaves, senhas, etc)

# Para histórico completo, considere usar:
# - BFG Repo-Cleaner
# - git-filter-repo
```

---

## 📚 Referências

- [Git Documentation - .gitignore](https://git-scm.com/docs/gitignore)
- [GitHub - gitignore Templates](https://github.com/github/gitignore)
- [OWASP - Secrets Management](https://owasp.org/www-community/Sensitive_Data_Exposure)
- [Terraform Best Practices](https://www.terraform.io/docs/language/state/sensitive-data.html)
- [Git Secret - Tool para secrets](https://git-secret.io/)

---

**Última atualização:** Novembro de 2025

> 🔐 **Lembre-se:** Segurança de secrets no git não é negociável. Dedique tempo para configurar corretamente desde o início do projeto.
