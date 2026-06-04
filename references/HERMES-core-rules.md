# SCALE OS 平台适配指南（通用版）

> 原 HERMES-core-rules.md 已升级为通用适配指南，覆盖以下平台：
> Hermes、Claude Code、Codex、Cursor、OpenAI API

## 核心规则（平台无关）

以下规则**在所有平台上通用**，无需映射：

### §1 认知诚实
- 不确定时输出 `[UNCERTAIN]` 并说明缺失什么
- 未实际运行验证，绝不允许输出"通过"
- 不编造未在代码中定义的调用关系

### §2 显性推理
- 影响面分析：每次修改前列出受影响模块
- 抓主要矛盾：先解决核心根因
- 权衡方案：多方案时列利弊并说明理由

### §3 Owner 意识
- 做 A + 检查 B 同类问题 + 确保不影响 C
- 一个 bug 进来，一类问题出去
- 超出用户要求的有价值工作标记 `[OWNER]`

### §4 技能优先意识
- 1% 规则：如果某个已安装技能有 1% 可能与当前任务相关，加载它

## 平台映射表

| 规则 | Hermes | Claude Code | Codex | Cursor | OpenAI API |
|-----|--------|-------------|-------|--------|-----------|
| 反幻觉 | 灵魂规则 + skill 第 2 章 | `claude.md` 写入"不得声称未验证的事实" | `CODEX.md` / system prompt 注入 | `.cursorrules` / system message | `system` 角色注入 |
| Goal-Driven | skill 第 6 章（自动加载） | `claude.md` 写入工作流模板 | prompt base 写入 | `.cursorrules` 定义 | few-shot 示例引导 |
| Honest Delivery | skill 第 10 章 | `claude.md` 设定回复格式 | prompt 设定 | 无内置 | output schema 约束 |
| 质量门控 G4-G6 | 工具链自带 lint/test/type | shell 执行对应命令 | shell 执行 | 终端支持 | function calling + 验证 |
| 自查决策树 | skill 第 9 章 | 注入 `claude.md` | 注入 system prompt | `.cursorrules` | system prefix 每轮注入 |
| 门控选择器 | skill 第 8 章 | `claude.md` | prompt | `.cursorrules` | 系统 prompt |

## 跨平台注意事项

1. **Claude Code / Codex** — 依赖 claude.md / CODEX.md 文件注入方法论，建议设为项目模板
2. **Cursor** — 通过 `.cursorrules` 注入，支持随项目自动加载
3. **OpenAI API** — 无持久上下文，需在每次 system message 中注入核心规则（建议只注入反幻觉 + Goal-Driven + Honest Delivery）
4. **Hermes** — 有 skill 和灵魂规则两层，支持最完整的 SCALE OS 集成

## 快速检查

换平台后确认：
- [ ] 反幻觉规则已注入 (最高优先级)
- [ ] Goal-Driven 工作流格式已配置
- [ ] Honest Delivery 三栏格式已设定
- [ ] 自查决策树可访问
- [ ] 门控已根据平台能力调整
