---
sidebar_position: 3
---

# Zk Merkle Tree for EVM

Zero-Knowledge Merkle Trees implementation using circom circuits and snarkjs, designed for EVM chains.

⚠️ **Early Stage Project**: This package is under active development. APIs may change as we improve the implementation.


## Features

- 🌳 ZK Merkle Tree with native EVM integration
- 🌲 Fast off-chain proof generation
- 🎋 On-chain verification
- 🌴 Custom trusted setup support
- 📦 Built on circom & snarkjs

## Installation

```bash
bun add @zkthings/merkle-evm
# or
npm install @zkthings/merkle-evm
```

## Quick Start

```typescript
import { ZkMerkleTree } from '@zkthings/merkle-evm';

// Create a new ZK Merkle Tree
const zkMerkle = new ZkMerkleTree();

// Add data and generate proof
const values = [‘Dragon Tree’, ‘Olive’ , ‘Linden’]

const { proof, publicSignals } = await zkMerkle.generateMerkleProof(
  values,
  'world'
);

// Verify off-chain (for testing)
const isValidOffChain = await zkMerkle.verifyProofOffChain(proof, publicSignals);

// Export and deploy verifier contract
const verifierContract = await zkMerkle.exportVerifierContract();
```

## Production Usage

### Trusted Setup

```typescript
import { PowerOfTau } from '@zkthings/merkle-evm';

// Initialize ceremony
const ceremony = new PowerOfTau(15);  // For trees up to depth 15
const ptauFile = await ceremony.initCeremony();

// Generate production parameters
await ceremony.finalizeCeremony();
await ceremony.finalizeCircuit('MerkleTreeProof');
```

### Production Deployment
```typescript
// Use custom ceremony output
const zkMerkle = new ZkMerkleTree({
  baseDir: './production-zkconfig',
  maxDepth: 20
});

// Deploy verifier contract
const verifierContract = await zkMerkle.exportVerifierContract();
```

## Architecture

```
📦 @zkthings/merkle-evm
├── core/             # Core Merkle Tree implementation
├── circuits/         # Circom circuit definitions
├── contracts/        # Solidity verifier contracts
└── ceremony/         # Trusted setup utilities
```

## Best Practices

### Local Development
```typescript
// Fast local testing
const zkMerkle = new ZkMerkleTree();
const isValid = await zkMerkle.verifyProofOffChain(proof, publicSignals);
```

### Production Setup
```typescript
// Secure production configuration
const zkMerkle = new ZkMerkleTree({
  baseDir: './production-zkconfig',
  maxDepth: 20
});
```

## Security Considerations

1. **Trusted Setup**
   - Multiple participants required
   - Secure randomness for contributions
   - Verify ceremony completion

2. **Contract Deployment**
   - Audit generated verifier
   - Test thoroughly on testnet
   - Monitor gas costs

3. **Verification**
   - Always verify on-chain in production
   - Use off-chain for testing only
   - Validate all proof components

## API Reference

### ZkMerkleTree
```typescript
class ZkMerkleTree {
  constructor(config?: ZkConfig);
  generateMerkleProof(values: string[], valueToProve: string): Promise<ProofData>;
  verifyProofOffChain(proof: Proof, publicSignals: PublicSignals): Promise<boolean>;
  exportVerifierContract(): Promise<string>;
}
```

### PowerOfTau
```typescript
class PowerOfTau {
  constructor(depth: number);
  initCeremony(): Promise<string>;
  finalizeCeremony(): Promise<void>;
  finalizeCircuit(name: string): Promise<void>;
}
```

## Contributing

PRs welcome! Check our [Contributing Guide](https://github.com/zkthings/merkle-evm/blob/main/CONTRIBUTING.md).

## Support

- [Documentation](https://zksdk.io/docs/intro)
- [Discord](https://discord.gg/zkthings)
- [GitHub Issues](https://github.com/zkthings/merkle-evm/issues)

## License

MIT © [zkThings](https://github.com/zkthings)
