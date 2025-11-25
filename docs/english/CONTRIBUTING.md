# Contributing - How to Improve This Documentation

> Guidelines for improving and maintaining this security documentation

---

## 🎯 What We Need

### Documentation Improvements

- [ ] Additional patterns for `.gitignore`
- [ ] More tool examples
- [ ] Case studies and real examples
- [ ] Translations to other languages
- [ ] Video guides or tutorials
- [ ] Automated testing scenarios

### Tool Updates

- [ ] New tools discovered
- [ ] Tool version updates
- [ ] Installation instructions refinement
- [ ] Troubleshooting additions
- [ ] Performance improvements

### Best Practices

- [ ] New security patterns
- [ ] Incident response examples
- [ ] Compliance additions
- [ ] Industry-specific guidance
- [ ] Enterprise patterns

### Feedback

- [ ] Clarity improvements
- [ ] Structure suggestions
- [ ] Missing sections
- [ ] Corrections
- [ ] Real-world scenarios

---

## 📋 Before You Start

### Check the Following

1. **Is this already in the docs?**
   - Use Ctrl+F to search
   - Check [INDEX.md](./INDEX.md) for keywords
   
2. **Is this still relevant?**
   - Check tool versions
   - Verify commands work
   - Test with current versions

3. **Does it fit the scope?**
   - Related to Git security?
   - Related to secret management?
   - Related to DevSecOps?
   - If not, consider a separate project

---

## ✍️ Writing Style Guide

### Structure

```markdown
# Section Header (H1)

Brief intro (1-2 sentences)

## Subsection (H2)

Detailed explanation with examples

### Sub-subsection (H3)

More specific information
```

### Code Blocks

Include language identifier:

````markdown
```bash
# Shell commands
echo "example"
```

```python
# Python code
print("example")
```

```yaml
# Configuration files
key: value
```
````

### Lists

Use for quick reference:

```markdown
- Item 1
- Item 2
  - Sub-item
  - Sub-item
```

### Tables

For comparisons:

```markdown
| Column 1 | Column 2 |
|----------|----------|
| Value 1  | Value 2  |
```

### Time Estimates

Always include time for tasks:

```
## Installation (10 minutes)
## Audit (1-2 hours)
## Full Setup (4 hours)
```

### Emojis

Use sparingly for visual breaks:

- 📋 For checklists
- 🔐 For security
- 🚀 For quick starts
- 💡 For tips
- ❌ For don'ts
- ✅ For dos

---

## 🔄 Contribution Process

### Step 1: Fork & Clone

```bash
# Fork this repository on GitHub
# Clone your fork
git clone https://github.com/YOUR_USERNAME/gitIgnore-security.git
cd gitIgnore-security

# Add upstream
git remote add upstream https://github.com/ORIGINAL_OWNER/gitIgnore-security.git
```

### Step 2: Create a Branch

```bash
# Update from upstream
git fetch upstream
git checkout main
git merge upstream/main

# Create feature branch
git checkout -b feature/your-feature-name
```

### Step 3: Make Changes

Edit the relevant `.md` file:

```bash
# Example: Adding new patterns
nano docs/english/git-list-ignore.md

# Example: Improving checklist
nano docs/english/SECURITY-CHECKLIST.md
```

### Step 4: Verify Changes

```bash
# Check syntax
ls -la docs/english/*.md

# Verify links
grep -r "](\./" docs/english/

# Test markdown (if you have a tool)
mdl docs/english/*.md
```

### Step 5: Commit

Follow the commit style:

```bash
# Pattern: type(scope): description

git add .
git commit -m "docs(tools): add Semgrep setup instructions"
git commit -m "docs(checklist): update compliance requirements"
git commit -m "docs(gitignore): add Kubernetes secret patterns"
```

**Commit types:**
- `docs` - Documentation changes
- `tools` - Tool-related changes
- `fix` - Bug fixes
- `feature` - New features
- `perf` - Performance improvements

**Scopes:**
- `checklist` - SECURITY-CHECKLIST.md changes
- `tools` - SECURITY-TOOLS.md changes
- `gitignore` - git-list-ignore.md changes
- `readme` - README.md changes
- `tips` - GOLDEN-TIPS.md changes

### Step 6: Push & Create PR

```bash
# Push your branch
git push origin feature/your-feature-name

# Create PR on GitHub
# Fill in description
# Reference related issues
```

### Step 7: Respond to Reviews

```bash
# Make requested changes
git add .
git commit -m "docs(scope): address review comments"
git push origin feature/your-feature-name

# Wait for approval
# Maintainer merges when ready
```

---

## 📝 Contribution Types & Requirements

### Bug Fixes

**What:** Errors, broken links, incorrect information
**Requirements:**
- [ ] Description of the bug
- [ ] How to reproduce
- [ ] Expected vs. actual behavior
- [ ] Proposed fix

**Example:**
```
Title: docs(tools): incorrect AWS Secrets Manager URL

Description: The AWS Secrets Manager console URL in 
SECURITY-TOOLS.md points to wrong region.

Current: https://console.aws.amazon.com/secretsmanager/
Expected: https://[region].console.aws.amazon.com/secretsmanager/

Proposed fix: Add [region] placeholder
```

### Documentation Additions

**What:** New sections, examples, patterns
**Requirements:**
- [ ] Clear title and purpose
- [ ] Explanation (1-2 paragraphs)
- [ ] Code examples if applicable
- [ ] Time estimate
- [ ] Links to related sections

**Example:**
```
## New Section: Windows-Specific Patterns

On Windows, certain files behave differently...

[explanation]

### Installation
[step-by-step]

### Verification
[how to test]

**Time:** 15 minutes
**Related:** [link to related section]
```

### Tool Updates

**What:** New versions, new features, alternatives
**Requirements:**
- [ ] Tool name and version
- [ ] What changed
- [ ] Installation steps
- [ ] Usage example
- [ ] Compatibility notes

### Real-World Examples

**What:** Case studies, incident examples
**Requirements:**
- [ ] Anonymized details (no company names)
- [ ] What happened
- [ ] How it was caught/prevented
- [ ] Lessons learned
- [ ] Links to relevant sections

### Translations

**What:** Translate to other languages
**Requirements:**
- [ ] Complete and accurate translation
- [ ] Created in `/docs/[language]/` folder
- [ ] All files translated (not partial)
- [ ] README with language info

---

## 🎨 Style Consistency

### Naming Conventions

- Files: `kebab-case.md`
- Sections: `Title Case`
- Variables: `UPPER_SNAKE_CASE`
- Directories: `lowercase`

### Formatting

```markdown
# Headings use single #
## Subsections use ##
### Details use ###

**Bold** for emphasis
`code` for inline code
[Links](./to-files.md) always relative

> Blockquotes for important notes

---
Horizontal rules for section breaks
```

### Link Format

Always use relative links:

```markdown
# Good
[See this document](./SECURITY-CHECKLIST.md)
[Read section](./README.md#compliance--standards)

# Bad
[See this document](https://github.com/owner/repo/blob/main/docs/english/SECURITY-CHECKLIST.md)
```

### File Organization

```
docs/
├── english/
│   ├── README.md
│   ├── QUICK-START.md
│   ├── git-list-ignore.md
│   ├── SECURITY-CHECKLIST.md
│   ├── SECURITY-TOOLS.md
│   ├── GOLDEN-TIPS.md
│   ├── EXECUTIVE-SUMMARY.md
│   ├── OVERVIEW.md
│   ├── NAVIGATION-MAP.md
│   ├── INDEX.md
│   ├── START-HERE.md
│   ├── CONTRIBUTING.md
│   ├── FINAL-SUMMARY.md
│   ├── .gitignore
│   └── .env.example
└── pt-br/
    └── [Portuguese versions]
```

---

## 🧪 Testing Your Changes

### Local Verification

```bash
# Check markdown syntax
# (if you have a linter)
markdownlint docs/english/*.md

# Verify links are relative
grep -r "https://" docs/english/*.md | grep -v "http://127"

# Check for consistency
grep -i "gitleaks\|Git Leaks\|git-leaks" docs/english/*.md
# Should be consistent use of one format
```

### Before Submitting PR

- [ ] Spell check
- [ ] Grammar check
- [ ] Links verified
- [ ] Code examples tested
- [ ] Time estimates accurate
- [ ] No broken formatting
- [ ] Consistent with style guide
- [ ] Related links added

---

## 📚 Quality Standards

### Content Quality

- [ ] Clear and concise
- [ ] Technically accurate
- [ ] Up-to-date information
- [ ] Examples work
- [ ] Links not broken
- [ ] Time estimates realistic

### Documentation Quality

- [ ] Proper markdown formatting
- [ ] Consistent style
- [ ] Proper headings hierarchy
- [ ] Appropriate use of emphasis
- [ ] Code blocks highlighted
- [ ] Links relative and working

### Completeness

- [ ] Introduction explains purpose
- [ ] All prerequisites listed
- [ ] Step-by-step instructions
- [ ] Verification/testing section
- [ ] Troubleshooting if applicable
- [ ] Related resources linked

---

## 🚀 Large Contributions

### For Major Changes

1. **Open an issue first**
   - Describe your proposal
   - Get feedback before implementing
   - Avoid wasted effort

2. **Start with a draft PR**
   - Mark as Draft
   - Get early feedback
   - Iterate based on comments

3. **Break into sections**
   - Don't change everything at once
   - Easier to review
   - Easier to merge

4. **Add documentation**
   - Explain your changes
   - Link to related issues
   - Reference relevant sections

---

## 💬 Communication

### Questions?

- **GitHub Issues:** For questions about content
- **Discussions:** For broader topics
- **Pull Requests:** Comments for specific changes

### Reporting Issues

Use this template:

```markdown
### Issue Title

### Description
What is the issue?

### Steps to Reproduce
1. First step
2. Second step
3. etc.

### Expected Behavior
What should happen?

### Actual Behavior
What actually happens?

### Proposed Solution
How could this be fixed?

### Context
- OS: Windows/Mac/Linux
- Tool version: X.Y.Z
- Related files: path/to/file.md
```

---

## 🎓 Review Process

### What Reviewers Check

1. **Accuracy** - Is the information correct?
2. **Clarity** - Is it well explained?
3. **Completeness** - Are all steps included?
4. **Style** - Does it match the guide?
5. **Quality** - Is it high quality?

### Feedback Types

- **Request Changes** - Must be addressed
- **Comment** - Nice to address
- **Approve** - Ready to merge

### After Review

1. **Address comments**
   - Make changes
   - Push new commits
   - Respond to questions

2. **Request re-review**
   - After making changes
   - Reviewer will check again

3. **Celebrate!**
   - Your contribution is merged
   - You're in the history

---

## 🏆 Recognition

### Contributors

All contributors are recognized in:
- Git history (commit authors)
- GitHub contributors page
- CONTRIBUTORS.md (coming soon)

### Becoming a Maintainer

Regular, high-quality contributors may be asked to help maintain. No pressure! Help as much as you can.

---

## ❓ FAQ

**Q: How long before my PR is reviewed?**
A: Usually within 1-2 weeks. Urgent issues get faster reviews.

**Q: Can I ask questions in my PR?**
A: Yes! Use PR comments. Reviewers are happy to help.

**Q: What if I disagree with feedback?**
A: Discuss it! It's a conversation, not a command.

**Q: Do I need to test everything?**
A: For code changes, yes. For docs, spell/grammar check is enough.

**Q: Can I contribute in other languages?**
A: Absolutely! Create `/docs/[language]/` folder and translate.

**Q: Is there a code of conduct?**
A: Be respectful and helpful. That's it.

---

## 🙏 Thank You!

Contributing makes this documentation better for everyone. Even small fixes help!

---

**Ready to contribute? Pick an issue, follow this guide, and submit a PR!**

