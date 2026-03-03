---
name: anti-reseller
description: "Use when building protection mechanisms for open source software against unauthorized resale. Triggers: anti-piracy, protect open source, prevent resale, copyright protection, license protection, software protection, prevent copying, 反倒卖, 开源保护, 版权保护, 防止倒卖, GPL violation, unauthorized sale"
user-invocable: true
argument-hint: "[project-path]"
---

# Anti-Reseller Protection

> **Anti-Reseller Protection for Open Source Software**
>
> A skill for implementing copyright-coupled protection in open source projects.

## Problem Statement

Open source software can be easily resold by unauthorized parties who:
1. Download the source code
2. Remove copyright notices and open source links
3. Compile and sell on platforms like Taobao/Xianyu

## Core Design Principle

```
PRINCIPLE: Copyright declaration + critical parameters = coupled bundle

If someone removes the copyright notice → critical parameters corrupted → program crashes
If someone keeps the copyright notice → proof of origin remains
```

### Key Insight

- **NOT** a separate protection module (easily removable)
- **IS** embedded in core algorithm (must be preserved to work)
- **Traditional approach fails**: Checksums can be removed; protection code can be deleted
- **This approach succeeds**: Protection is part of business logic, not add-on

---

## Three Difficulty Levels

### Level 1: Obscure Language (Beginner)

```
Language: Haskell, Erlang, OCaml, F#, etc.
Cost: Low - requires learning a functional language
Effectiveness: Medium - readable but unfamiliar to most resellers
```

**Idea**: Write protection logic in a niche functional language
- Resellers likely don't understand functional programming
- Adds "bragging rights" for the developer
- Still compilable to binary if needed

### Level 2: Binary Compilation (Intermediate)

```
Language: Rust, C++, Nim
Cost: Medium - requires binary compilation
Effectiveness: High - requires reverse engineering to bypass
```

**Idea**: Compile protection logic to binary, link statically
- Harder to analyze than source code
- Use aggressive obfuscation if needed
- Resellers need debugging skills to modify

### Level 3: Esolang (Advanced)

```
Language: Brainfuck, Befunge, Piet, INTERCAL, etc.
Cost: High - esoteric syntax, hard to maintain
Effectiveness: Very High - reverse engineering measured in YEARS

Note: Even elite reverse engineers avoid esolang code.
The mental overhead of parsing esolang makes analysis impractical.
```

**Idea**: Write protection logic in an esoteric programming language
- Brainfuck: 8 commands, extremely hard to understand
- Piet: Visual programming, image-based
- Befunge: 2D execution model
- The protection becomes nearly impossible to remove

---

## Implementation Patterns

### Pattern 1: Coupled Validation (Enhanced)

The `calculate_seed` concept is solid, but to truly defend against simple patches (like someone patching `core_algorithm` to ignore the seed), **use the seed as an encryption key for necessary configuration files or constants**, not just a procedural check.

```rust
// NOT THIS (easily removable):
fn check_license() -> bool {
    validate_copyright() && validate_checksum()
}

// BUT THIS (seed as encryption key):
fn process_data(input: &str) -> Result<Output> {
    // Derive encryption key from copyright
    let key = derive_key(COPYRIGHT_NOTICE);

    // Decrypt critical config using copyright-derived key
    // If copyright is wrong → wrong key → decryption fails → garbage config
    let config = decrypt_config(ENCRYPTED_CONFIG, key)?;

    // Use decrypted config in core algorithm
    let processed = core_algorithm(input, &config)?;

    // Verify output integrity with copyright
    verify_integrity(&processed, COPYRIGHT_NOTICE)?;

    Ok(processed)
}
```

**Why this is stronger**: The reseller cannot simply patch the algorithm to ignore the seed—they would need the correct key to decrypt the configuration that the algorithm depends on.

### Pattern 2: Distributed State Checks

Make checks interdependent so they cannot be removed individually.

```
Strategy: Startup → Processing → Shutdown pipeline

- Startup check: Generate state vector from copyright hash
  └── Produces: decode_key, verify_vector, session_salt

- Processing check: Use state vector to decode function pointers
  └── decode_key decrypts critical function addresses
  └── verify_vector validates runtime integrity
  └── session_salt mixes into output

- Shutdown check: Log with watermark using session_salt
  └── Produces: traceable usage record

Result:
- Remove startup → no decode_key → crash in processing
- Remove processing → no verified functions → undefined behavior
- Remove shutdown → no traceable record
- Patch any single point → other checks fail
```

### Pattern 3: Obfuscated String Reconstruction (NEW)

**Important Vulnerability**: Seasoned reverse engineers don't look at function names first (especially after symbol stripping); they look at data flow from the copyright string. If a reseller simply hex-edits "Author: X" to "Author: Y", they don't care what the validation function is named.

**Solution**: Obfuscate the copyright string itself in memory.

```rust
// NOT THIS (vulnerable to hex-edit):
const COPYRIGHT: &str = "Copyright (c) 2024 Author Name";

// BUT THIS (reconstructed at runtime):
fn reconstruct_copyright() -> String {
    // Split into chunks, XOR with key, reconstruct
    let chunk1 = [0x43, 0x6F, 0x70, 0x79]; // "Copy"
    let chunk2 = [0x72, 0x69, 0x67, 0x68]; // "righ"
    let chunk3 = [0x74, 0x20, 0x28, 0x63]; // "t (c"

    let key = 0x42; // Simple XOR key

    let reconstructed: String = chunk1.iter()
        .chain(chunk2.iter())
        .chain(chunk3.iter())
        .map(|&b| (b ^ key) as char)
        .collect();

    reconstructed
}

// Usage: Call at runtime, not stored as plaintext
fn validate() -> bool {
    let copyright = reconstruct_copyright();
    // Now use copyright for validation...
    copyright.contains("Author")
}
```

**Even better**: Derive different parts from different sources:
- Part 1: Compile-time constant
- Part 2: Runtime computation from build metadata
- Part 3: Obfuscated bytes decoded at startup

---

## Protection Workflow

```
1. Developer implements protection
   ├── Embed copyright in core algorithm
   ├── Use Level 1/2/3 based on threat model
   └── Add multiple validation points

2. Software released as open source
   ├── Source code publicly available
   ├── Copyright notices embedded
   └── Binary available for convenience

3. Reseller attempts to remove protection
   ├── Cannot easily identify protection code
   ├── Removal causes crashes
   └── Keeping copyright = evidence of origin

4. Users can verify origin
   ├── See copyright notice in binary/UI
   ├── Know original author
   └── Can provide evidence if resold
```

---

## Legal Considerations

| Protection Type | Legal Weight |
|-----------------|--------------|
| GPL License | Strong - derivative works must be open source |
| Copyright Notice | Medium - establishes authorship |
| EULA/ToS | Weak for free software, stronger for paid |
| Embedded Trademarks | Medium - trademark infringement |

**Recommendation**: Use GPL v3 + copyright notices + trademark
- GPL violations are legally actionable
- Copyright provides authorship proof
- Trademark protects brand identity

---

## When to Use This Skill

This skill applies when:
- Building open source software that may be commercialized
- Wanting to prevent unauthorized resale
- Looking for ways to prove origin of derivative works
- Need to embed protection in core logic, not as add-on

---

## Related Skills

| Context | See |
|---------|-----|
| Rust implementation | m05-type-driven |
| Performance constraints | m10-performance |
| Security considerations | m09-domain |
| Code obfuscation | m15-anti-pattern |

---

---

## Quick Start

### Invoking the Skill

```bash
# Analyze a project for anti-reseller protection
/anti-reseller ./my-project

# Get recommendations for new project
/anti-reseller
```

### What This Skill Provides

1. **Threat Assessment**: Evaluate your project's vulnerability to resale
2. **Protection Strategy**: Recommend difficulty level (1-3) based on your needs
3. **Implementation Patterns**: Provide code patterns that couple copyright with functionality
4. **Legal Guidance**: Explain legal protection layers (GPL, copyright, trademark)

---

## Detailed Implementation Examples

### Rust Implementation (Level 2: Binary)

```rust
// Define copyright as OBFUSCATED bytes (not plaintext)
const COPYRIGHT_CHUNKS: [[u8; 4]; 4] = [
    [0x43, 0x6F, 0x70, 0x79], // "Copy"
    [0x72, 0x69, 0x67, 0x68], // "righ"
    [0x74, 0x20, 0x28, 0x63], // "t (c"
    [0x29, 0x20, 0x32, 0x30], // ") 20"
];

const XOR_KEY: u8 = 0x42;

// Reconstruct copyright at runtime (not stored as plaintext)
fn reconstruct_copyright() -> String {
    COPYRIGHT_CHUNKS.iter()
        .flatten()
        .map(|&b| (b ^ XOR_KEY) as char)
        .collect()
}

// Derive encryption key from copyright
fn derive_key(copyright: &str) -> [u8; 32] {
    use std::collections::hash_map::DefaultHasher;
    use std::hash::{Hash, Hasher};

    let mut hasher = DefaultHasher::new();
    copyright.hash(&mut hasher);
    let hash = hasher.finish();

    // Expand to 256-bit key
    let mut key = [0u8; 32];
    for (i, chunk) in key.chunks_mut(8).enumerate() {
        let mut h = DefaultHasher::new();
        hash.hash(&mut h);
        (i as u64).hash(&mut h);
        let v = h.finish();
        chunk.copy_from_slice(&v.to_le_bytes());
    }
    key
}

// Simulated encrypted config (in reality, embed encrypted data)
const ENCRYPTED_MAGIC: &[u8] = &[0xAB, 0xCD, 0xEF, 0x12, 0x34, 0x56, 0x78, 0x90];

// Decrypt using copyright-derived key
fn decrypt_with_key(data: &[u8], key: &[u8]) -> Result<Vec<u8>, &'static str> {
    // Simplified: XOR with key expansion
    if data.len() < 8 {
        return Err("Invalid config");
    }

    let result: Vec<u8> = data.iter()
        .enumerate()
        .map(|(i, &b)| b ^ key[i % key.len()])
        .collect();

    // Verify magic bytes after "decryption"
    if &result[0..4] != b"MAG1" {
        return Err("Config decryption failed - invalid copyright");
    }

    Ok(result)
}

// Core algorithm that REQUIRES valid copyright to decrypt config
fn process_data(input: &[u8], config: &[u8]) -> Vec<u8> {
    // Use decrypted config in processing
    let salt = u64::from_le_bytes(config[4..12].try_into().unwrap());

    input.iter()
        .enumerate()
        .map(|(i, &b)| b.wrapping_add(((salt >> (i % 8)) & 0xFF) as u8))
        .collect()
}

// Main entry point - copyright is essential for decryption
pub fn process_with_copyright(input: &[u8]) -> Result<Vec<u8>, &'static str> {
    // Step 1: Reconstruct copyright at runtime
    let copyright = reconstruct_copyright();

    // Step 2: Derive key from copyright
    let key = derive_key(&copyright);

    // Step 3: Decrypt config (fails if copyright is wrong)
    let config = decrypt_with_key(ENCRYPTED_MAGIC, &key)?;

    // Step 4: Process with decrypted config
    let processed = process_data(input, &config);

    // Step 5: Verify output
    if processed.is_empty() {
        return Err("Processing failed");
    }

    Ok(processed)
}
```

### Type-Level Protection (Advanced)

```rust
use std::marker::PhantomData;

// Copyright as a type parameter - impossible to bypass
pub struct Licensed<T, C: Copyright> {
    data: T,
    _marker: PhantomData<C>,
}

pub trait Copyright {
    const NOTICE: &'static str;
}

pub struct MyProjectCopyright;
impl Copyright for MyProjectCopyright {
    const NOTICE: &'static str = "Copyright (c) 2024 Your Name";
}

// Function requires valid copyright type
pub fn process<T, C: Copyright>(licensed: Licensed<T, C>) -> T {
    // Only works if proper Copyright type is used
    licensed.data
}
```

### Multi-Language Protection (Level 1: Haskell)

Haskell's declarative nature is fundamentally different from the imperative thinking that resellers are familiar with. Even without obfuscation, the logic flow is extremely difficult to follow.

**Logic Design**: The main program passes the copyright notice to Haskell, which returns a校验值 (verification value) after complex transformations. If the notice is modified, the value won't match, causing the core algorithm to compute incorrect results.

```haskell
-- SecurityModule.hs
{-# LANGUAGE ForeignFunctionInterface #-}

module SecurityModule where

import Foreign.C.String
import Data.Char (ord)
import Data.Bits (xor)

-- Core logic: Transform copyright string into a logical seed
-- Resellers will find this recursion and mapping extremely confusing
calculateSeed :: String -> Int -> Int
calculateSeed [] acc = acc
calculateSeed (x:xs) acc = calculateSeed xs (acc `xor` (ord x * length xs))

-- Export to C/Go via FFI
foreign export ccall "get_logic_seed" getLogicSeed :: CString -> IO Int

getLogicSeed :: CString -> IO Int
getLogicSeed cstr = do
    str <- peekCString cstr
    -- 0xABCDE is our preset bias; if notice is changed, result won't match
    return $ calculateSeed str 0xABCDE
```

**Defense Point**: Haskell compiled binary contains lazy evaluation logic. After disassembly, it's nearly impossible to crack the logic through simple `strings` replacement.

---

### Level 3: Brainfuck (Esolang Ultimate Moat)

This is the "ultimate weapon" in your strategy. Even top reverse engineers will feel extreme psychological burden facing Brainfuck code.

**Logic Design**: Embed a tiny BF interpreter in your C/Rust main program. Pass the copyright notice to the BF script for processing.

```c
// Part of the main program
// This BF code's purpose: Read the first character of copyright notice,
// perform complex bit shifts, finally output a "logic startup key" for decrypting core logic.

const char* esolang_logic =
    ">+[<+>-]>++++++++[<++++++>-]<.>++++[<++++++>-]<..>++++[<++++++>-]<"
    "++++++++++++.------------.++++++.------.--------.>++++++[<++++++>-]"
    "<++.>++++++[<++++++>-]<+++++++.----.>++++++[<++++++>-]<+++++++.---"
    "-----.++++++++.------------.>++++++[<++++++>-]<+++++++.--------.+++";

// Only when passing correct "Copyright 2026..." string,
// will the correct "logic startup parameter" appear at specific memory location
// after BF execution.
run_bf_interpreter(esolang_logic, COPYRIGHT_NOTICE);
```

**Defense Points**:

- **Cannot be located via search**: BF has no `if` or `switch`, only `[` and `]`. IDA's control flow graph (CFG) will look like a mess.
- **Modification causes crash**: If reseller deletes even one `+` or `-`, the entire "tape" pointer shifts, causing the program to access illegal memory and crash.

---

### Mini Brainfuck Interpreter (Ready to Embed)

Here's a ~50 line ultra-mini Brainfuck interpreter in C that you can directly embed:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define TAPE_SIZE 30000

int run_bf(const char* code, const char* input) {
    unsigned char tape[TAPE_SIZE] = {0};
    int ptr = 0;
    int ip = 0;  // instruction pointer
    int ip_len = strlen(code);
    int input_pos = 0;

    while (ip < ip_len) {
        char op = code[ip];
        switch (op) {
            case '>': if (++ptr >= TAPE_SIZE) ptr = 0; break;
            case '<': if (--ptr < 0) ptr = TAPE_SIZE - 1; break;
            case '+': tape[ptr]++; break;
            case '-': tape[ptr]--; break;
            case '.': putchar(tape[ptr]); break;
            case ',': tape[ptr] = input ? input[input_pos++] : 0; break;
            case '[':
                if (!tape[ptr]) {
                    int depth = 1;
                    while (depth && ++ip < ip_len) {
                        if (code[ip] == '[') depth++;
                        if (code[ip] == ']') depth--;
                    }
                }
                break;
            case ']':
                if (tape[ptr]) {
                    int depth = 1;
                    while (depth && --ip >= 0) {
                        if (code[ip] == ']') depth++;
                        if (code[ip] == '[') depth--;
                    }
                }
                break;
        }
        ip++;
    }
    return tape[0];  // Return first cell as exit code
}

// Usage:
// run_bf(">+[<+>-]>.[,]", "Copyright 2026");
// Returns: verification result in tape[0]
```

**Integration with Protection**:

```c
// Embedded BF script that validates copyright
const char* validation_script =
    ",>,>,>,>,>,>,>,>"    // Read 8 chars of copyright
    "[<+>-]"              // XOR accumulation loop
    "<+";                 // Add 1 to result

int validate_copyright(const char* notice) {
    int result = run_bf(validation_script, notice);
    // Expected: if notice is correct, result matches expected value
    return result == 0x42;  // Expected verification constant
}
```

---

### Combined Defense Strategy

Combine both approaches for maximum effectiveness:

1. **Startup (Haskell)**: Call Haskell-compiled `.a` library, verify copyright Hash
2. **Runtime (Brainfuck)**: Before core algorithm execution, use embedded BF script to dynamically generate a decryption key from copyright, decrypt remaining business logic

**Summary**:
- **Friendly to researchers**: Source visible, even interpreter source visible
- **Deadly to resellers**: They can't see "plaintext" validation logic; modifying copyright causes errors in deep runtime logic they can't fix

This is exactly what you pursue: *"Since you want to resell my code, you become my free promoter."*

---

## Protection Checklist

When implementing anti-reseller protection:

- [ ] Define copyright constant at crate root
- [ ] **DO NOT store copyright as plaintext** - use runtime reconstruction
- [ ] Use copyright-derived key to encrypt/decrypt critical config
- [ ] Embed validation in 3+ locations (startup, process, shutdown)
- [ ] Make checks interdependent (state vector between stages)
- [ ] Use mundane function names (not "anti_piracy")
- [ ] Strip symbols from binary distribution
- [ ] Consider static linking for binary distribution
- [ ] Add GPL license file with proper headers
- [ ] Register trademark if applicable

---

## Summary

The anti-reseller protection approach works because:

1. **Embedding > Isolation**: Protection is part of business logic, not a removable module
2. **Crash > Warning**: Removing protection causes crashes, not just warnings
3. **Proof > Prevention**: Even if resold, copyright notice proves origin
4. **Cost > Reward**: Making removal hard makes reselling uneconomical

The three difficulty levels (obscure language → binary → esolang) provide escalating protection based on the threat model and development resources available.
