# 案例：快速原型场景 — Express 工作流

> **SCALE 工程方法论 v10.1 · 实战案例**
> 场景类型：Express 工作流（低风险前端改动）
> 对应章节：第 7 章（标准工作流 — Express 层级）
> 关联门控：G4（事实核查），G5（用户预览）

---

## 1. 场景速览

| 项目 | 内容 |
|------|------|
| 用户原始输入 | "把这个按钮的颜色从蓝改红" |
| 任务类型 | 单 CSS 变量/属性修改 |
| 风险等级 | 低（不影响其他模块，不改逻辑） |
| 推荐工作流 | **Express：fix → verify** |
| 耗时参考 | 1-2 分钟 |

---

## 2. 使用前提检查（是否适用 Express？）

SCALE 第 7 章定义 Express 的使用前提（**必须同时满足**）：

| 前提条件 | 检查结果 | 判断依据 |
|---------|---------|---------|
| 改动不影响其他模块 | ✅ 满足 | 按钮颜色是局部样式，不改变布局或逻辑 |
| 无需探索或设计决策 | ✅ 满足 | 颜色映射关系已知，不需调研 |
| 验证方式明确且一步可达 | ✅ 满足 | 浏览器 DevTools 实时验证，刷新即生效 |

**不适用 Express 的信号检查**：

| 排除信号 | 检查结果 |
|---------|---------|
| 涉及 3 个以上文件？ | ✅ 否（仅 1 个 CSS 文件或组件文件） |
| 需要阅读不熟悉的代码？ | ✅ 否（先在项目内搜索定位） |
| 不确定改完会破坏其他功能？ | ✅ 否（颜色作用域有限） |

**结论：符合 Express 场景，走 fix → verify 最短路径。**

---

## 3. Express 工作流执行过程

### 3.1 执行记录（好版本）

```
用户: 把这个按钮的颜色从蓝改红

[读取项目，定位样式定义]

read_file('src/styles/variables.css') → 发现 --button-primary-bg: #1890ff

[确认这是要改的变量]

read_file('src/components/Button.vue') → 确认该组件使用了 --button-primary-bg

[执行 fix]

patch: src/styles/variables.css
  old: --button-primary-bg: #1890ff;
  new: --button-primary-bg: #ff4d4f;

[执行 verify]

终端: 启动开发服务器 → npm run dev
浏览器: 打开页面，点击按钮 → 按钮颜色已变红
DevTools: Elements 面板检视按钮 → background-color: #ff4d4f ✓
页面截图: 按钮其他交互状态（hover/active）未受影响 ✓

[确认无副作用]

终端: lint 检查 → pnpm lint → exit 0

✅ 完成
```

### 3.2 完整对话记录（好版本）

```
用户:
  "把这个按钮的颜色从蓝改红"

Agent:
  [场景判断]
  这是单 CSS 变量修改，不影响其他模块，验证方式明确。
  符合 Express 工作流前提 → 走 fix → verify。

  [fix 阶段]
  先在项目内搜索颜色变量。

  search_files(pattern='--button-*', target='content')
  → src/styles/variables.css:12:  --button-primary-bg: #1890ff;

  确认 main 按钮就是这个变量。

  read_file('src/styles/variables.css', limit:5, offset:10)
  → line 12: --button-primary-bg: #1890ff;

  修改文件：

  patch(variables.css)
  old: --button-primary-bg: #1890ff;
  new: --button-primary-bg: #ff4d4f;

  [verify 阶段]
  启动开发服务器 → 打开按钮所在页面 → 按钮已从蓝色变红。

  检查交互状态：
  - hover：颜色加深 ✓
  - active：颜色更深 ✓
  - disabled：灰色 ✓（无变化，符合预期）

  DevTools 截图确认生效。

  运行 lint：pnpm lint → exit 0。无新问题。

✅ 完成内容:
  - 按钮主色从蓝 (#1890ff) 改为红 (#ff4d4f)
  - 修改文件: src/styles/variables.css

🔄 验证结果:
  - 浏览器预览: 按钮显示红色 ✓
  - DevTools 检查: background-color 映射正确 ✓
  - 交互状态: hover/active/disabled 均正常 ✓
  - Lint: pass (pnpm lint → exit 0)

❌ 未验证项:
  - 无（改动已全部验证）
```

### 3.3 对比：同一场景的坏版本（啰嗦 + 无效操作）

```
用户:
  "把这个按钮的颜色从蓝改红"

Agent（坏版本）:
  "好的，我来分析这个按钮的颜色修改需求。

  首先，让我做一个全面的影响面分析。
  这个按钮可能涉及以下方面：
  1. CSS 变量
  2. 组件样式
  3. 主题系统
  4. 全局样式覆盖
  5. 浏览器兼容性
  6. 无障碍对比度
  7. 动画过渡
  ...（列出了 12 项）

  让我先探索整个项目结构：
  tree src/ → 输出全部目录结构（80+ 行）

  让我读一下所有相关的配置文件：
  read_file('package.json')
  read_file('vite.config.ts')
  read_file('tsconfig.json')
  read_file('.eslintrc.js')
  read_file('.prettierrc')
  read_file('tailwind.config.js')

  现在让我分析按钮组件可能的所有变体：
  search_files(pattern='Button', target='files')
  → 找到 Button.vue, ButtonGroup.vue, IconButton.vue,
    Button.stories.ts, Button.test.ts, Button.spec.ts

  分别读每一个文件...

  现在列出修改方案（列出了 5 个方案）：
  方案 A：改 CSS 变量
  方案 B：加新变量并替换引用
  方案 C：用 Tailwind 类覆盖
  方案 D：用行内 style
  方案 E：替换组件 props

  经过权衡分析，我决定采用方案 A...

  写完一个详细的计划文档：
  ## 修改计划
  - 目标：将按钮主色改为红色
  - 风险：极低
  - 回滚方案：git revert
  - 验证方案：浏览器查看
  ...

  开始执行修改...

  修改后去检查了 5 个组件文件是否受影响。

  运行了全套测试：npm run test → 等待 3 分钟全部通过。

  运行了 E2E 测试：npm run test:e2e → 等待 8 分钟。

  然后生成了一份 300 字的结案报告..."

❌ 问题总结：
  - 2 分钟的活干了 15 分钟
  - 读了大量无关文件（package.json、vite.config.ts）
  - 做了无意义分析（12 项影响面，实际只需 1 项）
  - 写了冗余的计划文档（Express 本应跳过 plan）
  - 跑了不必要的门控（E2E 测试、全量测试）
  - 核心的有效动作其实只有：定位变量 → 改值 → 验证
```

---

## 4. 好版本 vs 坏版本对比总结

| 维度 | ✅ 好版本（遵守 Express） | ❌ 坏版本（违反 Express） |
|------|---------------------------|---------------------------|
| 耗时 | ~2 分钟 | ~15 分钟 |
| 读文件 | 仅读 2 个相关文件 | 读了 10+ 个无关文件 |
| 计划 | 跳过（Express 特性） | 写了详细计划文档 |
| 改动量 | 1 行 CSS 变量 | 同（但绕了大圈） |
| 验证方式 | DevTools 实时预览 | 全量测试 + E2E |
| 用户感受 | "1 分钟就改好了，赞" | "怎么这么久？我就改个颜色" |
| 判断力 | 正确识别低风险 → 走最短路径 | 把所有改动都当大项目处理 |
| 外科手术纪律 | 仅改目标行 | 改完了还顺手格式化（发生在坏版本中常见） |

---

## 5. 关键教训

1. **判断力比执行力更重要** — 选对工作流是最大效率杠杆
2. **Express 的"快"来自"少"** — 少读无关文件、少写计划、少跑不必要的门控
3. **三个前提缺一不可** — 但凡有一个不满足，就必须升级到 Standard 或 Full
4. **DevTools 是前端改颜色的最佳验证工具** — 不需要写测试就能确认效果
5. **发现符合 Express 场景时，直接问自己：** "我真的需要写计划吗？读这个文件有帮助吗？"

---

## 6. 自查决策树验证（交付前）

| 检查项 | 结果 | 证据 |
|--------|------|------|
| 诚实检查 | ✅ | 文件已读，验证已跑，exit 0 |
| 范围检查 | ✅ | 1 行修改，追溯到用户请求 |
| 进度检查（9 检测器） | ✅ | 无 BruteRetry/IdleTool/etc. |
| 验证检查（G4-G5） | ✅ | Lint pass，DevTools 预览确认 |
| 交付检查 | ✅ | 三栏格式已列 |

---

## 7. 适用延伸

以下场景同样适合 Express 工作流：

| 场景 | 修改数量 | 验证方式 |
|------|---------|---------|
| 改文案错别字 | 1 文件 | 页面预览 |
| 修改 API 超时配置 | 1 配置项 | curl 测试 |
| 调整字体大小 | 1 CSS 变量 | DevTools |
| 修改日志级别 | 1 行配置 | 日志输出检查 |
| 修正文档日期错误 | 1 处 | 肉眼检查 |

> **核心心法**：Express 不是偷懒，是对风险的正确评估。越准确识别低风险场景，越能腾出精力处理真正复杂的问题。
