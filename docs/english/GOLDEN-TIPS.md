# Golden Tips - Top 10 Security Insights

> Essential security practices and daily habits for DevSecOps

---

## 🌟 Tip #1: The .env.example Rule

**The Rule:** Create `.env.example` with EMPTY or FAKE values, commit it. Create `.env` with REAL values, never commit it.

**Why:** Team members can see what variables are needed without seeing real secrets.

```bash
# ✅ Commit to repo (.env.example)
DATABASE_URL=postgres://localhost/myapp
API_KEY=your_api_key_here
SECRET_KEY=change_me

# ❌ Never commit (.env)
DATABASE_URL=postgres://user:password123@prod.db.example.com/myapp
API_KEY=sk_live_1234567890abcdefghijklmnopqrstuvwxyz
SECRET_KEY=super_secret_key_do_not_share
```

**Implementation:**
```bash
echo ".env" >> .gitignore
cp .env.example .env
# Edit .env with real values
git add .gitignore .env.example
git commit -m "Add configuration templates"
git push
```

---

## 🌟 Tip #2: Pre-commit Hooks are Your Safety Net

**The Rule:** Install pre-commit hooks immediately. They catch 95% of secret leaks.

**Why:** Hooks run BEFORE commit, blocking secrets before they ever reach git history.

```bash
# Install once
pip install pre-commit
pre-commit install

# Now pre-commit runs automatically on every commit
# If secrets found → commit blocked → you fix it
# If no secrets found → commit proceeds normally
```

**Time saved:** 10 seconds per commit to prevent 10-day incident response.

---

## 🌟 Tip #3: The 2-Minute Secret Rotation

**The Rule:** If credentials are exposed, revoke within 2 minutes.

**Why:** Attacker average time to use exposed credential: 3 minutes.

```bash
# IMMEDIATELY do this:

# 1. Revoke
aws iam delete-access-key --access-key-id AKIAIOSFODNN7EXAMPLE

# 2. Regenerate
aws iam create-access-key --user-name <username>

# 3. Update application
# Update .env, Vault, or secret manager

# 4. Notify team
echo "API key rotated, please redeploy"
```

**Time to execute:** 2 minutes
**Cost of not doing:** Unlimited cloud resource usage

---

## 🌟 Tip #4: Three Sacred Files

**The Rule:** These three files MUST be in EVERY git repository.

| File | Purpose | Always Commit |
|------|---------|---------------|
| `.gitignore` | List what NOT to commit | ✅ YES |
| `.env.example` | Template with fake values | ✅ YES |
| `.pre-commit-config.yaml` | Hook configuration | ✅ YES |

```bash
# Verify they exist
ls -la | grep -E "gitignore|env.example|pre-commit"

# If missing, add them
cp .gitignore .gitignore
cp .env.example .env.example
touch .pre-commit-config.yaml
```

---

## 🌟 Tip #5: The Audit Before Production

**The Rule:** Always audit git history before deploying to production.

**Why:** Find 80% of secrets before they're live.

```bash
# Quick audit (5 minutes)
git log -p | grep -i "password\|api_key\|secret" | head -20

# Complete audit (30 minutes)
gitleaks detect --source . -v

# Check specific commits
git show <commit-hash> | grep -i "password"
```

**Frequency:** Every release, every week, every month

---

## 🌟 Tip #6: Never Use Real Credentials Locally

**The Rule:** Development always uses fake credentials.

**Why:** Local development = less secure than production.

```bash
# ✅ Development (.env.example)
DATABASE_URL=postgres://localhost/myapp_dev
API_KEY=fake_test_key_do_not_use

# ✅ Local (.env)
DATABASE_URL=postgres://user:password@localhost/myapp_dev
API_KEY=sk_test_1234567890abcdefghijklmnopqrstuvwxyz

# ❌ Never do this locally
API_KEY=sk_live_production_key  # Wrong!
DATABASE_URL=postgres://user:password@prod-db.aws.com  # Wrong!
```

**Benefit:** If laptop stolen, only dev keys compromised, not production.

---

## 🌟 Tip #7: Terraform .tfvars are Credentials

**The Rule:** `.tfvars` files contain infrastructure secrets. Treat like passwords.

```bash
# ✅ Add to .gitignore
echo "*.tfvars" >> .gitignore
echo "*.tfvars.json" >> .gitignore
echo ".terraform/" >> .gitignore
echo "*.tfstate*" >> .gitignore

# ❌ Never commit
terraform.tfvars  # Contains AWS keys, database passwords
main.tfvars       # Contains API keys
prod.tfvars       # Contains production credentials
```

**Use instead:** Terraform Cloud, environment variables, or secret manager.

---

## 🌟 Tip #8: The 5-File Security Report

**The Rule:** These 5 files tell you if you're secure.

**How to check:**
```bash
# 1. .gitignore exists and is comprehensive
test -f .gitignore && echo "✅ .gitignore exists" || echo "❌ Missing"

# 2. .env is ignored
grep "^\.env$" .gitignore && echo "✅ .env ignored" || echo "❌ Not ignored"

# 3. Pre-commit installed
pre-commit run --all-files && echo "✅ Pre-commit works" || echo "❌ Failed"

# 4. No secrets in history
git log -p | grep -i "password\|api_key" && echo "❌ Secrets found" || echo "✅ Clean"

# 5. GitHub secret scanning enabled
# Check repository settings → Code security & analysis
```

**Frequency:** Weekly

---

## 🌟 Tip #9: The Incident Response Reflex

**The Rule:** If you accidentally commit a secret, execute incident response immediately.

**The 3-Step Reflex:**

1. **Revoke (2 min)** - Deactivate the credential immediately
   ```bash
   aws iam delete-access-key --access-key-id KEY_ID
   ```

2. **Remove (15 min)** - Remove from git history
   ```bash
   pip install git-filter-repo
   git filter-repo --invert-paths --path secret-file.txt
   ```

3. **Notify (5 min)** - Tell team to pull
   ```bash
   echo "Security incident: git force-push incoming, please git pull --rebase"
   git push --force-with-lease
   ```

**Time to execute:** 20 minutes
**Cost of not doing:** Full incident response (5-30 days)

---

## 🌟 Tip #10: Trust But Verify

**The Rule:** Automated tools catch 95% of secrets, but not 100%.

**The 1% they miss:**
- Secrets encoded in code logic
- Secrets in comments
- Non-standard formats
- Context-dependent secrets

**How to catch:** Manual code review

```bash
# During code review, search for:
"password"
"secret"
"api_key"
"token"
"credential"
"key ="
"password:"

# Also check:
- Are there hardcoded database URLs?
- Are there hardcoded API endpoints?
- Are there default credentials?
- Are there commented-out secrets?
```

**Frequency:** Every pull request, every deployment

---

## 📋 Daily Checklist

Use this EVERY day:

```
⬜ [ ] Did I modify .env or local config? (Remember: never commit!)
⬜ [ ] Did I add any new credentials? (Add to .env.example as template)
⬜ [ ] Am I about to push? (Run: git diff --cached | grep -i "password")
⬜ [ ] Did pre-commit hooks run? (Yes = good, No = fix before pushing)
⬜ [ ] Is my code review buddy? (Someone else caught what I missed?)
```

---

## ⏰ Weekly Checklist

```
⬜ [ ] Run git log audit
⬜ [ ] Review GitHub secret scanning alerts
⬜ [ ] Check team for uncommented credentials
⬜ [ ] Update .gitignore if needed
⬜ [ ] Verify team installed pre-commit hooks
```

---

## 📊 Quick Reference

| Situation | Action | Time |
|-----------|--------|------|
| **About to commit** | Run pre-commit hooks | 10 sec |
| **Just before push** | `git diff --cached \| grep password` | 5 sec |
| **Found secret committed** | Revoke → git-filter-repo → force push | 20 min |
| **Team member asks** | Share .env.example, not real .env | 1 min |
| **Code review** | Search for hardcoded credentials | 5 min |
| **Before production** | `gitleaks detect --source .` | 2 min |
| **Monthly** | Full repository audit | 30 min |

---

## 🎯 The Security Mindset

**Remember these three principles:**

1. **Default Deny:** Assume everything is secret unless proven otherwise
2. **Belt & Suspenders:** Use multiple layers (hooks + scanning + manager)
3. **When in Doubt:** Don't commit it, ask first

---

## ❓ Golden Questions to Ask Yourself

Before every commit, ask:

1. **Do I see a password or key in this diff?**
   - Yes → Don't commit, move to .env
   - No → Continue

2. **Would I be comfortable if this code became public?**
   - No → There's a secret, move it
   - Yes → Safe to commit

3. **Could anyone reverse-engineer credentials from this code?**
   - Yes → Don't commit
   - No → Safe to commit

---

## 🚀 Next Steps

1. **Today:** Implement Tip #1 and #2 (20 minutes)
2. **This week:** Complete all tips (1 hour)
3. **This month:** Make them habits

---

**Remember:** The best security is the one you do every day, not the one you plan to do someday.

