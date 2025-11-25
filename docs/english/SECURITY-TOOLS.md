# Security Tools - Complete Setup Guide

> Comprehensive guide to implementing and using DevSecOps security tools

---

## 📚 Quick Navigation

- [Developer Setup](#-developer-setup-15-minutes)
- [DevOps Setup](#-devops-setup-2-hours)
- [Enterprise Setup](#-enterprise-setup-4-hours)
- [Detection Tools](#-detection-tools)
- [Prevention Tools](#-prevention-tools)
- [Management Tools](#-management-tools)
- [Troubleshooting](#-troubleshooting)

---

## 🚀 Developer Setup (15 minutes)

### Pre-commit Hooks Framework

The easiest way to prevent secrets from being committed.

**Install:**
```bash
pip install pre-commit
```

**Create `.pre-commit-config.yaml`:**
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
        args: ['--maxkb=1000']
  - repo: https://github.com/gitpython-developers/GitPython
    rev: 3.1.40
    hooks:
      - id: check-merge-conflict
      - id: check-case-conflict
```

**Install hooks:**
```bash
pre-commit install
```

**Test:**
```bash
# Run on all files
pre-commit run --all-files

# From now on, hooks run automatically before commit
git commit -m "Your message"  # Hooks run here
```

**Reset baseline (after cleaning secrets):**
```bash
detect-secrets scan > .secrets.baseline
```

---

### Git-Secrets

Prevents committing secrets at the Git hook level.

**Install:**
```bash
# macOS
brew install git-secrets

# Linux
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
make install

# Manual (any OS)
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
sudo make install  # or add to PATH
```

**Setup:**
```bash
# Install hooks globally
git secrets --install

# Install in specific repo
cd /your/project
git secrets --install

# Add AWS detection rules
git secrets --register-aws
```

**Scan existing repo:**
```bash
git secrets --scan-history
```

**Add custom pattern:**
```bash
git secrets --add 'AKIA[0-9A-Z]{16}'  # AWS access keys
```

---

## 🔍 Detection Tools

### TruffleHog (Historical Scanning)

Finds secrets already in git history.

**Install:**
```bash
pip install truffleHog
```

**Scan filesystem:**
```bash
truffleHog filesystem /path/to/project \
  --json \
  --entropy=False \
  --max-depth=10000 \
  > results.json
```

**Scan git repository:**
```bash
truffleHog git https://github.com/user/repo \
  --json \
  --since-commit HEAD~50 \
  > git-results.json
```

**Review results:**
```bash
# Pretty print
python3 -m json.tool results.json

# Count findings
cat results.json | grep "verified" | wc -l

# Filter high confidence
cat results.json | jq '.[] | select(.verified == true)'
```

**Remediate:**
See "Found a Secret" section in SECURITY-CHECKLIST.md

---

### Gitleaks (CI/CD Scanning)

Detects hardcoded secrets in code changes.

**Install:**
```bash
# macOS
brew install gitleaks

# Linux
wget https://github.com/gitleaks/gitleaks/releases/download/v12.0.0/gitleaks-linux-x64
chmod +x gitleaks-linux-x64
sudo mv gitleaks-linux-x64 /usr/local/bin/gitleaks

# Docker
docker run -v /path/to/project:/repo zricethezav/gitleaks:latest \
  detect --source /repo
```

**Scan:**
```bash
# Current directory
gitleaks detect --source .

# Verbose output
gitleaks detect --source . -v

# Generate report
gitleaks detect --source . --report-path gitleaks-report.json
```

**GitHub Actions (Recommended):**
```yaml
name: Secret Scanning
on: [push, pull_request]

jobs:
  gitleaks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: gitleaks/gitleaks-action@v2
        with:
          source: .
          verbose: true
          fail: true
```

---

### Detect-Secrets

Context-aware secret detection with low false positives.

**Install:**
```bash
pip install detect-secrets
```

**Create baseline:**
```bash
detect-secrets scan > .secrets.baseline
```

**Scan for new secrets:**
```bash
detect-secrets scan --all-files --baseline .secrets.baseline
```

**Audit findings:**
```bash
detect-secrets audit .secrets.baseline
```

**CI/CD Integration:**
```bash
# In your CI/CD pipeline
detect-secrets scan \
  --baseline .secrets.baseline \
  --fail-on-new-secrets \
  --exit-code-if-secrets-found 1
```

---

## 🛡️ Prevention Tools

### GitHub Secret Scanning

Built into GitHub, automatically scans for known secret patterns.

**Enable (Public Repos - Free):**
1. Go to repository Settings
2. Code security & analysis
3. Enable "Secret scanning"
4. Done - automatic scanning starts

**Enable (Private Repos - GitHub Advanced):**
1. Settings → Code security & analysis
2. Enable "Secret scanning"
3. Enable "Push protection" (prevents commits)
4. Configure secret scanning alerts

**Custom patterns (Enterprise):**
```bash
# In repository settings, add custom patterns
# Example: ^(my_custom_prefix)_[a-zA-Z0-9]{32}$
```

**API to check alerts:**
```bash
curl -H "Authorization: token $GITHUB_TOKEN" \
  https://api.github.com/repos/owner/repo/secret-scanning/alerts
```

---

### GitLab Secret Detection

Built into GitLab, scans for credentials.

**Enable:**
1. Project → Settings → Security & Compliance
2. Secret Detection
3. Enable "Pre-receive secret detection" (prevents push)

**Scan pipeline:**
```yaml
stages:
  - security

secret_detection:
  stage: security
  image: node:latest
  script:
    - npm install -g detect-secrets
    - detect-secrets scan --all-files
```

---

### Azure DevOps Secret Scanning

Integrated secret scanning for Azure pipelines.

**In Azure Pipeline:**
```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: CredScanTask@2
    inputs:
      scanFolder: '$(Build.SourcesDirectory)'
```

---

## 🔐 Management Tools

### HashiCorp Vault

Enterprise secret management and encryption.

**Install:**
```bash
# macOS
brew install vault

# Linux
wget https://releases.hashicorp.com/vault/1.15.0/vault_1.15.0_linux_amd64.zip
unzip vault_1.15.0_linux_amd64.zip
sudo mv vault /usr/local/bin/
```

**Start dev server:**
```bash
vault server -dev
```

**Basic usage:**
```bash
# Export token and address (from dev output)
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN='s.xxxxxxxxxxxxxx'

# Write secret
vault kv put secret/myapp/db \
  username="dbuser" \
  password="dbpass"

# Read secret
vault kv get secret/myapp/db

# Read as JSON
vault kv get -format=json secret/myapp/db
```

**Application integration:**
```python
import hvac

client = hvac.Client(url='http://127.0.0.1:8200')
secret = client.secrets.kv.read_secret_version(path='myapp/db')
password = secret['data']['data']['password']
```

---

### AWS Secrets Manager

AWS native secret management.

**Store secret:**
```bash
aws secretsmanager create-secret \
  --name prod/db/password \
  --secret-string "my-secure-password"
```

**Retrieve secret (CLI):**
```bash
aws secretsmanager get-secret-value \
  --secret-id prod/db/password \
  --query SecretString \
  --output text
```

**Retrieve secret (Python):**
```python
import boto3
import json

client = boto3.client('secretsmanager')
response = client.get_secret_value(SecretId='prod/db/password')
password = response['SecretString']
```

**CI/CD integration (GitHub Actions):**
```yaml
- name: Get secret from AWS
  uses: aws-actions/aws-secretsmanager-get-secrets@v1
  with:
    secret-ids: prod/db/password
    parse-json: true
```

---

### GCP Secret Manager

Google Cloud Platform secret management.

**Create secret:**
```bash
echo -n "my-secure-password" | gcloud secrets create prod-db-password \
  --data-file=-
```

**Access secret:**
```bash
gcloud secrets versions access latest --secret="prod-db-password"
```

**Python integration:**
```python
from google.cloud import secretmanager

client = secretmanager.SecretManagerServiceClient()
project_id = "my-project"
secret_id = "prod-db-password"
version_id = "latest"

name = f"projects/{project_id}/secrets/{secret_id}/versions/{version_id}"
response = client.access_secret_version(request={"name": name})
password = response.payload.data.decode("UTF-8")
```

---

### Azure Key Vault

Azure secret and key management.

**Store secret:**
```bash
az keyvault secret set \
  --vault-name myKeyVault \
  --name DbPassword \
  --value "my-secure-password"
```

**Retrieve secret:**
```bash
az keyvault secret show \
  --vault-name myKeyVault \
  --name DbPassword \
  --query value -o tsv
```

**Python integration:**
```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

credential = DefaultAzureCredential()
client = SecretClient(
  vault_url="https://mykeyvault.vault.azure.com/",
  credential=credential
)
password = client.get_secret("DbPassword").value
```

---

## 🔄 Rotation & Compliance

### Automatic Secret Rotation

**AWS Secrets Manager rotation:**
```bash
aws secretsmanager rotate-secret \
  --secret-id prod/db/password \
  --rotation-rules AutomaticallyAfterDays=30
```

**Vault rotation policy:**
```bash
vault write auth/approle/role/my-app \
  token_ttl=1h \
  token_max_ttl=4h
```

---

### Audit Logging

**AWS CloudTrail:**
```bash
aws cloudtrail create-trail \
  --name secrets-trail \
  --s3-bucket-name my-audit-bucket
```

**Vault audit logs:**
```bash
vault audit enable file file_path=/vault/logs/audit.log
```

---

## 🧪 Testing & Verification

### Test if tool blocks secrets

**Test TruffleHog with pre-commit:**
```bash
# Add fake secret
echo "aws_secret_access_key = AKIAIOSFODNN7EXAMPLE" > test.txt

# Try to commit
git add test.txt
git commit -m "test"  # Should fail

# Remove
rm test.txt
```

**Test GitHub secret scanning:**
```bash
# Create valid-looking (but fake) token
echo "ghp_1234567890abcdefghijklmnopqrstuvwxyz" > token.txt
git add token.txt
git commit -m "test"
git push  # GitHub should block/alert
```

---

## 🚨 Troubleshooting

### Pre-commit hook fails on all commits

**Problem:** All commits fail, even without secrets
```bash
git commit -m "test"
# Error: pre-commit hook failed
```

**Solution:**
```bash
# Update pre-commit
pip install --upgrade pre-commit

# Reinstall hooks
pre-commit uninstall
pre-commit install

# Run to see actual error
pre-commit run --all-files
```

---

### "git secrets not found"

**Solution:**
```bash
# macOS
brew install git-secrets

# Linux
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets
make install

# Verify
which git-secrets
git secrets --version
```

---

### "Vault connection refused"

**Problem:** Can't connect to Vault
```bash
vault status
# Error: connection refused
```

**Solution:**
```bash
# Start Vault (in another terminal)
vault server -dev

# Export address
export VAULT_ADDR='http://127.0.0.1:8200'

# Try again
vault status  # Should work now
```

---

### False positives in detect-secrets

**Problem:** Tool flags non-secret as secret
```bash
detect-secrets scan --all-files
# Flags: password = "example_password_123"
```

**Solution:**
```bash
# Audit and verify it's not a secret
detect-secrets audit .secrets.baseline

# Mark as safe (no credentials)
# The tool will remember and not flag again
```

---

## 🎯 Implementation Timeline

### Week 1: Foundation
- [ ] Install pre-commit hooks
- [ ] Create `.pre-commit-config.yaml`
- [ ] Install git-secrets
- [ ] Enable GitHub secret scanning

### Week 2: Automation
- [ ] Setup CI/CD scanning (Gitleaks)
- [ ] Configure GitHub Actions workflow
- [ ] Audit existing repository
- [ ] Document findings

### Week 3: Management
- [ ] Setup secret manager (Vault/AWS/GCP/Azure)
- [ ] Migrate hardcoded secrets
- [ ] Setup rotation policy
- [ ] Configure audit logging

### Week 4: Advanced
- [ ] Setup monitoring and alerting
- [ ] Create incident response procedure
- [ ] Train team on tools
- [ ] Schedule regular audits

---

## 📊 Recommended Stack

| Tool | Install Time | Learning Curve | Cost | Recommended |
|------|--------------|-----------------|------|------------|
| pre-commit | 5 min | Easy | Free | ✅ Essential |
| git-secrets | 5 min | Easy | Free | ✅ Essential |
| GitHub Secret Scanning | 1 min | None | Free | ✅ Essential |
| Gitleaks | 10 min | Easy | Free | ✅ CI/CD |
| Vault | 30 min | Medium | Free | 🟡 Recommended |
| AWS Secrets Manager | 15 min | Medium | $/month | 🟡 AWS users |
| Detect-secrets | 10 min | Easy | Free | 🟡 Advanced |

**Minimum viable setup:** pre-commit + git-secrets + GitHub scanning (20 minutes, free)

**Complete setup:** Above + Gitleaks + Vault (2 hours, mostly free)

---

**Next:** See [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) for step-by-step implementation.

