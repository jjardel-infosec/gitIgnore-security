# 🛠️ Ferramentas de Segurança para Git e DevSecOps

> Guia completo de ferramentas essenciais para proteger seus repositórios e secrets

---

## 📋 Índice

1. [Detecção de Secrets](#detecção-de-secrets)
2. [Gerenciamento de Secrets](#gerenciamento-de-secrets)
3. [Git Hooks](#git-hooks)
4. [Análise de Código](#análise-de-código)
5. [Scanning de Dependências](#scanning-de-dependências)
6. [Encriptação](#encriptação)
7. [CI/CD Security](#cicd-security)
8. [Cloud Security](#cloud-security)

---

## 🔍 Detecção de Secrets

### 1. **TruffleHog**
Escaneia repositórios em busca de secrets expostos.

**Instalação:**
```bash
# Via pip
pip install truffleHog

# Via brew (macOS)
brew install truffleHog

# Via docker
docker run -it ghcr.io/trufflesecurity/trufflescan:latest
```

**Uso:**
```bash
# Escanear repositório local
truffleHog filesystem /path/to/repo

# Escanear repositório Git
truffleHog git https://github.com/seu-repo.git

# Escanear GitHub organization
truffleHog github --org seu-org

# Escanear apenas últimos commits
truffleHog git https://github.com/seu-repo.git --since-commit abc123
```

**Documentação:** https://github.com/trufflesecurity/truffleHog

---

### 2. **Detect Secrets**
Detecta e gerencia secrets em repositórios.

**Instalação:**
```bash
pip install detect-secrets
```

**Uso:**
```bash
# Criar baseline
detect-secrets scan > .secrets.baseline

# Auditar baseline
detect-secrets audit .secrets.baseline

# Usar com pre-commit
# Veja seção Pre-commit Hooks
```

**Documentação:** https://github.com/Yelp/detect-secrets

---

### 3. **Git-Secrets**
Previne commits de secrets.

**Instalação:**
```bash
# macOS
brew install git-secrets

# Linux
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
make install
```

**Uso:**
```bash
# Instalar hooks globalmente
git secrets --install ~/.git-secrets/hooks

# Instalar em repositório
git secrets --install

# Adicionar padrão para AWS keys
git secrets --register-aws

# Adicionar padrão customizado
git secrets --add 'password\s*[=:]\s*'

# Verificar repositório
git secrets --scan

# Verificar staged changes
git secrets --scan --cached
```

**Documentação:** https://github.com/awslabs/git-secrets

---

### 4. **Gitleaks**
Detector SAST para secrets.

**Instalação:**
```bash
# Via brew
brew install gitleaks

# Via docker
docker run zricethezav/gitleaks:latest
```

**Uso:**
```bash
# Escanear repositório local
gitleaks detect --source /path/to/repo

# Escanear com verbose
gitleaks detect --verbose

# Gerar relatório JSON
gitleaks detect --report-path report.json

# Verificar PR (em CI/CD)
gitleaks detect --exit-code
```

**Documentação:** https://github.com/zricethezav/gitleaks

---

## 🔐 Gerenciamento de Secrets

### 1. **HashiCorp Vault**
Solução enterprise de gerenciamento de secrets.

**Instalação:**
```bash
# macOS
brew install vault

# Linux
sudo apt-get install vault

# Docker
docker run -it vault
```

**Uso Básico:**
```bash
# Iniciar server dev
vault server -dev

# Logar
vault login s.token123

# Escrever secret
vault kv put secret/app/db password=mysecret

# Ler secret
vault kv get secret/app/db

# Listar secrets
vault kv list secret/app/
```

**Documentação:** https://www.vaultproject.io

---

### 2. **AWS Secrets Manager**
Gerenciador de secrets nativo AWS.

**CLI:**
```bash
# Criar secret
aws secretsmanager create-secret \
  --name prod/db/password \
  --secret-string "my-password"

# Recuperar secret
aws secretsmanager get-secret-value \
  --secret-id prod/db/password

# Rotacionar secret
aws secretsmanager rotate-secret \
  --secret-id prod/db/password
```

**Rotação Automática:**
```json
{
  "AutomaticallyAfterDays": 30,
  "Duration": "3h",
  "ScheduleExpression": "rate(30 days)"
}
```

**Documentação:** https://aws.amazon.com/secrets-manager

---

### 3. **Google Secret Manager**
Gerenciador de secrets do Google Cloud.

**CLI:**
```bash
# Criar secret
echo -n "my-password" | gcloud secrets create db-password --data-file=-

# Acessar secret
gcloud secrets versions access latest --secret="db-password"

# Listar versions
gcloud secrets versions list db-password
```

**Documentação:** https://cloud.google.com/secret-manager

---

### 4. **SOPS (Secrets Operations)**
Encrypta arquivos de configuração.

**Instalação:**
```bash
# macOS
brew install sops

# Linux
sudo apt-get install sops

# Docker
docker run -it mozilla/sops
```

**Uso:**
```bash
# Criar arquivo encriptado
sops secrets.yaml

# Editar arquivo encriptado
sops edit secrets.yaml

# Descriptografar para stdout
sops -d secrets.yaml

# Usar com AWS KMS
SOPS_KMS_ARN=arn:aws:kms:... sops edit secrets.yaml
```

**Documentação:** https://github.com/mozilla/sops

---

### 5. **Git-Crypt**
Encriptação transparente para Git.

**Instalação:**
```bash
# macOS
brew install git-crypt

# Linux
sudo apt-get install git-crypt
```

**Uso:**
```bash
# Inicializar git-crypt
git-crypt init

# Gerar chave
git-crypt export-key /tmp/git-crypt-key

# Configurar .gitattributes
echo "secrets/** filter=git-crypt diff=git-crypt" > .gitattributes

# Unlock repository
git-crypt unlock /tmp/git-crypt-key

# Share key com colega
git-crypt add-gpg-user [GPG-USER-ID]
```

**Documentação:** https://github.com/AGWA/git-crypt

---

## 🪝 Git Hooks

### 1. **Pre-commit Framework**
Framework para gerenciar git hooks.

**Instalação:**
```bash
pip install pre-commit
```

**Configuração (.pre-commit-config.yaml):**
```yaml
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']

  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: detect-private-key
      - id: check-added-large-files
      - id: check-json
      - id: check-yaml

  - repo: https://github.com/zricethezav/gitleaks
    rev: v8.18.0
    hooks:
      - id: gitleaks
```

**Instalação e Uso:**
```bash
# Instalar hooks
pre-commit install

# Rodar em todos os arquivos
pre-commit run --all-files

# Atualizar hooks
pre-commit autoupdate
```

**Documentação:** https://pre-commit.com

---

## 📊 Análise de Código

### 1. **SonarQube**
Análise de qualidade e segurança de código.

**Instalação:**
```bash
# Docker
docker run -d --name sonarqube -p 9000:9000 sonarqube:latest

# Brew
brew install sonarqube
```

**Uso:**
```bash
# Escanear projeto
sonar-scanner \
  -Dsonar.projectKey=my-app \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-token
```

**Documentação:** https://www.sonarqube.org

---

### 2. **Semgrep**
Análise SAST com regras customizáveis.

**Instalação:**
```bash
pip install semgrep

# Via brew
brew install semgrep
```

**Uso:**
```bash
# Escanear repositório
semgrep --config=p/security-audit .

# Escanear com regra específica
semgrep -r "password" .

# Gerar JSON report
semgrep -o report.json --json .
```

**Documentação:** https://semgrep.dev

---

### 3. **CodeQL (GitHub)**
Análise de código com GitHub Advanced Security.

**Uso em GitHub:**
1. Ir para **Settings → Code security & analysis**
2. Ativar **GitHub Advanced Security**
3. Ativar **CodeQL** 

**CI/CD (GitHub Actions):**
```yaml
name: CodeQL

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  analyze:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: github/codeql-action/init@v2
      - uses: github/codeql-action/autobuild@v2
      - uses: github/codeql-action/analyze@v2
```

**Documentação:** https://codeql.github.com

---

## 🔗 Scanning de Dependências

### 1. **Dependabot (GitHub)**
Verifica vulnerabilidades em dependências.

**Ativação:**
```
Settings → Code security & analysis → Enable Dependabot
```

**Configuração (.github/dependabot.yml):**
```yaml
version: 2
updates:
  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: daily
    open-pull-requests-limit: 5

  - package-ecosystem: pip
    directory: "/"
    schedule:
      interval: daily

  - package-ecosystem: docker
    directory: "/"
    schedule:
      interval: weekly
```

---

### 2. **Snyk**
Scanning de vulnerabilidades e license compliance.

**Instalação:**
```bash
npm install -g snyk
# ou
brew install snyk
```

**Uso:**
```bash
# Logar
snyk auth

# Testar vulnerabilidades
snyk test

# Gerar report
snyk test --json > report.json

# Monitor projeto
snyk monitor

# Test Docker image
snyk container test my-image:latest
```

**Documentação:** https://snyk.io

---

### 3. **OWASP Dependency-Check**
Identifica vulnerabilidades conhecidas.

**Instalação:**
```bash
# Docker
docker run -v $(pwd):/src owasp/dependency-check

# Manual
wget https://github.com/jeremylong/DependencyCheck_...-release.zip
```

**Uso:**
```bash
dependency-check.sh --project "MyApp" --scan /path/to/app
```

**Documentação:** https://owasp.org/www-project-dependency-check

---

## 🔒 Encriptação

### 1. **OpenSSL**
Criptografia padrão de linha de comando.

**Uso:**
```bash
# Gerar chave privada
openssl genrsa -out private.key 2048

# Gerar certificado público
openssl req -new -x509 -key private.key -out certificate.crt

# Encriptar arquivo
openssl enc -aes-256-cbc -in secrets.txt -out secrets.txt.enc

# Descriptografar
openssl enc -aes-256-cbc -d -in secrets.txt.enc -out secrets.txt
```

---

### 2. **GPG (GNU Privacy Guard)**
Encriptação e assinatura digital.

**Instalação:**
```bash
# macOS
brew install gpg

# Linux
sudo apt-get install gnupg
```

**Uso:**
```bash
# Gerar par de chaves
gpg --full-generate-key

# Encriptar arquivo
gpg --encrypt --recipient "user@email.com" secrets.txt

# Descriptografar
gpg --decrypt secrets.txt.gpg

# Assinar commit
git commit -S -m "Signed commit"
```

---

## 🔄 CI/CD Security

### 1. **GitHub Actions - Secret Scanning**
Detecta secrets em GitHub Actions.

**.github/workflows/secrets-scan.yml:**
```yaml
name: Secret Scanning

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  secret-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: TruffleHog Scan
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          base: ${{ github.event.repository.default_branch }}
          head: HEAD
```

---

### 2. **GitLab CI - Security Scanning**
**.gitlab-ci.yml:**
```yaml
include:
  - template: Security/Secret-Detection.gitlab-ci.yml
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml

stages:
  - security

secret_detection:
  variables:
    SECURE_LOG_LEVEL: info
```

---

### 3. **Jenkins - Security Plugins**
Plugins de segurança para Jenkins.

```groovy
pipeline {
    agent any
    
    stages {
        stage('Secret Scanning') {
            steps {
                sh 'trufflehog filesystem . --json > report.json'
                sh 'gitleaks detect --exit-code'
            }
        }
        
        stage('Dependency Check') {
            steps {
                sh 'dependency-check.sh --project "MyApp" --scan .'
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'report.json'
        }
    }
}
```

---

## ☁️ Cloud Security

### 1. **AWS Security Tools**

**AWS Inspector:**
```bash
aws inspector start-assessment-run \
  --assessment-template-arn arn:aws:inspector:...
```

**AWS Config:**
```bash
# Checar secrets em Config
aws configservice describe-config-rules
```

---

### 2. **Google Cloud Security Command Center**

```bash
# Criar custom finding
gcloud scc findings create --description="Exposed secret" ...

# Listar findings
gcloud scc findings list --organization=ORG_ID
```

---

### 3. **Azure Security Center**

```bash
# Ativar alertas
az security auto-provisioning-setting update \
  --auto-provision On
```

---

## 📊 Comparação de Ferramentas

| Ferramenta | Tipo | Linguagens | Custo | Melhor Para |
|-----------|------|-----------|-------|-----------|
| **TruffleHog** | Secret Scanner | Todos | Gratuito | Detecção rápida |
| **Gitleaks** | Secret Scanner | Todos | Gratuito | CI/CD |
| **Git-Secrets** | Pre-commit | Todos | Gratuito | Prevenção local |
| **Vault** | Secret Mgmt | Todos | Enterprise | Escalabilidade |
| **SOPS** | Encryption | Todos | Gratuito | GitOps |
| **SonarQube** | Code Analysis | Java, Python, JS | Enterprise | Qualidade código |
| **Semgrep** | SAST | Multi | Gratuito | Regras customizadas |
| **Snyk** | Dependency | Multi | SaaS | Vulnerabilidades |
| **Dependabot** | Dependency | Multi | Gratuito (GitHub) | Automação |

---

## 🚀 Stack Recomendado Completo

### Pequeno Projeto (Gratuito)
```
├── Git-Secrets (pre-commit)
├── GitHub Secret Scanning
├── Dependabot
└── Basic .gitignore
```

### Projeto Médio (Escalável)
```
├── Pre-commit framework
├── Gitleaks (CI/CD)
├── SOPS (secrets)
├── SonarQube (code analysis)
├── Snyk (dependencies)
└── GitHub Advanced Security
```

### Projeto Enterprise (Completo)
```
├── HashiCorp Vault
├── TruffleHog + Gitleaks
├── Pre-commit + Detect-Secrets
├── SonarQube Enterprise
├── Snyk Enterprise
├── AWS Secrets Manager
├── GitHub/GitLab Enterprise
└── SIEM Integration
```

---

## 📚 Recursos Complementares

- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Cloud Security Alliance](https://cloudsecurityalliance.org/)

---

**Última atualização:** Novembro de 2025

> 🔒 Ferramentas boas fortalecem sua postura de segurança, mas educação é fundamental.
