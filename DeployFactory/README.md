# DeployFactory Contract Deployment Guide

This guide demonstrates how to deploy a contract factory using raw transaction publishing with the `deploy-deployFactory.sh` script.

## Overview

The `deploy-deployFactory.sh` script provides a clean, readable way to deploy contracts using `cast publish` with pre-deployment checks and formatted bytecode for better maintainability.

## Prerequisites

- **Foundry**: Ensure you have `cast` installed from [Foundry](https://book.getfoundry.sh/getting-started/installation)
- **Network Access**: RPC endpoint access to your target blockchain network
- **Account Balance**: Sufficient native tokens for gas fees

## Usage

### Quick Start

```bash
# Make the script executable
chmod +x deploy-deployFactory.sh

# Run the deployment
./deploy-deployFactory.sh
```

### Script Features

- ✅ **Balance Check**: Verifies deployer account balance before deployment
- ✅ **Code Verification**: Checks existing code at the target address  
- ✅ **Readable Bytecode**: Contract data broken into 64-character chunks
- ✅ **Error Handling**: Clear feedback throughout the process
- ✅ **Raw Transaction**: Uses `cast publish` for direct deployment

## Configuration

### Network Settings

Edit the script to modify network configuration:

```bash
# Set your RPC URL
RPC_URL="https://YOUR_RPC_URL"
```

### Target Address

Update the owner/target address if needed:

```bash
# In the script, modify this address:
cast balance 0xBb6e024b9cFFACB947A71991E386681B1Cd1477D --rpc-url $RPC_URL
cast code 0xBb6e024b9cFFACB947A71991E386681B1Cd1477D --rpc-url $RPC_URL
```

## Script Breakdown

### 1. Pre-deployment Checks

The script performs essential checks before deployment:

```bash
# Check deployer balance
cast balance 0xBb6e024b9cFFACB947A71991E386681B1Cd1477D --rpc-url $RPC_URL

# Verify existing code at address
cast code 0xBb6e024b9cFFACB947A71991E386681B1Cd1477D --rpc-url $RPC_URL
```

### 2. Formatted Bytecode

The contract bytecode is organized in readable 64-character chunks:

```bash
CALLDATA="0xf9016c8085174876e8008303c4d88080b90154608060405234801561"
CALLDATA+="001057600080fd5b50610134806100206000396000f3fe608060405234801560"
CALLDATA+="0f57600080fd5b506004361060285760003560e01c80634af63f0214602d57"
# ... additional chunks
```

### 3. Deployment Execution

```bash
# Deploy using cast publish
cast publish $CALLDATA --rpc-url $RPC_URL
```

## Original Command Reference

For comparison, the original single-line deployment command was:

```bash
cast publish 0xf9016c8085174876e8008303c4d88080b90154608060405234801561001057600080fd5b50610134806100206000396000f3fe6080604052348015600f57600080fd5b506004361060285760003560e01c80634af63f0214602d575b600080fd5b60cf60048036036040811015604157600080fd5b810190602081018135640100000000811115605b57600080fd5b820183602082011115606c57600080fd5b80359060200191846001830284011164010000000083111715608d57600080fd5b91908080601f016020809104026020016040519081016040528093929190818152602001838380828437600092019190915250929550509135925060eb915050565b604080516001600160a01b039092168252519081900360200190f35b6000818351602085016000f5939250505056fea26469706673582212206b44f8a82cb6b156bfcc3dc6aadd6df4eefd204bc928a4397fd15dacf6d5320564736f6c634300060200331b83247000822470 --rpc-url https://YOUR_RPC
```

## Benefits of the Improved Script

### 🎨 **Readability**
- Bytecode split into manageable chunks
- Clear section headers and comments
- Consistent 64-character line lengths

### 🔧 **Maintainability**
- Easy to modify specific parts of bytecode
- Configurable variables for network settings
- Version control friendly (cleaner diffs)

### 🛡️ **Safety**
- Pre-deployment balance checks
- Code verification steps
- Clear error feedback

### 👀 **Transparency**
- Each step is clearly documented
- Easy to understand what's happening
- Suitable for auditing and review

## Troubleshooting

### Common Issues

**Script not executable:**
```bash
chmod +x deploy-deployFactory.sh
```

**RPC connection issues:**
- Verify your RPC URL is correct
- Check network connectivity
- Ensure the RPC endpoint supports the required methods

**Insufficient balance:**
- Check account balance with: `cast balance YOUR_ADDRESS --rpc-url YOUR_RPC`
- Ensure sufficient native tokens for gas fees

**Cast not found:**
- Install Foundry: `curl -L https://foundry.paradigm.xyz | bash`
- Run: `foundryup`

## Security Notes

- ⚠️ **Never commit private keys** to version control
- ⚠️ **Test on testnets** before mainnet deployment  
- ⚠️ **Verify bytecode** matches your intended contract
- ⚠️ **Double-check addresses** and network settings

## Support

For issues or questions related to this deployment script, please check:

1. **Foundry Documentation**: [book.getfoundry.sh](https://book.getfoundry.sh/)
2. **Cast Reference**: [cast publish documentation](https://book.getfoundry.sh/reference/cast/cast-publish)
3. **Network RPC**: Verify your RPC endpoint status

---

**Note**: This script deploys a specific DeployFactory contract. Ensure you understand the contract's functionality before deployment. 