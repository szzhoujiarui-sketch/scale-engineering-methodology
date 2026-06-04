# Conflict Analysis: scale-engineering-methodology v10.3 vs Existing Skills

Generated: 2026-05-29 during v10.3 upgrade.

## Method

Each overlapping skill was read and compared against SCALE v10.3 on:
- Workflow phases (explore/plan/execute/verify/deliver)
- Gate requirements (G1-G9, now with non-code alternatives)
- Core principles (anti-hallucination, anti-laziness, surgical discipline)
- Self-check decision tree
- Three-tier annotation (🟢基础/🟡进阶/🔵深度)

## Results: No Conflicts Found

| Skill | SCALE role | Existing role | Relationship |
|-------|-----------|---------------|-------------|
| **memory-workflow** | Section 11 Self-Evolution: project-level docs | User-level memory | Different layers, different file targets |
| **test-driven-development** | G3 gate: test first or explain why not | RED-GREEN-REFACTOR execution | SCALE sets requirement, TDD provides method |
| **systematic-debugging** | Bugfix workflow | 4-phase RC investigation | SCALE as orchestrator, SD as implementer |
| **writing-plans** | Section 6 Goal-Driven Loop | Plan format, task granularity | SCALE specifies what a plan must contain, WP specifies how to write it |
| **plan** | Section 7 Standard tier | Save markdown to .hermes/plans/ | Complementary formats |
| **agent-world-explorer** | New in v10.1 - unrelated domain | Agent World navigation | No overlap |
| **smart-fetch** | G4 fact-checking: data source verification | Web content extraction | SCALE provides the framework, smart-fetch provides the tool |
| **xiaping** | G9 knowledge update: skill publishing | Skill marketplace operations | SCALE provides the principle, xiaping provides the platform |

## Conflict Resolution Framework

When SCALE rules conflict with other skills or user instructions:

### Type 1: Rule vs Rule Conflict

**Example:** SCALE says "no code changes without verification" but user says "just push it, I'll test later."

**Resolution:**
1. Identify which rule is more fundamental (anti-hallucination > user convenience)
2. Explain the conflict to the user
3. Offer a compromise (push to staging, not production)
4. Document the decision

### Type 2: Skill vs Skill Conflict

**Example:** SCALE says "explore before coding" but TDD says "write test first."

**Resolution:**
1. These are complementary, not conflicting
2. SCALE's explore phase can include understanding existing tests
3. TDD's "write test first" fits into SCALE's G3 gate
4. Use SCALE as orchestrator, TDD as method

### Type 3: Principle vs Practice Conflict

**Example:** SCALE says "surgical discipline — no unrelated changes" but you notice a bug in adjacent code.

**Resolution:**
1. Mention the bug to the user
2. Do NOT fix it unless asked
3. If asked, it becomes part of the current task scope
4. Document the discovery in Honest Delivery's ❌ section

### Type 4: Efficiency vs Quality Conflict

**Example:** SCALE says "full workflow for new features" but user wants it done in 5 minutes.

**Resolution:**
1. Assess actual risk — is it really a "new feature" or a "simple addition"?
2. If low risk, use Standard instead of Full
3. If high risk, explain why Full is needed
4. Never skip anti-hallucination rules regardless of time pressure

## Decision Tree for Conflicts

```
遇到冲突时：

1. 反幻觉规则是否被违反？
   → 是 → 停，不能妥协。反幻觉是硬规则。
   → 否 → 继续

2. 用户指令是否明确？
   → 是 → 按用户指令执行，但标注冲突
   → 否 → 按 SCALE 规则执行，解释原因

3. 是否涉及安全/数据？
   → 是 → 按更严格的规则执行
   → 否 → 按风险等级选择

4. 是否有折中方案？
   → 是 → 提出折中方案
   → 否 → 按更安全的方案执行
```

## v10.3 Changes vs v10.1

1. **Three-tier annotation** — 🟢基础/🟡进阶/🔵深度，Agent 按需加载
2. **Lite version merged** — 不再有独立轻量版，主文件分级标注
3. **Scene quick reference** — 新增 references/scene-quickref.md
4. **Non-code gate details** — G4/G5/G6 非代码场景具体检查项
5. **New case studies** — 电商运营、教育咨询、内容创作
6. **Conflict resolution framework** — 本文件新增四种冲突类型和决策树

## When to Re-run

- If a new workflow/gate/methodology skill is created
- If scale-engineering-methodology is significantly revised again
- If a new platform adaptation is needed
- If user reports rule conflicts in practice
