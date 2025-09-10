---
hip: 20
title: Blockchain Node Standard
author: Hanzo AI Team
type: Standards Track
category: Core
status: Draft
created: 2025-01-09
requires: HIP-0
---

# HIP-20: Blockchain Node Standard

## Abstract

This proposal defines the blockchain node standard for Hanzo's L2/L1 infrastructure. All nodes MUST implement this interface.

**Repository**: [github.com/luxfi/node](https://github.com/luxfi/node)

## Motivation

We need ONE standard way to:
- Run blockchain nodes
- Validate blocks
- Participate in consensus

## Specification

### Node Configuration

```yaml
# node.yaml
network: mainnet
role: validator
consensus: proof-of-compute

compute:
  gpu: true
  min_memory: 16GB
  models: [jin-nano, hllm-7b]

p2p:
  port: 9651
  max_peers: 256
  
rpc:
  port: 9650
  apis: [eth, net, web3]
```

### Node Interface

```go
type Node interface {
    Start() error
    Stop() error
    
    // Consensus
    ProposeBlock(block *Block) error
    ValidateBlock(block *Block) bool
    
    // Compute
    ProvideCompute(task *ComputeTask) (*ComputeProof, error)
    VerifyCompute(proof *ComputeProof) bool
}
```

### RPC API

```yaml
POST /ext/bc/C/rpc
  Ethereum-compatible JSON-RPC
  
POST /ext/bc/P/rpc
  Platform chain API
  
POST /ext/compute
  Compute task submission
```

## Implementation

Node participates in Proof of Compute consensus:

```
GPU Resources → Node (HIP-20) → PoC Consensus → Block Production
```

## References

1. [HIP-0: Architecture](./hip-0.md)
2. [HIP-1: $AI Token](./hip-1.md)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).