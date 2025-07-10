# CI/CD Quick Start Guide

Fast reference for Environmental Voting CI/CD pipeline.

---

## ✅ What's Included

### GitHub Actions Workflows
- ✅ **test.yml** - Automated testing on Node 18.x & 20.x
- ✅ **quality.yml** - Code quality checks (Solhint, security)
- ✅ **pr-check.yml** - Pull request validation
- ✅ **deploy.yml** - Manual deployment automation

### Configuration Files
- ✅ **LICENSE** - MIT License
- ✅ **.solhint.json** - Solidity linting rules
- ✅ **codecov.yml** - Coverage configuration (90% target)

### Testing Features
- ✅ Multi-version Node.js (18.x, 20.x)
- ✅ Automatic test runs on push/PR
- ✅ Coverage upload to Codecov
- ✅ Gas reporting
- ✅ Contract size checks

---

## 🚀 Quick Commands

```bash
# Run full CI pipeline locally
npm run ci

# Individual checks
npm run lint              # Solidity linting
npm run compile           # Compile contracts
npm test                  # Run all tests
npm run coverage          # Generate coverage
npm run size              # Check contract sizes

# Testing
npm run test:unit         # Unit tests only
npm run test:gas          # With gas reporting
npm run test:sepolia      # Sepolia testnet tests
```

---

## 🔧 GitHub Setup (One-Time)

### 1. Add Repository Secrets

Go to: **Settings → Secrets and variables → Actions**

Add these secrets:
```
SEPOLIA_RPC_URL          # https://sepolia.infura.io/v3/YOUR-PROJECT-ID
PRIVATE_KEY              # Your wallet private key (0x...)
ETHERSCAN_API_KEY        # From etherscan.io/myapikey
CODECOV_TOKEN            # From codecov.io (after adding repo)
```

### 2. Setup Codecov

1. Visit https://codecov.io
2. Sign in with GitHub
3. Add your repository
4. Copy token
5. Add as `CODECOV_TOKEN` secret in GitHub

### 3. Enable Workflows

Workflows activate automatically when you push to GitHub.

---

## 📊 What Runs Automatically

### On Every Push to main/develop:
```
✓ Compile contracts
✓ Run all tests (Node 18.x & 20.x)
✓ Generate coverage
✓ Upload to Codecov
✓ Check contract sizes
✓ Run Solhint
✓ Security audit
```

### On Every Pull Request:
```
✓ All tests from push
✓ PR validation
✓ Coverage threshold check (90%)
✓ Automated PR comment
✓ Security scan
```

---

## 🎯 Workflow Triggers

| Workflow | Trigger | When |
|----------|---------|------|
| test.yml | push, PR | main/develop branches |
| quality.yml | push, PR | main/develop branches |
| pr-check.yml | PR | All PRs |
| deploy.yml | manual | Actions tab |

---

## 📋 Pre-Push Checklist

Before pushing code:

```bash
# 1. Run linting
npm run lint

# 2. Run tests
npm test

# 3. Check coverage
npm run coverage

# 4. Full CI check
npm run ci
```

---

## 🔍 View Workflow Status

### Via GitHub UI
1. Go to **Actions** tab
2. See all workflow runs
3. Click run for details

### Via GitHub CLI
```bash
gh run list              # List recent runs
gh run watch             # Watch current run
gh run view --log        # View logs
```

---

## 🎨 Add Status Badges

Add to README.md (replace USERNAME/REPO):

```markdown
![Tests](https://github.com/USERNAME/REPO/actions/workflows/test.yml/badge.svg)
![Quality](https://github.com/USERNAME/REPO/actions/workflows/quality.yml/badge.svg)
[![codecov](https://codecov.io/gh/USERNAME/REPO/branch/main/graph/badge.svg)](https://codecov.io/gh/USERNAME/REPO)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
```

---

## 🚢 Manual Deployment

### Deploy to Sepolia

**Via GitHub UI:**
1. Go to **Actions** tab
2. Select "Deploy to Sepolia"
3. Click "Run workflow"
4. Choose environment: `sepolia`
5. Click "Run workflow"

**Via GitHub CLI:**
```bash
gh workflow run deploy.yml -f environment=sepolia
```

---

## 📈 Coverage Requirements

| Metric | Target | Threshold |
|--------|--------|-----------|
| Project | 90% | ±2% |
| Patch | 85% | ±5% |

Coverage must meet targets for PR approval.

---

## 🐛 Troubleshooting

### Tests Fail in CI but Pass Locally

**Check Node version:**
```bash
node --version          # Should be 18.x or 20.x
nvm use 18              # Switch version
npm test                # Test again
```

### Coverage Upload Fails

**Verify token:**
```bash
# Check token is set in GitHub Secrets
# Regenerate if needed from codecov.io
```

### Linting Errors

**Fix automatically:**
```bash
npm run lint:fix

# Or check specific issues:
npm run lint
```

---

## 📚 Full Documentation

- **[CI_CD.md](./CI_CD.md)** - Complete CI/CD guide (12KB)
- **[TESTING.md](./TESTING.md)** - Testing documentation (16KB)
- **[DEPLOYMENT.md](./DEPLOYMENT.md)** - Deployment guide (15KB)

---

## ✅ Verification

Check everything is working:

```bash
# 1. Local CI
npm run ci

# 2. Push test branch
git checkout -b test-ci
git push origin test-ci

# 3. Watch Actions tab
# See workflows run automatically

# 4. Create test PR
gh pr create

# 5. Check PR comments
# See automated validation
```

---

## 🎯 Quality Gates

All checks must pass:

- ✅ All tests passing (57+ tests)
- ✅ Coverage ≥ 90%
- ✅ Solhint checks pass
- ✅ No security issues
- ✅ Contract size < 24KB
- ✅ No hardcoded secrets

---

## 📞 Support

For issues:
1. Check [CI_CD.md](./CI_CD.md) troubleshooting section
2. Review workflow logs in Actions tab
3. Verify all secrets are set correctly

---

**Happy Building! 🚀**

All workflows in English with no project-specific naming.
