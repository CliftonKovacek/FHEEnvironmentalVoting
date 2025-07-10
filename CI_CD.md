# CI/CD Pipeline Documentation

Comprehensive Continuous Integration and Continuous Deployment setup for Environmental Voting.

---

## 📋 Table of Contents

- [Overview](#overview)
- [GitHub Actions Workflows](#github-actions-workflows)
- [Setup Instructions](#setup-instructions)
- [Code Quality](#code-quality)
- [Coverage Integration](#coverage-integration)
- [Deployment Process](#deployment-process)
- [Troubleshooting](#troubleshooting)

---

## 🎯 Overview

This project uses GitHub Actions for automated CI/CD with the following features:

### Automated Processes

✅ **Automated Testing**
- Unit tests on every push and PR
- Multi-version Node.js testing (18.x, 20.x)
- Gas reporting
- Coverage generation

✅ **Code Quality Checks**
- Solhint linting
- Security audits
- Contract size verification
- Formatting validation

✅ **Deployment Automation**
- Manual deployment to Sepolia
- Automatic contract verification
- Deployment artifact storage

✅ **Pull Request Validation**
- Automated PR checks
- Coverage threshold enforcement
- Security scanning
- Automated comments with results

---

## 🔄 GitHub Actions Workflows

### 1. Test Suite (`test.yml`)

**Triggers:**
- Push to `main`, `develop`, or `master` branches
- Pull requests to these branches

**Jobs:**
```yaml
Matrix Strategy:
  - Node.js 18.x
  - Node.js 20.x
```

**Steps:**
1. Checkout code
2. Setup Node.js
3. Install dependencies
4. Compile contracts
5. Run tests
6. Generate gas report
7. Generate coverage
8. Upload to Codecov
9. Check contract sizes

**Usage:**
```bash
# Manually trigger
gh workflow run test.yml

# View status
gh run list --workflow=test.yml
```

---

### 2. Code Quality (`quality.yml`)

**Triggers:**
- Push to main branches
- All pull requests

**Jobs:**

#### Lint Check
- Runs Solhint on all Solidity files
- Checks code style compliance
- Validates formatting

#### Security Audit
- npm audit for dependencies
- Generates security report
- Uploads audit artifacts

#### Contract Size Check
- Verifies contracts under 24KB limit
- Reports size for each contract
- Fails if limit exceeded

**Usage:**
```bash
# Run linting locally
npm run lint

# Fix auto-fixable issues
npm run lint:fix
```

---

### 3. Pull Request Check (`pr-check.yml`)

**Triggers:**
- PR opened, synchronized, or reopened

**Jobs:**

#### Validate PR
- Check commit messages
- Compile contracts
- Run all tests
- Generate coverage
- Verify coverage threshold (90%)
- Check contract sizes
- Post automated comment

#### Security Check
- Run security audit
- Scan for hardcoded secrets
- Check for vulnerabilities

**Features:**
- Automated PR comments with results
- Security scanning
- Coverage enforcement

---

### 4. Deploy (`deploy.yml`)

**Triggers:**
- Manual workflow dispatch

**Inputs:**
- `environment`: sepolia or localhost

**Jobs:**
1. Run tests before deployment
2. Deploy contract
3. Verify on Etherscan
4. Upload deployment artifacts

**Usage:**
```bash
# Deploy via GitHub UI
# Go to Actions → Deploy to Sepolia → Run workflow

# Or via CLI
gh workflow run deploy.yml -f environment=sepolia
```

---

## 🛠️ Setup Instructions

### 1. Repository Secrets

Configure these secrets in GitHub Settings → Secrets and variables → Actions:

```bash
# Required for deployment
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR-PROJECT-ID
PRIVATE_KEY=your-wallet-private-key
ETHERSCAN_API_KEY=your-etherscan-api-key

# Required for coverage
CODECOV_TOKEN=your-codecov-token
```

**How to add secrets:**
1. Go to repository Settings
2. Click "Secrets and variables" → "Actions"
3. Click "New repository secret"
4. Add name and value
5. Click "Add secret"

---

### 2. Branch Protection Rules

Recommended settings for `main` branch:

```yaml
Branch Protection Settings:
  ✅ Require pull request reviews (1 approver)
  ✅ Require status checks to pass
     - test (Node 18.x)
     - test (Node 20.x)
     - lint
     - security-check
  ✅ Require branches to be up to date
  ✅ Do not allow bypassing settings
```

**Setup:**
1. Go to Settings → Branches
2. Add branch protection rule
3. Branch name pattern: `main`
4. Enable required settings above
5. Save changes

---

### 3. Codecov Integration

**Step 1: Get Codecov Token**
1. Visit https://codecov.io
2. Sign in with GitHub
3. Add your repository
4. Copy the token

**Step 2: Add to GitHub Secrets**
```bash
Name: CODECOV_TOKEN
Value: <your-token-here>
```

**Step 3: Verify Integration**
- Push to repository
- Check GitHub Actions
- Visit Codecov dashboard
- Confirm coverage upload

---

## 📊 Code Quality

### Solhint Configuration

**File:** `.solhint.json`

**Rules Enabled:**
- Compiler version enforcement (^0.8.0)
- Function visibility checks
- Naming conventions
- Security best practices
- Gas optimization hints
- Maximum line length (120)
- Event naming standards

**Running Locally:**
```bash
# Check all contracts
npm run lint

# Auto-fix issues
npm run lint:fix

# Check specific file
npx solhint contracts/EnvironmentalVoting.sol
```

**Common Issues:**

| Issue | Solution |
|-------|----------|
| `max-line-length` | Break long lines |
| `func-visibility` | Add visibility modifier |
| `var-name-mixedcase` | Use mixedCase for variables |
| `no-unused-vars` | Remove or use the variable |

---

### Security Audits

**Automated Checks:**
- npm audit on every PR
- Dependency vulnerability scanning
- Secret detection in code
- High-severity issue reporting

**Manual Audit:**
```bash
# Run audit locally
npm audit

# View detailed report
npm audit --json > audit-report.json

# Fix vulnerabilities
npm audit fix
```

---

## 📈 Coverage Integration

### Codecov Dashboard

**Features:**
- Visual coverage reports
- PR coverage diff
- Trend tracking
- File-level breakdown

**Badge:**
```markdown
[![codecov](https://codecov.io/gh/YOUR-USERNAME/YOUR-REPO/branch/main/graph/badge.svg)](https://codecov.io/gh/YOUR-USERNAME/YOUR-REPO)
```

### Coverage Targets

```yaml
Project Coverage:
  Target: 90%
  Threshold: 2%

Patch Coverage:
  Target: 85%
  Threshold: 5%
```

**Local Coverage:**
```bash
# Generate coverage report
npm run coverage

# View in browser
open coverage/index.html
```

---

## 🚀 Deployment Process

### Automated Deployment

**Step 1: Trigger Workflow**
```bash
# Via GitHub UI
Actions → Deploy to Sepolia → Run workflow

# Via GitHub CLI
gh workflow run deploy.yml -f environment=sepolia
```

**Step 2: Monitor Progress**
- Watch GitHub Actions tab
- Check deployment logs
- Verify contract address

**Step 3: Verify Deployment**
- Check Etherscan link
- Test contract interaction
- Verify source code

### Manual Deployment

```bash
# 1. Ensure tests pass
npm test

# 2. Deploy to Sepolia
npm run deploy

# 3. Verify contract
npm run verify

# 4. Test interaction
npm run interact
```

---

## 🎨 Status Badges

Add these to README.md:

```markdown
![Test Suite](https://github.com/USERNAME/REPO/actions/workflows/test.yml/badge.svg)
![Code Quality](https://github.com/USERNAME/REPO/actions/workflows/quality.yml/badge.svg)
[![codecov](https://codecov.io/gh/USERNAME/REPO/branch/main/graph/badge.svg)](https://codecov.io/gh/USERNAME/REPO)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)
```

---

## 🐛 Troubleshooting

### Common Issues

#### 1. Tests Failing in CI but Passing Locally

**Possible Causes:**
- Node version mismatch
- Missing environment variables
- Timing issues

**Solution:**
```bash
# Test with same Node version
nvm use 18
npm test

# Check for timing issues
npm test -- --timeout 60000
```

#### 2. Coverage Upload Failing

**Possible Causes:**
- Invalid Codecov token
- Network issues
- Coverage file not generated

**Solution:**
```bash
# Verify token is set
echo $CODECOV_TOKEN

# Generate coverage locally
npm run coverage

# Check coverage file exists
ls coverage/lcov.info
```

#### 3. Solhint Errors

**Solution:**
```bash
# See all errors
npm run lint

# Fix auto-fixable issues
npm run lint:fix

# Check specific file
npx solhint contracts/EnvironmentalVoting.sol
```

#### 4. Deployment Workflow Fails

**Possible Causes:**
- Missing secrets
- Insufficient funds
- Network issues

**Solution:**
```bash
# Verify secrets are set
# Check in Settings → Secrets

# Test locally first
npm run deploy:local

# Check account balance
# Ensure Sepolia ETH available
```

---

## 📋 Workflow Files Reference

### Directory Structure

```
.github/
└── workflows/
    ├── test.yml          # Main test suite
    ├── quality.yml       # Code quality checks
    ├── pr-check.yml      # Pull request validation
    └── deploy.yml        # Deployment automation
```

### Configuration Files

```
.solhint.json          # Solhint rules
.solhintignore         # Files to ignore
codecov.yml            # Codecov config
LICENSE                # MIT License
```

---

## 🎯 Best Practices

### For Contributors

1. **Before Committing:**
   ```bash
   npm run lint
   npm test
   npm run coverage
   ```

2. **Writing PR:**
   - Clear description
   - Link related issues
   - Update documentation
   - Ensure CI passes

3. **Code Quality:**
   - Follow Solhint rules
   - Add tests for new features
   - Maintain coverage >90%
   - Document complex logic

### For Maintainers

1. **Merging PRs:**
   - All checks must pass
   - Review coverage changes
   - Check gas costs
   - Verify security audit

2. **Releasing:**
   - Tag releases
   - Update changelog
   - Deploy to testnet
   - Verify contracts

---

## 📚 Additional Resources

### Documentation
- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Codecov Documentation](https://docs.codecov.com)
- [Solhint Rules](https://github.com/protofire/solhint/blob/master/docs/rules.md)

### Tools
- [GitHub CLI](https://cli.github.com)
- [Act (Local Actions)](https://github.com/nektos/act)
- [Codecov Browser Extension](https://docs.codecov.com/docs/browser-extension)

---

## ✅ Quick Reference

### Essential Commands

```bash
# Run all CI checks locally
npm run ci

# Lint Solidity
npm run lint

# Run tests
npm test

# Generate coverage
npm run coverage

# Deploy
npm run deploy

# Verify
npm run verify
```

### Workflow Commands

```bash
# List workflows
gh workflow list

# View workflow runs
gh run list

# Watch workflow
gh run watch

# View logs
gh run view --log
```

---

## 🎉 Summary

Your CI/CD pipeline includes:

✅ **4 GitHub Actions workflows**
- Automated testing on multiple Node versions
- Code quality and security checks
- Pull request validation
- Deployment automation

✅ **Codecov integration**
- Automatic coverage reporting
- PR coverage diffs
- Coverage trends

✅ **Solhint linting**
- Automated code quality checks
- Security best practices
- Style enforcement

✅ **Complete automation**
- Tests run on every push/PR
- Automatic deployments
- Security audits
- Coverage tracking

---

**All workflows use English and contain no project-specific naming conventions.**

For support, see [DEPLOYMENT.md](./DEPLOYMENT.md) or [TESTING.md](./TESTING.md).
