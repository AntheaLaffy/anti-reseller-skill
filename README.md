# anti-reseller-skill

> Claude Code Skill for Anti-Reseller Protection in Open Source Software

[English](./README.md) | [中文](./README-zh.md)

---

## Overview

This is a Claude Code skill that helps developers implement protection mechanisms for open source software against unauthorized resale. It provides patterns and strategies to **couple copyright notices with core functionality**, making removal impossible without breaking the program.

## Core Principle

```
PRINCIPLE: Copyright declaration + critical parameters = coupled bundle

If someone removes the copyright notice → critical parameters corrupted → program crashes
If someone keeps the copyright notice → proof of origin remains
```

## Features

- **Threat Assessment**: Evaluate project vulnerability to resale
- **Protection Strategies**: Three difficulty levels
  - Level 1: Obscure Language (Haskell, Erlang, OCaml)
  - Level 2: Binary Compilation (Rust, C++, Nim)
  - Level 3: Esolang (Brainfuck, Befunge, Piet)
- **Implementation Patterns**: Code examples for various languages
  - Coupled Validation (seed as encryption key)
  - Distributed State Checks
  - Runtime String Reconstruction
  - Haskell FFI examples
  - Brainfuck interpreter (~50 lines)
- **Legal Guidance**: GPL, copyright, and trademark protection

## Three Difficulty Levels

### Level 1: Obscure Language (Beginner)
- Write protection logic in Haskell, Erlang, OCaml, etc.
- Functional thinking is unfamiliar to most resellers
- Compilable to binary if needed

### Level 2: Binary Compilation (Intermediate)
- Compile protection logic to binary, link statically
- Harder to analyze than source code
- Use obfuscation if needed

### Level 3: Esolang (Advanced)
- Write in Brainfuck, Befunge, Piet, INTERCAL, etc.
- Even elite reverse engineers avoid esolang code
- Mental overhead makes analysis impractical

## Key Patterns

1. **Coupled Validation**: Use copyright as encryption key for config
2. **Distributed Checks**: Embed validation in startup → processing → shutdown
3. **Runtime Reconstruction**: Don't store copyright as plaintext in memory
4. **Obfuscated Naming**: Use mundane names, not "anti_piracy"

## Installation

### For Claude Code

```bash
# Copy to personal skills directory
mkdir -p ~/.claude/skills/anti-reseller
cp -r anti-reseller/* ~/.claude/skills/anti-reseller/
```

### For rust-skills Plugin

The skill is compatible with rust-skills framework. Ensure rust-skills is installed and the skill will be auto-discovered.

## Usage

```bash
# Analyze a project for protection
/anti-reseller ./my-project

# Get general recommendations
/anti-reseller

# Get recommendations for new project
/anti-reseller new
```

## Documentation

See [anti-reseller/SKILL.md](anti-reseller/SKILL.md) for complete documentation including:

- Detailed implementation examples
- Rust code with encryption key pattern
- Haskell FFI examples
- Brainfuck interpreter (ready to embed)
- Protection checklist
- Legal considerations

## Related Skills

| Context | See |
|---------|-----|
| Rust implementation | m05-type-driven |
| Performance constraints | m10-performance |
| Security considerations | m09-domain |
| Code obfuscation | m15-anti-pattern |

## License

MIT
