# EIP-7702 Chain Support Verification

A simple shell script to verify if an Ethereum-compatible blockchain or RPC endpoint supports EIP-7702 functionality.

## Overview

With the introduction of EIP-7702, Ethereum and compatible chains gain a powerful new primitive: the ability for externally owned accounts (EOAs) to temporarily act as smart contract accounts by setting custom code for a transaction. This script helps you determine if a particular chain or RPC endpoint supports EIP-7702 **before** you start building on top of it.

## Why EIP-7702 Matters

EIP-7702 enables EOAs to execute smart contract logic for a single transaction, opening up new possibilities for:

- **Account Abstraction**: Enhanced user experience with smart account features
- **Security**: Temporary code execution without permanent account transformation
- **Flexibility**: Dynamic account behavior based on transaction context

However, not all networks or RPC providers support this feature yet. This verification script helps you check support quickly and reliably.

## Quick Start

```bash
# Make the script executable
chmod +x eip7702-verify.sh

# Run the verification
./eip7702-verify.sh
```

## How It Works

The script uses a clever approach to test EIP-7702 support by sending a custom JSON-RPC request with these key components:

### 1. Test Transaction Structure

```json
{
  "from": "0xdeadbeef00000000000000000000000000000000",
  "to": "0xdeadbeef00000000000000000000000000000000", 
  "data": "0x0000...0001...0000",
  "value": "0x0"
}
```

- **from/to**: Dummy EOA addresses (no funding required)
- **data**: Specific pattern containing r, s, v parameters for ecrecover precompile
- **value**: No ETH transfer required

### 2. EIP-7702 State Override

The critical part is the third parameter - a state override:

```json
{
  "0xdeadbeef00000000000000000000000000000000": {
    "code": "0xef01000000000000000000000000000000000000000001"
  }
}
```

**Breaking down the code field:**
- `0xef01`: Magic prefix indicating EIP-7702 code
- `0x0000000000000000000000000000000000000001`: Address of the ecrecover precompile contract

### 3. Verification Method

The script uses `eth_estimateGas` as a probe:
- **Supported chains**: Return a gas estimate
- **Unsupported chains**: Return an error or fail to process the EIP-7702 semantics

## Configuration

### Changing the Target Network

Edit the `RPC_URL` variable in the script:

```bash
RPC_URL="https://your-rpc-endpoint.com"
```

### Common RPC Endpoints

```bash
# Ethereum Mainnet
RPC_URL="https://eth.llamarpc.com"

# Polygon
RPC_URL="https://polygon-rpc.com"

# Arbitrum
RPC_URL="https://arb1.arbitrum.io/rpc"

# Your custom endpoint
RPC_URL="https://your-custom-rpc.com"
```

## Interpreting Results

### ✅ **EIP-7702 Supported**

Response will include a gas estimate:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x5208"
}
```

### ❌ **EIP-7702 Not Supported**

Common error responses:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid params"
  }
}
```

Or the request may fail entirely if the RPC doesn't understand the state override format.

## Technical Details

### The Test Data Explained

The `data` field contains:

```
0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```

This represents:
- **r parameter**: 32 bytes of zeros
- **s parameter**: 32 bytes with a single `1`
- **v parameter**: 32 bytes of zeros

This simulates parameters for the `ecrecover` precompile, ensuring the transaction contains realistic contract call data.

### Why This Approach Works

1. **Non-invasive**: No actual state changes or gas consumption
2. **Comprehensive**: Tests both RPC understanding and node implementation
3. **Realistic**: Uses actual precompile addresses and valid call data
4. **Fast**: Single HTTP request provides immediate feedback

## Troubleshooting

### Common Issues

**Script not executable:**
```bash
chmod +x eip7702-verify.sh
```

**Network connectivity:**
```bash
# Test basic connectivity
curl -X POST $RPC_URL \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

**RPC endpoint down:**
- Try alternative RPC endpoints for the same network
- Check the provider's status page

### Debugging

Add verbose output to see full response:

```bash
# Add -v flag for verbose output
curl -v --location $RPC_URL \
--header 'Content-Type: application/json' \
--data '{ ... }'
```

## Advanced Usage

### Batch Testing Multiple Networks

Create a simple loop to test multiple endpoints:

```bash
#!/bin/bash

RPCS=(
  "https://eth.llamarpc.com"
  "https://polygon-rpc.com" 
  "https://arb1.arbitrum.io/rpc"
)

for rpc in "${RPCS[@]}"; do
  echo "Testing: $rpc"
  RPC_URL=$rpc ./eip7702-verify.sh
  echo "---"
done
```

### JSON Response Parsing

Use `jq` for cleaner output:

```bash
./eip7702-verify.sh | jq .
```

## Related Resources

- **EIP-7702 Specification**: [EIP-7702 on GitHub](https://eips.ethereum.org/EIPS/eip-7702)
- **Complete Examples**: [7702-set-code-example Repository](https://github.com/jingkang0822/7702-set-code-example.git)
- **Account Abstraction**: [ERC-4337 Documentation](https://eips.ethereum.org/EIPS/eip-4337)

## Security Notes

- ⚠️ This script only **verifies support** - it doesn't perform actual transactions
- ⚠️ The dummy addresses used are safe and require no funding
- ⚠️ Always test on testnets before deploying EIP-7702 functionality to mainnet

## Contributing

Found an issue or have suggestions? Feel free to:

1. Open an issue describing the problem
2. Submit a pull request with improvements
3. Share results from testing different networks

---

**Note**: EIP-7702 is a relatively new feature. Support may vary across different Ethereum clients and layer 2 solutions. Always verify support before building production applications. 