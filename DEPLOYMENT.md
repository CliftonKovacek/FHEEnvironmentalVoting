# EnvironmentalVoting - Deployment Guide

Complete guide for deploying and managing the EnvironmentalVoting smart contract using Hardhat.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Compilation](#compilation)
- [Testing](#testing)
- [Deployment](#deployment)
- [Verification](#verification)
- [Interaction](#interaction)
- [Simulation](#simulation)
- [Network Information](#network-information)
- [Troubleshooting](#troubleshooting)

---

## 🔧 Prerequisites

Before deploying, ensure you have:

- **Node.js**: v18.0.0 or higher
- **npm**: v9.0.0 or higher
- **Git**: For version control
- **Wallet**: MetaMask or similar with testnet ETH
- **API Keys**:
  - Infura or Alchemy account (for RPC access)
  - Etherscan account (for contract verification)

### Get Testnet ETH

For Sepolia deployment, get free test ETH from:
- [Alchemy Sepolia Faucet](https://sepoliafaucet.com/)
- [Infura Sepolia Faucet](https://www.infura.io/faucet/sepolia)
- [Chainlink Sepolia Faucet](https://faucets.chain.link/sepolia)

---

## 📦 Installation

### 1. Clone and Install Dependencies

```bash
# Navigate to project directory
cd /d/

# Install dependencies
npm install
```

### 2. Verify Installation

```bash
# Check Hardhat installation
npx hardhat --version

# List available tasks
npx hardhat help
```

---

## ⚙️ Configuration

### 1. Create Environment File

```bash
# Copy example environment file
cp .env.example .env
```

### 2. Configure Environment Variables

Edit `.env` file with your credentials:

```env
# Sepolia RPC URL
SEPOLIA_RPC_URL=https://sepolia.infura.io/v3/YOUR-PROJECT-ID

# Your wallet private key (KEEP SECRET!)
PRIVATE_KEY=your-private-key-here

# Etherscan API key for verification
ETHERSCAN_API_KEY=your-etherscan-api-key-here

# Optional: Gas reporting
REPORT_GAS=true
COINMARKETCAP_API_KEY=your-coinmarketcap-api-key
```

### 3. Get Required API Keys

#### Infura/Alchemy RPC URL
1. Create account at [Infura](https://infura.io) or [Alchemy](https://alchemy.com)
2. Create new project
3. Copy project ID and create RPC URL:
   - Infura: `https://sepolia.infura.io/v3/YOUR-PROJECT-ID`
   - Alchemy: `https://eth-sepolia.g.alchemy.com/v2/YOUR-API-KEY`

#### Etherscan API Key
1. Create account at [Etherscan](https://etherscan.io)
2. Go to [API Keys](https://etherscan.io/myapikey)
3. Create new API key
4. Copy key to `.env` file

#### Export Private Key (MetaMask)
1. Open MetaMask
2. Click account menu → Account Details
3. Click "Export Private Key"
4. Enter password and copy key
5. Add `0x` prefix if not present
6. Paste into `.env` file

**⚠️ SECURITY WARNING**: Never share or commit your private key!

---

## 🔨 Compilation

### Compile Contracts

```bash
# Compile all contracts
npm run compile

# Alternative
npx hardhat compile
```

### Check Contract Size

```bash
# Enable contract sizer
CONTRACT_SIZER=true npm run compile

# Or add to .env file
echo "CONTRACT_SIZER=true" >> .env
```

### Clean Build Artifacts

```bash
# Clean compiled artifacts
npm run clean
```

---

## 🧪 Testing

### Run Tests

```bash
# Run all tests
npm test

# Run with gas reporting
REPORT_GAS=true npm test

# Generate coverage report
npm run coverage
```

### Local Development Node

```bash
# Start local Hardhat node
npm run node

# In another terminal, deploy to local node
npm run deploy:local
```

---

## 🚀 Deployment

### Deploy to Sepolia Testnet

```bash
# Deploy to Sepolia
npm run deploy

# Alternative with explicit network
npx hardhat run scripts/deploy.js --network sepolia
```

### Deploy to Local Network

```bash
# Terminal 1: Start local node
npm run node

# Terminal 2: Deploy
npm run deploy:local
```

### Deployment Output

The deployment script will:
1. ✅ Check deployer balance
2. ⛽ Estimate gas costs
3. 🔨 Deploy contract
4. 💾 Save deployment info to `deployments/` directory
5. 📊 Display contract address and transaction details

Example output:
```
🚀 Starting deployment process...

📡 Deploying to network: sepolia
🆔 Chain ID: 11155111

👤 Deployer address: 0x1234...5678
💰 Deployer balance: 1.5 ETH

⛽ Estimating deployment gas...
📊 Estimated gas: 2,500,000

🔨 Deploying EnvironmentalVoting contract...

✅ Contract deployed successfully!
📍 Contract address: 0xabcd...ef01
⏱️  Deployment time: 12.5s

🔍 Etherscan: https://sepolia.etherscan.io/address/0xabcd...ef01
📜 Verify contract: npm run verify
```

### Deployment Files

After deployment, check:
- `deployments/sepolia-deployment.json` - Deployment information
- Contract address and network details

---

## ✅ Verification

### Verify on Etherscan

After deployment completes:

```bash
# Verify contract
npm run verify

# Alternative with explicit network
npx hardhat run scripts/verify.js --network sepolia
```

### Manual Verification

If automatic verification fails:

1. Go to [Sepolia Etherscan](https://sepolia.etherscan.io)
2. Navigate to your contract address
3. Click "Contract" → "Verify and Publish"
4. Select:
   - Compiler: v0.8.24
   - License: MIT
   - Optimization: Yes, 200 runs
5. Paste contract code and verify

### Verification Output

```
🔍 Starting contract verification process...

📡 Network: sepolia
🆔 Chain ID: 11155111

📂 Loading deployment info from: deployments/sepolia-deployment.json
📍 Contract address: 0xabcd...ef01
📋 Constructor args: None

🔨 Verifying contract on Etherscan...
⏳ This may take a few moments...

✅ Contract verified successfully!

🔍 View on Etherscan: https://sepolia.etherscan.io/address/0xabcd...ef01
```

---

## 💬 Interaction

### Interactive CLI

```bash
# Start interactive session
npm run interact

# Alternative with explicit network
npx hardhat run scripts/interact.js --network sepolia
```

### Available Actions

The interactive script provides:

1. **Create new proposal** (admin only)
   - Set title, description, and duration
   - Returns proposal ID

2. **View all proposals**
   - Lists all created proposals
   - Shows status and details

3. **View specific proposal**
   - Detailed proposal information
   - Vote counts and timing

4. **Submit vote**
   - Cast encrypted vote (requires FHEVM)
   - Yes/No voting

5. **Check if you voted**
   - Verify your voting status
   - View timestamp if voted

6. **Reveal proposal results** (admin only)
   - Decrypt and reveal vote totals
   - Requires FHEVM gateway access

7. **End proposal** (admin only)
   - Manually end active proposal
   - Prevents further voting

8. **Get contract info**
   - View admin address
   - Total proposals count

### Example Interaction Session

```
🗳️  EnvironmentalVoting Contract Interaction Tool

==============================================================

📡 Network: sepolia
🆔 Chain ID: 11155111

📍 Contract address: 0xabcd...ef01

👤 Your address: 0x1234...5678
💰 Your balance: 1.2 ETH

🔑 Admin address: 0x1234...5678
✅ You are the admin

📋 Available Actions:
   1. Create new proposal (admin only)
   2. View all proposals
   3. View specific proposal
   4. Submit vote
   5. Check if you voted
   6. Reveal proposal results (admin only)
   7. End proposal (admin only)
   8. Get contract info
   0. Exit

Select an action (0-8):
```

---

## 🎭 Simulation

### Run Complete Simulation

```bash
# Simulate full voting workflow
npm run simulate

# Alternative
npx hardhat run scripts/simulate.js --network sepolia
```

### Simulation Steps

The simulation script:

1. 📝 Creates multiple environmental proposals
2. 👥 Simulates multiple voters
3. 🗳️ Demonstrates voting workflow (structure only)
4. 📊 Displays proposal status
5. 💾 Generates detailed report

### Simulation Output

```
🎭 EnvironmentalVoting Simulation

======================================================================

📡 Network: sepolia
🆔 Chain ID: 11155111

👥 Available accounts: 10

📋 Simulation Setup:
   Admin: 0x1234...5678
   Voters: 5
      Voter 1: 0xaaaa...bbbb
      Voter 2: 0xbbbb...cccc
      Voter 3: 0xcccc...dddd
      Voter 4: 0xdddd...eeee
      Voter 5: 0xeeee...ffff

======================================================================

🎯 STEP 1: Creating Environmental Proposals

📝 Creating Proposal 1: "Ban Single-Use Plastics in City Parks"
   Duration: 7 days
   ✅ Created! Proposal ID: 1
   📝 TX: 0x1111...2222

📝 Creating Proposal 2: "Expand Urban Green Spaces"
   Duration: 10 days
   ✅ Created! Proposal ID: 2
   📝 TX: 0x3333...4444

📝 Creating Proposal 3: "Mandatory Solar Panels for New Buildings"
   Duration: 14 days
   ✅ Created! Proposal ID: 3
   📝 TX: 0x5555...6666

✅ Created 3 proposals

======================================================================

✅ SIMULATION COMPLETE

📋 Summary:
   Contract: 0xabcd...ef01
   Proposals Created: 3
   Voters: 5
   Network: sepolia

📖 Next Steps:
   1. Review simulation report in reports/ directory
   2. Test with frontend application
   3. Implement FHEVM encryption for actual voting
   4. Test proposal lifecycle (vote → reveal → end)
```

### Simulation Reports

Reports are saved to `reports/` directory:
- `simulation-sepolia-YYYY-MM-DD-HH-mm-ss.json`
- Contains full simulation data
- JSON format for easy parsing

---

## 🌐 Network Information

### Sepolia Testnet

**Network Details:**
- **Name**: Sepolia
- **Chain ID**: 11155111
- **Currency**: SepoliaETH (Test ETH)
- **Block Explorer**: https://sepolia.etherscan.io
- **RPC Endpoints**:
  - Infura: `https://sepolia.infura.io/v3/YOUR-PROJECT-ID`
  - Alchemy: `https://eth-sepolia.g.alchemy.com/v2/YOUR-API-KEY`
  - Public: `https://rpc.sepolia.org`

**Network Characteristics:**
- Proof of Stake (PoS) testnet
- Merge-ready
- Long-term support
- Recommended for testing

### Contract Deployment Information

After deployment, your contract will have:

**Contract Address**:
```
https://sepolia.etherscan.io/address/YOUR-CONTRACT-ADDRESS
```

**Transaction Hash**:
```
https://sepolia.etherscan.io/tx/YOUR-TX-HASH
```

**Contract ABI**: Available in `artifacts/contracts/EnvironmentalVoting.sol/EnvironmentalVoting.json`

---

## 🔍 Etherscan Links

After deployment and verification, you can:

### View Contract
```
https://sepolia.etherscan.io/address/YOUR-CONTRACT-ADDRESS
```

### Read Contract Functions
```
https://sepolia.etherscan.io/address/YOUR-CONTRACT-ADDRESS#readContract
```

### Write Contract Functions
```
https://sepolia.etherscan.io/address/YOUR-CONTRACT-ADDRESS#writeContract
```

### View Transactions
```
https://sepolia.etherscan.io/address/YOUR-CONTRACT-ADDRESS#transactions
```

### View Events
```
https://sepolia.etherscan.io/address/YOUR-CONTRACT-ADDRESS#events
```

---

## 🐛 Troubleshooting

### Common Issues

#### 1. "Insufficient funds for gas"

**Problem**: Deployer account has no ETH

**Solution**:
```bash
# Get testnet ETH from faucet
# Check balance
npx hardhat run --network sepolia scripts/check-balance.js
```

#### 2. "Cannot find module 'dotenv'"

**Problem**: Dependencies not installed

**Solution**:
```bash
npm install
```

#### 3. "Invalid API Key"

**Problem**: Etherscan API key invalid or missing

**Solution**:
- Verify `.env` has correct `ETHERSCAN_API_KEY`
- Check key at https://etherscan.io/myapikey
- Regenerate if needed

#### 4. "Network connection timeout"

**Problem**: RPC endpoint not responding

**Solution**:
- Check `SEPOLIA_RPC_URL` in `.env`
- Try alternative RPC provider
- Check internet connection

#### 5. "Contract verification failed"

**Problem**: Verification parameters incorrect

**Solution**:
```bash
# Wait 1-2 minutes after deployment
# Try verification again
npm run verify

# If still failing, check:
# - Constructor arguments match deployment
# - Compiler version is 0.8.24
# - Optimization enabled with 200 runs
```

#### 6. "Transaction underpriced"

**Problem**: Gas price too low

**Solution**:
```bash
# Check current gas prices
# https://sepolia.etherscan.io/gastracker

# Increase gas in hardhat.config.js or wait for lower prices
```

### Debug Mode

Enable detailed logging:

```bash
# Set Hardhat logging level
export HARDHAT_VERBOSE=true

# Run with debug output
DEBUG=hardhat:* npm run deploy
```

### Reset Deployment

If deployment gets stuck:

```bash
# 1. Clean artifacts
npm run clean

# 2. Delete deployment files
rm -rf deployments/sepolia-deployment.json

# 3. Recompile
npm run compile

# 4. Deploy again
npm run deploy
```

---

## 📊 Gas Optimization

### Estimate Gas Costs

Before deployment, estimate costs:

```bash
# Enable gas reporting
REPORT_GAS=true npm test

# Check contract sizes
CONTRACT_SIZER=true npm run compile
```

### Typical Gas Costs (Sepolia)

| Operation | Estimated Gas | Est. Cost (50 gwei) |
|-----------|--------------|---------------------|
| Deploy Contract | ~2,500,000 | ~0.125 ETH |
| Create Proposal | ~200,000 | ~0.01 ETH |
| Vote | ~100,000 | ~0.005 ETH |
| Reveal Results | ~150,000 | ~0.0075 ETH |
| End Proposal | ~50,000 | ~0.0025 ETH |

*Note: Costs vary based on network congestion*

---

## 🔐 Security Best Practices

### Private Key Management

1. **Never commit `.env` file**
   - Already in `.gitignore`
   - Double-check before pushing

2. **Use separate wallets**
   - Testnet wallet for development
   - Mainnet wallet for production
   - Hardware wallet for large amounts

3. **Rotate keys regularly**
   - Generate new keys periodically
   - Update all services using old keys

### API Key Security

1. **Limit API key permissions**
   - Only enable required features
   - Set rate limits if available

2. **Regenerate if exposed**
   - Immediately regenerate compromised keys
   - Update all services

### Smart Contract Security

1. **Audit before mainnet**
   - Professional security audit
   - Community review
   - Bug bounty program

2. **Test thoroughly**
   - Comprehensive test coverage
   - Testnet deployment first
   - Gradual mainnet rollout

---

## 📚 Additional Resources

### Documentation
- [Hardhat Docs](https://hardhat.org/docs)
- [Ethers.js Docs](https://docs.ethers.org/v6/)
- [FHEVM Documentation](https://docs.zama.ai/fhevm)

### Tools
- [Remix IDE](https://remix.ethereum.org) - Online Solidity IDE
- [Tenderly](https://tenderly.co) - Smart contract monitoring
- [OpenZeppelin Defender](https://defender.openzeppelin.com) - Contract administration

### Support
- [Hardhat Discord](https://discord.gg/hardhat)
- [Ethereum Stack Exchange](https://ethereum.stackexchange.com)
- [Zama Community](https://community.zama.ai)

---

## 📝 License

This project is licensed under the MIT License.

---

**Happy Deploying! 🚀**

For issues or questions, please check the troubleshooting section or contact support.
