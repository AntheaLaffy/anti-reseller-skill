# anti-reseller-skill

> 开源软件防倒卖保护的 Claude Code Skill

[English](./README.md) | [中文](./README-zh.md)

---

## 概述

这是一个 Claude Code skill，帮助开发者实现开源软件的反倒卖保护机制。提供将**版权声明与核心功能耦合**的模式和策略，使移除版权声明会导致程序崩溃。

## 核心原则

```
原则：版权声明 + 关键参数 = 耦合捆绑

如果有人移除版权声明 → 关键参数损坏 → 程序崩溃
如果有人保留版权声明 → 保留原创证据
```

## 功能特性

- **威胁评估**：评估项目被倒卖的风险
- **保护策略**：三个难度等级
  - Level 1：冷门语言 (Haskell, Erlang, OCaml)
  - Level 2：二进制编译 (Rust, C++, Nim)
  - Level 3：Esolang (Brainfuck, Befunge, Piet)
- **实现模式**：多种语言的代码示例
  - 耦合验证（种子作为加密密钥）
  - 分布式状态检查
  - 运行时字符串重构
  - Haskell FFI 示例
  - Brainfuck 解释器（~50 行）
- **法律指导**：GPL、版权和商标保护

## 三个难度等级

### Level 1：冷门语言（入门级）
- 使用 Haskell、Erlang、OCaml 等编写保护逻辑
- 函数式思维对大多数倒卖者来说很陌生
- 可以编译成二进制

### Level 2：二进制编译（进阶级）
- 将保护逻辑编译成二进制并静态链接
- 比源代码更难分析
- 需要时可使用混淆

### Level 3：Esolang（高级）
- 使用 Brainfuck、Befunge、Piet、INTERCAL 等编写
- 即使是顶尖逆向工程师也会避开 esolang 代码
- 心理负担使分析变得不切实际

## 核心模式

1. **耦合验证**：将版权作为加密密钥用于配置解密
2. **分布式检查**：在启动 → 处理 → 关闭三个阶段嵌入验证
3. **运行时重构**：不要在内存中明文存储版权
4. **混淆命名**：使用普通函数名，不要用 "anti_piracy"

## 安装

### 对于 Claude Code

```bash
# 复制到个人 skills 目录
mkdir -p ~/.claude/skills/anti-reseller
cp -r anti-reseller/* ~/.claude/skills/anti-reseller/
```

### 对于 rust-skills 插件

该 skill 与 rust-skills 框架兼容。确保已安装 rust-skills，skill 将自动被发现。

## 使用方法

```bash
# 分析项目的保护措施
/anti-reseller ./my-project

# 获取一般性建议
/anti-reseller

# 获取新项目建议
/anti-reseller new
```

## 文档

完整文档见 [anti-reseller/SKILL.md](anti-reseller/SKILL.md)，包括：

- 详细实现示例
- 包含加密密钥模式的 Rust 代码
- Haskell FFI 示例
- Brainfuck 解释器（可直接嵌入）
- 保护检查清单
- 法律注意事项

## 相关 Skills

| 场景 | 参考 |
|------|------|
| Rust 实现 | m05-type-driven |
| 性能约束 | m10-performance |
| 安全考量 | m09-domain |
| 代码混淆 | m15-anti-pattern |

## 许可证

MIT
