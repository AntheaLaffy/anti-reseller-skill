---
name: anti-reseller
description: "Use when building protection mechanisms for open source software against unauthorized resale. Keywords: anti-piracy, protect open source, prevent resale, copyright protection, license protection, 开源保护, 防止倒卖, 版权保护, 反倒卖"
user-invocable: false
---

# Anti-Reseller Protection

> **Anti-Reseller Protection for Open Source Software**

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

### Pattern 1: Coupled Validation

```rust
// NOT THIS (easily removable):
fn check_license() -> bool {
    validate_copyright() && validate_checksum()
}

// BUT THIS (embedded in core):
fn process_data(input: &str) -> Result<Output> {
    // Copyright string is also a seed for cryptographic operations
    let seed = calculate_seed(COPYRIGHT_NOTICE, input.len());

    // Invalid copyright = invalid seed = wrong calculations = crash
    let processed = core_algorithm(input, seed)?;

    // More validation embedded in multiple places
    verify_integrity(&processed, COPYRIGHT_NOTICE)?;

    Ok(processed)
}
```

### Pattern 2: Distributed Checks

```
Strategy: Embed validation in multiple locations

- Startup check: Verify copyright hash
- Processing check: Use copyright as algorithm input
- Shutdown check: Log usage with copyright watermark
- All checks are interdependent

Result: Remove one → crash. Remove all → code won't compile/run.
```

### Pattern 3: Obfuscated Naming

```rust
// Use mundane names that look like regular business logic:
// - calc_seed(), check_sum(), verify_hash()
// - Not: anti_piracy_check(), license_validation()
//
// This makes it harder for resellers to identify protection code
```

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

## Summary

The anti-reseller protection approach works because:

1. **Embedding > Isolation**: Protection is part of business logic, not a removable module
2. **Crash > Warning**: Removing protection causes crashes, not just warnings
3. **Proof > Prevention**: Even if resold, copyright notice proves origin
4. **Cost > Reward**: Making removal hard makes reselling uneconomical

The three difficulty levels (obscure language → binary → esolang) provide escalating protection based on the threat model and development resources available.
