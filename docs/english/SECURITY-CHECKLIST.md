# Security Checklist - Comprehensive Audits

> Step-by-step checklists for different scenarios and requirements

---

## ⚡ Pre-Commit Checklist (Before every push)

Use this EVERY time before pushing to any repository.

### Before Staging (5 minutes)

- [ ] Check what files you're about to commit
```bash
git status
```

- [ ] Review all changes
```bash
git diff
```

- [ ] Look for any credentials/secrets manually
```bash
git diff | grep -i "password\|api\|key\|secret\|token"
```

- [ ] Check environment variables
```bash
git diff | grep "\.env\|DATABASE_URL\|AWS_"
```

### Files to NEVER commit

- [ ] `.env` file (use `.env.example` instead)
- [ ] `*.pem`, `*.key`, `*.crt` files
- [ ] `.aws/`, `.gcp/`, `.azure/` directories
- [ ] `kubeconfig` files
- [ ] `id_rsa`, `id_rsa.pub` files
- [ ] `*.tfvars` files
- [ ] `*.p12`, `*.pfx` certificate files
- [ ] `.docker/config.json`
- [ ] Any files listed in `.gitignore`

### Final Check Before Push

```bash
# Stage changes
git add .

# Run pre-commit hooks
pre-commit run --all-files

# If it fails, fix the issues and stage again

# Check one more time
git diff --cached | head -100  # Show first 100 lines

# Only if no secrets found
git commit -m "Your message"
git push
```

---

## ✅ Initial Setup Checklist (First time)

Complete this once for each repository.

### 1. Repository Configuration (10 min)

- [ ] Clone repository
```bash
git clone <your-repo>
cd <your-repo>
```

- [ ] Copy `.gitignore` to project
```bash
# If using templates from this project
cp /path/to/.gitignore .
```

- [ ] Copy `.env.example` if needed
```bash
cp /path/to/.env.example .
```

- [ ] Create local `.env` file
```bash
cp .env.example .env
# Edit with real values
nano .env
```

- [ ] Verify `.env` is not in git
```bash
git status | grep .env  # Should NOT appear
```

### 2. Pre-commit Hooks Setup (10 min)

- [ ] Install pre-commit framework
```bash
pip install pre-commit
```

- [ ] Create `.pre-commit-config.yaml`
```bash
# Copy from SECURITY-TOOLS.md or create:
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/Yelp/detect-secrets
    rev: v1.4.0
    hooks:
      - id: detect-secrets
        args: ['--baseline', '.secrets.baseline']
EOF
```

- [ ] Install hooks
```bash
pre-commit install
```

- [ ] Run on all files
```bash
pre-commit run --all-files
```

- [ ] Create baseline (if using detect-secrets)
```bash
detect-secrets scan > .secrets.baseline
```

### 3. GitHub/GitLab Configuration (5 min)

**If using GitHub:**
- [ ] Go to Settings → Code security & analysis
- [ ] Enable "Secret scanning"
- [ ] Enable "Dependabot alerts"
- [ ] Enable "Code scanning" (if private)

**If using GitLab:**
- [ ] Go to Settings → Integrations
- [ ] Enable "Secret Detection"
- [ ] Enable "Dependency Scanning"

### 4. Team Notification (5 min)

- [ ] Share `.gitignore` with team
```bash
git add .gitignore .env.example
git commit -m "Add security: .gitignore and .env.example"
git push
```

- [ ] Notify team to pull and install hooks
```bash
# Message to team:
# "Please run: git pull && pip install pre-commit && pre-commit install"
```

- [ ] Share documentation
- [ ] Schedule security training

---

## 🔍 Full Repository Audit (30-60 min)

Run this on an existing repository to find issues.

### Phase 1: Manual Review (15 min)

```bash
# 1. Search for secrets in history
git log -p | grep -i "password\|api_key\|secret\|token" | head -20

# 2. Check for common secret files
git ls-files | grep -E "\.env|\.pem|\.key|\.tfvars|kubeconfig|\.aws"

# 3. Check for .env files
git log --all --full-history -- ".env"

# 4. Check for private keys
git log --all --full-history -- "id_rsa*"
```

### Phase 2: Automated Tools (20 min)

**Install and run TruffleHog:**
```bash
pip install truffleHog

# Scan filesystem
truffleHog filesystem . --json > truffleHog-results.json

# Check Git history
truffleHog git https://github.com/user/repo --json > truffleHog-git.json

# Review results
cat truffleHog-results.json | grep -i "found"
```

**Install and run Gitleaks:**
```bash
# macOS
brew install gitleaks

# Linux / Manual
wget https://github.com/gitleaks/gitleaks/releases/download/v12.0.0/gitleaks-linux-x64
chmod +x gitleaks-linux-x64

# Scan
./gitleaks-linux-x64 detect --source . -v

# Save report
./gitleaks-linux-x64 detect --source . -r gitleaks-report.json
```

**Using detect-secrets:**
```bash
pip install detect-secrets

# Scan
detect-secrets scan --all-files > results.baseline

# Audit results
detect-secrets audit results.baseline
```

### Phase 3: Review & Remediate (20-40 min)

**For each finding:**

- [ ] Note the file and line number
- [ ] Determine if real or false positive
- [ ] If real secret:
  - [ ] **REVOKE IMMEDIATELY** - regenerate/rotate the secret
  - [ ] Notify team
  - [ ] Remove from history (see section below)
  - [ ] Force push changes
- [ ] If false positive:
  - [ ] Add to baseline or ignore rules

---

## 🔐 Found a Secret? Emergency Response

**EXECUTE THIS IMMEDIATELY:**

### Step 1: REVOKE (5 min - URGENT)

```bash
# 1. Change the password/key right now
# 2. Delete the secret from your account
# 3. Notify your team immediately

# Examples:
# - AWS: Deactivate access key immediately
# - GitHub: Regenerate token
# - Database: Change password
# - API: Revoke old key, create new one
```

### Step 2: Clean History (30 min)

```bash
# Install git-filter-repo
pip install git-filter-repo

# Option A: Remove a specific file (e.g., .env)
git filter-repo --invert-paths --path .env

# Option B: Remove a directory
git filter-repo --invert-paths --path .aws

# Option C: Remove specific content
git filter-repo --path-glob 'SECRET=*' --invert-paths

# This rewrites ALL history - be careful!
```

### Step 3: Force Push (2 min - CAREFUL)

```bash
# This rewrites history - notify your team first!

# Option 1: Force push to specific branch
git push origin <branch> --force-with-lease

# Option 2: Force push all branches
git push --all --force-with-lease

# Option 3: If you can't force push (protected main):
# 1. Contact repository admin
# 2. Temporarily disable branch protection
# 3. Force push
# 4. Re-enable protection
```

### Step 4: Notify Team

```bash
# Tell your team to:
git pull --rebase origin <branch>

# Or if all branches were force-pushed:
git fetch origin
git checkout main
git reset --hard origin/main
```

### Step 5: Document (15 min)

- [ ] Create incident record
- [ ] Note what was exposed
- [ ] Note when revoked
- [ ] Note remediation steps
- [ ] Schedule post-mortem

---

## 👥 Team Onboarding Checklist

Use this to onboard new team members.

### Day 1: Basics (1 hour)

- [ ] Share `.gitignore` 
- [ ] Share `.env.example`
- [ ] Show what NOT to commit
  - [ ] What is a secret?
  - [ ] Where do secrets come from?
  - [ ] What happens if exposed?

### Day 2: Setup (1 hour)

- [ ] Install pre-commit hooks
```bash
pip install pre-commit
pre-commit install
```

- [ ] Copy `.env.example` to `.env`
- [ ] Get actual environment values
- [ ] Verify `.env` is gitignored
- [ ] Test workflow:
  ```bash
  echo "test" > test.txt
  git add test.txt
  git commit -m "test"  # Should run pre-commit
  git reset HEAD test.txt  # Remove
  rm test.txt
  ```

### Day 3: Best Practices (30 min)

- [ ] Review GOLDEN-TIPS.md
- [ ] Review README.md
- [ ] Ask questions
- [ ] Understand incident response

### Week 1: Verification

- [ ] First commit runs successfully
- [ ] Pre-commit hooks work
- [ ] Can explain why `.gitignore` matters
- [ ] Knows how to rotate a secret

---

## 🔒 DevOps Security Checklist

For DevOps/Infrastructure teams.

### Repository Setup (1 hour)

- [ ] `.gitignore` configured
- [ ] `.env.example` present with templates
- [ ] No `.env` file in repo
- [ ] No terraform `.tfvars` in repo
- [ ] No cloud credential files in repo

### Scanning Configuration (1-2 hours)

- [ ] Pre-commit hooks installed
- [ ] GitHub/GitLab secret scanning enabled
- [ ] CI/CD secret scanning configured
```bash
# GitHub Actions example
- name: Gitleaks
  uses: gitleaks/gitleaks-action@v2
```

- [ ] Baseline secrets created if using detect-secrets
- [ ] False positives documented

### History Audit (30 min - 1 hour)

- [ ] Scanned git history with TruffleHog
- [ ] Scanned with Gitleaks
- [ ] Reviewed results
- [ ] Cleaned up any findings
- [ ] Documented findings

### Secret Management (2-4 hours)

- [ ] Vault initialized or configured
- [ ] AWS Secrets Manager setup
- [ ] Secret rotation policy created
- [ ] Access logging enabled
- [ ] Audit trail configured

### CI/CD Integration (2-4 hours)

- [ ] Secrets in environment variables (not code)
- [ ] Secret scanning in build pipeline
- [ ] Container image scanning
- [ ] Dependency vulnerability scanning
- [ ] Audit logging enabled

### Team & Documentation (1 hour)

- [ ] Team trained on secret management
- [ ] Documentation updated
- [ ] Incident response plan created
- [ ] Emergency contacts listed
- [ ] Regular audit schedule (quarterly)

---

## 🏢 Compliance Checklist

### GDPR (EU)

- [ ] Data protection by design
- [ ] No hardcoded customer data
- [ ] Secret management in place
- [ ] Access logging enabled
- [ ] Right to be forgotten implemented
- [ ] Data retention policy
- [ ] Third-party contracts

### PCI-DSS (Payment Cards)

- [ ] No cardholder data in code
- [ ] No keys/credentials in code
- [ ] Firewall rules configured
- [ ] Encryption in transit
- [ ] Access controls
- [ ] Regular security testing
- [ ] Incident response plan

### HIPAA (Healthcare US)

- [ ] No PHI in code repositories
- [ ] Encryption at rest and transit
- [ ] Access controls and logging
- [ ] Regular backups
- [ ] Incident response plan
- [ ] Business associate agreements
- [ ] Breach notification procedures

### SOC 2 (Service Providers)

- [ ] Security policy documented
- [ ] Access controls enforced
- [ ] Audit logging enabled
- [ ] Incident response plan
- [ ] Regular audits (quarterly)
- [ ] Personnel security training
- [ ] Monitoring and alerting

---

## 🎯 Monthly Review Checklist

Run this every month to maintain security.

### Week 1

- [ ] Review recent commits for secrets
```bash
git log --oneline -20
git show <commit>  # Review each one
```

- [ ] Check for new tool updates
- [ ] Review GitHub alerts
- [ ] Review Dependabot alerts

### Week 2

- [ ] Run full scan with TruffleHog/Gitleaks
- [ ] Document any findings
- [ ] Fix identified issues
- [ ] Update .gitignore if needed

### Week 3

- [ ] Team training/reminders
- [ ] Review compliance requirements
- [ ] Update documentation
- [ ] Check secret rotation status

### Week 4

- [ ] Incident review (if any)
- [ ] Post-mortem from incidents
- [ ] Plan improvements
- [ ] Execute improvements

---

## 🎓 Before Going Live Checklist

Before deploying to production.

### Code Quality (1 hour)

- [ ] No secrets in code
- [ ] No API keys hardcoded
- [ ] No database credentials in code
- [ ] No private keys in code
- [ ] .env.example has only templates
- [ ] No uncommented credentials
- [ ] No debug credentials left in

### Security Scanning (30 min)

- [ ] SAST scan completed (SonarQube, Semgrep)
- [ ] Dependency scan completed (Snyk, OWASP)
- [ ] Container image scan (Trivy, Grype)
- [ ] Secret scan passed (Gitleaks, TruffleHog)
- [ ] No high/critical issues

### Access & Credentials (1 hour)

- [ ] Production secrets in secret manager
- [ ] Database credentials rotated
- [ ] API keys not shared
- [ ] SSH keys secured
- [ ] Certificates valid and renewed
- [ ] Access logged and monitored

### Deployment Safety (30 min)

- [ ] Rollback plan ready
- [ ] Monitoring and alerts configured
- [ ] Incident response team identified
- [ ] Communication channels ready
- [ ] Deployment checklist created

---

## 📋 Post-Incident Checklist

After discovering a security issue.

### Immediate (First Hour)

- [ ] Revoke compromised credentials
- [ ] Notify affected parties
- [ ] Gather evidence (logs, commits)
- [ ] Stop further exposure
- [ ] Enable monitoring

### Short-term (First 24 hours)

- [ ] Root cause analysis
- [ ] Fix identified issues
- [ ] Test fixes thoroughly
- [ ] Remove from git history
- [ ] Force push changes
- [ ] Notify team to pull

### Medium-term (This week)

- [ ] Post-mortem meeting
- [ ] Document lessons learned
- [ ] Create preventive measures
- [ ] Update policies/documentation
- [ ] Team training/reminders

### Long-term (This month)

- [ ] Implement preventive tools
- [ ] Update CI/CD checks
- [ ] Review and improve processes
- [ ] Share findings with team
- [ ] Plan regular audits

---

## ✨ Quick Reference Cards

### Never commit these:

```
❌ .env files
❌ *.pem, *.key, *.crt files
❌ .aws, .gcp, .azure directories
❌ *.tfvars files
❌ kubeconfig files
❌ .docker/config.json
❌ Passwords in comments
❌ API keys
❌ Database credentials
❌ Private keys
```

### Always use these:

```
✅ .gitignore (with all patterns)
✅ .env.example (with empty/fake values)
✅ pre-commit hooks
✅ Environment variables (via Vault/Secrets Manager)
✅ Secret rotation
✅ Secret scanning in CI/CD
✅ Access logging
✅ Incident response plan
```

### Before every commit:

```bash
git diff --cached | grep -i "password\|api\|key\|secret\|token"
# Should return nothing!

git diff --cached | grep "\.env\|DATABASE_URL\|AWS_"
# Should return nothing!

pre-commit run --all-files
# Should pass!
```

---

**Use this checklist for every project. Security is a process, not an event.**

