# EIP-7702 Implementation Examples

This repository contains example implementations for working with EIP-7702, specifically focusing on two key functionalities:

## 1. Chain Compatibility Verification (`eip7702-verify.sh`)

A shell script that verifies whether a specified blockchain network supports EIP-7702's set code functionality.

### Usage

```bash
# Make the script executable
chmod +x eip7702-verify.sh

# Run the script
./eip7702-verify.sh
```

The script makes an RPC call to estimate gas for an EIP-7702 set code operation. If the network supports EIP-7702, it will return a gas estimate. If not, it will return an error.

### Configuration

- Modify the `RPC_URL` variable in the script to point to your desired network's RPC endpoint.

## 2. Authorization List Execution (`set-code-and-execute.ts`)

A TypeScript implementation that demonstrates how to:

- Create and sign authorization data according to EIP-712 standard
- Execute transactions with authorization lists
- Handle EIP-7702 specific transaction formatting

### Prerequisites

- Node.js and npm installed
- Hardhat environment set up
- Required environment variables:
  - `DEPLOYER_PRIVATE_KEY`: Private key for transaction signing
  - `CONTRACT_IMPL`: Address of the contract implementation

### Usage

```bash
# Install dependencies
npm install

# Run the script
npx hardhat run set-code-and-execute.ts
```

## Environment Setup

1. Create a `.env` file with the following variables:

```env
DEPLOYER_PRIVATE_KEY=your_private_key_here
CONTRACT_IMPL=your_contract_address_here
```

2. Install required dependencies:

```bash
npm install hardhat @nomicfoundation/hardhat-toolbox ethers
```

## Features

- **Chain Verification**: Quickly test if a network supports EIP-7702
- **Authorization List**: Implements EIP-7702's authorization list functionality
- **Transaction Signing**: Demonstrates proper transaction signing for EIP-7702
- **Gas Estimation**: Includes gas estimation for EIP-7702 operations

## Security Notes

- Never commit your private keys or sensitive credentials
- Always use environment variables for sensitive data
- Test thoroughly on test networks before deploying to mainnet

## Contributing

Feel free to submit issues and enhancement requests!
