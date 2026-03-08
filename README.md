# CUDA AES Basic 128

## Overview
`CUDA_AES_BASIC_128` is a GPU-accelerated implementation of AES-128 encryption.  
The project demonstrates how symmetric-key encryption can be parallelized so many 16-byte data blocks are processed concurrently on a CUDA-capable GPU.

At a high level, the application:
- Initializes a CUDA device.
- Expands a 128-bit key into round keys.
- Launches a CUDA kernel that encrypts data blocks in parallel.
- Validates encryption output with a known AES test vector.
- Measures throughput for different input sizes.

## What AES-128 Is
AES (Advanced Encryption Standard) is a widely used symmetric block cipher standardized by NIST.

Key properties of AES-128:
- Block size: 128 bits (16 bytes).
- Key size: 128 bits (16 bytes).
- Rounds: 10 transformation rounds.

Each block is encrypted through a sequence of operations (such as substitution, shifting, mixing, and round-key addition) that provide confusion and diffusion.  
Because AES is a block cipher, larger messages are processed block by block.

## What This Project Implements
This repository focuses on AES-128 encryption for block data with GPU execution:
- A CUDA kernel performs AES round operations on blocks assigned to GPU threads.
- Host-side code manages memory transfers, kernel launches, and timing.
- A CPU-side key expansion routine generates the expanded key schedule used by the kernel.
- Test-vector verification checks correctness of encrypted output.

## Project Structure
- `aes.cu`: Main program, CUDA kernel code, encryption path, tests, and throughput reporting.
- `aes_core.c`: AES core routines such as key expansion and CPU reference primitives.
- `aes.h`: Shared AES constants and basic types (block/key/round configuration).
- `aes_locl.h`: AES helper types/macros used by the core implementation.
- `aes.sln`, `aes.vcproj`: Project/solution metadata.

## Technology Used
- C/C++ for host-side control flow and AES core logic.
- NVIDIA CUDA for parallel GPU computation.
- CUDA Runtime API for GPU memory management, kernel launches, and synchronization.
- Static AES lookup tables (S-Box and finite-field multiplication tables) for efficient round transformations.

## Why GPU for AES
AES encryption of independent blocks is highly parallelizable. GPUs are well-suited for this model because they execute many threads concurrently, which can significantly improve throughput for large inputs.

## Typical Execution Flow
1. Program starts and verifies a CUDA device is available.
2. AES test vectors are encrypted and compared against expected ciphertext.
3. Input data is encrypted on GPU for increasing sizes.
4. Timing and throughput metrics are printed.
