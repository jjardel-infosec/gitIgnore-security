# 🚀 Quick Start - Git Ignore Security

> 5-minute guide to get started immediately

---

## ⚡ Immediate Setup

### 1️⃣ Copy the .gitignore (30 seconds)

```bash
# Copy the .gitignore file to your project
cp .gitignore /your/project/.gitignore

# Commit immediately
cd /your/project
git add .gitignore
git commit -m "Add comprehensive .gitignore for security"
git push
```

### 2️⃣ Create .env.example (1 minute)

```bash
# Copy as base
cp .env.example /your/project/.env.example

# Customize with your variables
# Remove real values, leave empty or with placeholders
nano /your/project/.env.example

# Commit
git add .env.example
git commit -m "Add .env.example template"
git push
```

### 3️⃣ Create your local .env (1 minute)

```bash
# Create the real .env file
cd /your/project
cp .env.example .env

# Fill with REAL values
# BUT DO NOT COMMIT
nano .env

# Confirm it's in .gitignore
git status  # .env should NOT appear
```

### 4️⃣ Install Pre-commit Hooks (2 minutes)

```bash
# Install
pip install pre-commit

# Create configuration file
cat > /your/project/.pre-commit-config.yaml << 'EOF'
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
EOF

# Install hooks
cd /your/project
pre-commit install

# Run once on all files
pre-commit run --all-files
```

### 5️⃣ Configure GitHub Secret Scanning (1 minute)

If your code is on GitHub:

```
Open your repository
→ Settings
→ Code security & analysis
→ Enable "Secret scanning"
→ Enable "Dependabot alerts"
```

---

## ✅ Quick Verification

Use this checklist to confirm:

```bash
# 1. Check .gitignore
cat /your/project/.gitignore | grep -E "\.env|\.pem|\.key|\.tfvars"

# 2. Check if .env is ignored
cd /your/project
git status | grep ".env"  # Should NOT appear

# 3. Check pre-commit
pre-commit run --all-files

# 4. Check history
git log -p | grep -i "password\|api_key\|secret" | head

# 5. Test commit
echo "test" > testfile.txt
git add testfile.txt
git commit -m "Test pre-commit"  # Should run hooks
```

---

## 🔥 Essential Patterns

If you only have 3 minutes, add THIS to `.gitignore`:

```gitignore
# CRITICAL - Always ignore
.env
.env.local
.env.*.local
*.tfvars
*.tfvars.json
.terraform/
terraform.tfstate*

# KEYS
*.pem
*.key
*.crt
id_rsa
id_rsa.pub

# CLOUD
.aws/
.gcp/
.azure/
kubeconfig

# DATABASE
*.db
dump.sql
database.yml

# LOGS
*.log
logs/

# NODE
node_modules/

# PYTHON
venv/
env/

# IDE
.vscode/
.idea/
```

---

## 🛠️ Essential Tools (3 tools)

If you only install 3:

### 1. Git-Secrets
Prevents committing secrets:
```bash
# macOS
brew install git-secrets
git secrets --install
git secrets --register-aws

# Linux
git clone https://github.com/awslabs/git-secrets.git
cd git-secrets && make install
```

### 2. TruffleHog
Finds already-committed secrets:
```bash
pip install truffleHog
truffleHog filesystem /your/project
```

### 3. Gitleaks
Detects secrets in CI/CD:
```bash
brew install gitleaks
# Or in GitHub Actions:
# name: Gitleaks
# uses: gitleaks/gitleaks-action@v2
```

---

## 🚨 Found Secret Already Committed?

**EXECUTE IMMEDIATELY:**

```bash
# 1. REVOKE IMMEDIATELY
# - Regenerate API keys
# - Change database passwords
# - Revoke GitHub/AWS tokens

# 2. Remove from history
pip install git-filter-repo
git filter-repo --invert-paths --path .env

# 3. Force push (CAREFUL!)
git push origin --force-with-lease

# 4. Notify team
# "Secret was exposed, please do git pull --rebase"
```

---

## 📚 Next Readings

### 5 Min (Done)
- ✅ Copied `.gitignore`
- ✅ Created `.env.example`
- ✅ Installed pre-commit

### 15 Min (Do Today)
- [ ] Read [README.md](./README.md) - Why This Matters
- [ ] Use [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) - Pre-Commit

### 1 Hour (This Week)
- [ ] Read [git-list-ignore.md](./git-list-ignore.md) - Complete Patterns
- [ ] Install 1 extra tool

### 3 Hours (This Month)
- [ ] Study [SECURITY-TOOLS.md](./SECURITY-TOOLS.md) - Tools
- [ ] Implement continuous monitoring

---

## 🎯 Weekly Goals

### Week 1
- [ ] `.gitignore` committed
- [ ] `.env.example` created
- [ ] `pre-commit` installed
- [ ] No secrets detected

### Week 2
- [ ] GitHub Secret Scanning enabled
- [ ] Team notified
- [ ] Documentation shared
- [ ] 1 tool implemented

### Week 3-4
- [ ] Audit completed
- [ ] Issues resolved
- [ ] Monitoring active
- [ ] Team trained

---

## 🤔 Quick FAQ

**Q: Why can't .env be committed?**
A: Contains passwords, API keys and real credentials.

**Q: What about environment variables?**
A: Still use `.env.example` as template, not `.env`.

**Q: What to do with `.env.example`?**
A: Commit WITHOUT real values, only empty or fake ones.

**Q: Will pre-commit hooks block my commits?**
A: Yes, from secrets! That's good.

**Q: Can I use .env in production?**
A: NO. Use secret managers (AWS Secrets Manager, Vault).

**Q: How long does it take?**
A: 5 minutes setup + 1 hour everything ready.

**Q: What if it was committed before?**
A: Immediately revoke and remove from history (see section above).

---

## 📞 Quick Support

### Error: "git-secrets" not found
```bash
# macOS
brew install git-secrets

# Linux
sudo apt-get install git-secrets
```

### Error: ".env" is not being ignored
```bash
# Remove from cache
git rm --cached .env

# Add to .gitignore
echo ".env" >> .gitignore

# Commit
git add .gitignore
git commit -m "Update .gitignore"
```

### Error: "pre-commit not found"
```bash
pip install pre-commit
pre-commit install
```

---

## 🎓 After This

Once setup is done:

1. **Keep updated**: Review `.gitignore` monthly
2. **Educate team**: Share [README.md](./README.md)
3. **Automate**: Configure GitHub Actions for secret scanning
4. **Monitor**: Use [SECURITY-TOOLS.md](./SECURITY-TOOLS.md)
5. **Audit**: Use [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) quarterly

---

## 🔗 Full Documentation

- 📘 [Index](./INDEX.md) - Documentation map
- 📖 [README.md](./README.md) - Complete view
- 📋 [git-list-ignore.md](./git-list-ignore.md) - Detailed patterns
- ✅ [SECURITY-CHECKLIST.md](./SECURITY-CHECKLIST.md) - Audits
- 🛠️ [SECURITY-TOOLS.md](./SECURITY-TOOLS.md) - Tools

---

**⏱️ Have 5 minutes? [Do the setup](#️-immediate-setup)**

**Total time to production:** 1 hour

**Benefit:** Protects 99% of secret leaks

---

> 🔐 **First step is always the best step** - Start today!
