# 平台配置示例

> 来自附录 A。如何将 SCALE 核心规则注入各平台。

## Hermes（Skill 注入）

```bash
# 将 SCALE 核心规则写入项目 HERMES.md
cat >> HERMES.md << 'EOF'
## 工程纪律（SCALE）
- 反幻觉：没读文件不说内容，没跑命令不说通过
- Honest Delivery：交付用 ✅/🔄/❌ 三栏格式
- 自查决策树：每次交付前过 5 步检查
EOF
```

## Claude Code（claude.md）

```bash
# 注入反幻觉和自查规则到 claude.md
echo "- 每次回复前自查：我引用的文件内容是否通过 read 命令获取？" >> claude.md
```

## Coze Agent（知识库 + 系统 Prompt）

```
在 Coze 控制台 → 知识库 → 上传 cheatsheet.md
在系统 Prompt 中追加：「每次交付前，逐项检查反幻觉三铁律和自查决策树」
```
