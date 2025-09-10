---
hip: 19
title: Tensor Operations Standard
author: Hanzo AI Team
type: Standards Track
category: Core
status: Draft
created: 2025-01-09
---

# HIP-19: Tensor Operations Standard

## Abstract

This proposal defines the tensor operations standard for ML computations in Rust. All ML operations MUST use this interface.

**Repository**: [github.com/hanzoai/candle](https://github.com/hanzoai/candle)

## Motivation

We need ONE standard way to:
- Perform tensor operations
- Run ML inference in Rust
- Optimize for different hardware

## Specification

### Tensor Interface

```rust
pub trait Tensor {
    fn shape(&self) -> &[usize];
    fn dtype(&self) -> DType;
    fn device(&self) -> &Device;
    
    // Operations
    fn matmul(&self, other: &Self) -> Result<Self>;
    fn add(&self, other: &Self) -> Result<Self>;
    fn softmax(&self, dim: i64) -> Result<Self>;
}
```

### Model Loading

```rust
pub trait Model {
    fn load(path: &Path) -> Result<Self>;
    fn forward(&self, input: &Tensor) -> Result<Tensor>;
    fn to_device(&mut self, device: Device) -> Result<()>;
}
```

### Device Abstraction

```rust
pub enum Device {
    Cpu,
    Cuda(usize),
    Metal,
}
```

## Implementation

Candle provides backend for Jin models:

```
Jin Model (HIP-3) → Candle (HIP-19) → Hardware (CPU/GPU)
```

## References

1. [HIP-3: Jin Architecture](./hip-3.md)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).