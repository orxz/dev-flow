---
name: dev-flow
version: 3.0.0
updated: 2026-05-16
user-invocable: true
description: |
  开发流程引擎——将非平凡开发任务路由到 7 条标准化流程，强制执行工程纪律（先规划后编码、TDD、逐任务审查、文档同步、进度持久化），覆盖从评估到上线的完整生命周期。
  适用于：多文件功能开发、bug修复、重构、依赖升级、紧急热修复、跨会话任务。
  当用户描述一个非平凡开发任务时，主动建议使用——在任何代码编写之前。
triggers:
  - dev-flow
  - 开发流程
  - 开发工作流
  - which workflow
  - how should I structure
  - new feature workflow
  - bug fix process
  - refactor process
  - emergency fix
  - dependency upgrade process
  - 新增功能
  - 修复bug
  - 重构
  - 紧急修复
  - 依赖升级
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
  - Skill
  - Task
  - TodoWrite
---

# dev-flow

## 工程原则

六条铁律，适用于所有流程：

1. **无计划不编码** — 动手前必须有书面方案（设计文档、实施计划、或 markdown 步骤清单）
2. **无测试不改码** — 修改代码前必须有测试覆盖：新功能先写失败测试，bug 先写复现测试
3. **无审查不发布** — 代码变更必须经过审查（工具审查或自查 diff），不能跳过
4. **无根因不修复** — bug 修复前必须定位根因，禁止猜测试探
5. **无安全网不重构** — 重构前必须有测试覆盖，小步推进，每步可独立验证
6. **改了代码必须改文档** — 文档落后于代码 = 技术债务

## 阶段自检

完成任何阶段后，确认以下三项：

- **做了什么**: 一句话描述当前阶段的产出
- **下一步是什么**: 明确下一个阶段
- **有无跳过**: 如跳过了某个步骤，说明原因（"觉得没必要" 不是正当理由）

## 场景引导

收到任务后，按以下决策链选择流程：

```
1. 会改代码吗？
   ├── 不会 → 纯评估/分析/调研 → 流程E
   └── 会 →
       2. 是生产紧急问题吗？（线上挂了、大面积受影响）
          ├── 是 → 流程F
          └── 否 →
               3. 是依赖/框架升级吗？
                  ├── 是 → 流程G
                  └── 否 →
                       4. 现有功能有正确行为可参考吗？
                          ├── 有，但它坏了 → 流程B
                          ├── 有，但要推倒重来 → 流程C
                          ├── 有，但要调整内部实现（行为不变）→ 流程D
                          ├── 没有，这是全新的 → 流程A
                          └── 无法判断 → 用边界判定表分析模糊点，输出分析后询问用户确认
```

**边界判定**:

| 易混淆 | 区分标准 |
|--------|----------|
| Bug vs 重做 | 预期行为已定义但产出不对 → Bug；预期行为本身要改 → 重做 |
| 重构 vs 重做 | 外部接口不变 → 重构；接口/契约变化 → 重做 |
| 重构 vs 新功能 | 不改变用户可见行为 → 重构；用户有新能力 → 新功能 |
| 评估 vs 重构 | 只看不摸 → 评估；动手改 → 重构 |
| 评估 vs 新功能 | 回答"要不要做" → 评估；直接开始做 → 新功能 |
| Bug vs 紧急修复 | 非紧急可走完整TDD → Bug；线上挂了需立刻修 → 紧急修复 |
| 升级 vs 重构 | 只改版本号不改代码 → 升级；改动内部实现 → 重构 |

**一句话速判**:

> 不知道要不要做 → E · 线上挂了 → F · 它坏了 → B · 它没错但要翻新 → C · 它没错但要更好 → D · 全新 → A · 升级依赖 → G

## 场景路由

| 场景 | 典型关键词 | 走哪个流程 |
|------|-----------|-----------|
| 新功能 | "新增"、"开发"、"实现"、"添加" | 流程A: 新功能开发 |
| Bug修复 | "修复"、"报错"、"bug"、"坏了" | 流程B: Bug修复 |
| 功能重做 | "重做"、"重写"、"翻新"XX模块 | 流程C: 功能重做 |
| 优化重构 | "优化"、"重构"、"改进"XX模块 | 流程D: 模块优化重构 |
| 功能评估 | "评估"、"分析"、"调研"、"是否值得"、"可行性" | 流程E: 功能评估 |
| 紧急修复 | "紧急"、"线上"、"hotfix"、"挂了"、"崩了" | 流程F: 紧急修复 |
| 依赖升级 | "升级"、"更新版本"、"依赖" | 流程G: 依赖升级 |

**不适用:** 单行修复、改配置、改文案 — 直接改即可。

## 流程定义

流程定义文件按需加载。根据场景引导选定流程后，读取对应文件执行：

| 流程 | 文件 | 触发场景 |
|------|------|----------|
| A: 新功能开发 | `flows/A.md` | 全新功能 |
| B: Bug修复 | `flows/B.md` | 现有功能坏了 |
| C: 功能重做 | `flows/C.md` | 推翻重写 |
| D: 模块优化重构 | `flows/D.md` | 内部优化，行为不变 |
| E: 功能评估 | `flows/E.md` | 只分析不改代码 |
| F: 紧急修复 | `flows/F.md` | 线上挂了 |
| G: 依赖升级 | `flows/G.md` | 版本升级 |

**加载方式**: 用 Read 工具读取对应文件路径（如 `flows/A.md`），然后按其步骤执行。

## 可选工具套件

三个工具套件可以加速流程，但不是硬依赖。下方流程步骤中每个外部技能都有"如可用/否则"双路径。

**技能名称约定**: 带 `/` 前缀的是 slash 命令（gstack），不带前缀的是技能名（Superpowers）。两者都通过 Skill 工具调用。

**如何判断工具是否可用**: 尝试调用技能（通过 Skill 工具）。如果技能不存在，执行"否则"路径。

| 工具套件 | 技能名 | 不可用时 |
|---|---|---|
| gstack | `/plan-eng-review`, `/review`, `/investigate`, `/ship`, `/document-release`, `/office-hours`, `/plan-ceo-review`, `/freeze` | 见各流程步骤中的"否则"路径 |
| Superpowers | `writing-plans`, `executing-plans`, `subagent-driven-development`, `requesting-code-review`, `test-driven-development` | 见各流程步骤中的"否则"路径 |
| planning-with-files | `planning-with-files-zh` | 手工创建和维护 `task_plan.md`（带复选框）、`progress.md`（进度记录）、`findings.md`（发现记录） |

**自动安装依赖套件**: 当流程中反复命中"否则"路径时，按以下流程处理：

1. **检测**: 用 Bash 检查技能是否存在
2. **询问**: 向用户确认是否安装（仅首次询问）
3. **安装**: 用户确认后执行对应命令
4. **验证**: 再次检测确认安装成功

**路径约定**: 所有检测和安装统一使用 `SKILLS_DIR` 变量，优先 `~/.qoder/skills`（如存在），否则 `~/.claude/skills`。

**安装命令**:

| 套件 | 检测命令 | 安装命令 |
|------|----------|----------|
| gstack | `SKILLS_DIR=$(test -d ~/.qoder/skills && echo ~/.qoder/skills || echo ~/.claude/skills); test -d "$SKILLS_DIR/gstack" && echo "ok"` | 见下方脚本 |
| Superpowers | `SKILLS_DIR=$(test -d ~/.qoder/skills && echo ~/.qoder/skills || echo ~/.claude/skills); for s in writing-plans executing-plans subagent-driven-development requesting-code-review test-driven-development; do test -e "$SKILLS_DIR/$s" || { echo "missing: $s"; exit 1; }; done && echo "ok"` | 见下方脚本 |
| planning-with-files | `SKILLS_DIR=$(test -d ~/.qoder/skills && echo ~/.qoder/skills || echo ~/.claude/skills); test -e "$SKILLS_DIR/planning-with-files-zh" && echo "ok"` | 见下方脚本 |

**gstack 自动安装**:

```bash
SKILLS_DIR=$(test -d ~/.qoder/skills && echo ~/.qoder/skills || echo ~/.claude/skills) && if [ -d "$SKILLS_DIR/gstack" ]; then cd "$SKILLS_DIR/gstack" && git pull && bash ./setup; else git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git "$SKILLS_DIR/gstack" && cd "$SKILLS_DIR/gstack" && bash ./setup; fi
```

**Superpowers 自动安装**（从插件缓存链接到 skills 目录）:

```bash
SUPERPOWERS_SRC=$(find ~/.claude/plugins/cache -path "*/superpowers/*/skills" -type d 2>/dev/null | sort -V | tail -1) && if [ -z "$SUPERPOWERS_SRC" ]; then echo "ERROR: Superpowers plugin not found in cache. Install the plugin in Claude Code first."; exit 1; fi && SKILLS_DIR=$(test -d ~/.qoder/skills && echo ~/.qoder/skills || echo ~/.claude/skills) && for skill in "$SUPERPOWERS_SRC"/*/; do name=$(basename "$skill"); ln -sfn "$skill" "$SKILLS_DIR/$name"; done && echo "Superpowers installed: $(ls "$SKILLS_DIR" | grep -cE 'writing-plans|executing-plans|subagent-driven|requesting-code|test-driven') skills linked"
```

**planning-with-files 自动安装**（从插件缓存链接）:

```bash
PWF_SRC=$(find ~/.claude/plugins/cache -path "*planning-with-files*/planning-with-files-zh" -type d 2>/dev/null | sort -V | tail -1) && if [ -z "$PWF_SRC" ]; then echo "ERROR: planning-with-files plugin not found in cache. Install the plugin in Claude Code first."; exit 1; fi && SKILLS_DIR=$(test -d ~/.qoder/skills && echo ~/.qoder/skills || echo ~/.claude/skills) && ln -sfn "$PWF_SRC" "$SKILLS_DIR/planning-with-files-zh" && echo "planning-with-files-zh installed"
```

如果用户明确表示不需要某个套件，后续不再提示。

---

## 流程入口前置检查

启动任何流程前，先确认以下事项（所有流程通用）：

- [ ] 已在项目 Git 仓库根目录
- [ ] 依赖已安装（`npm install` / `pip install` / 等），无安装错误
- [ ] 测试套件基线已通过（记录当前测试状态，作为回归基准）
- [ ] 已读取项目 CLAUDE.md / CONTRIBUTING.md / README（如有）

如任一项不满足，先修复再进入流程。

---

## 子代理使用判断

走"否则"路径（无 Superpowers 的 `subagent-driven-development` / `executing-plans`）时，按以下标准决定是否用 Task 子代理：

| 场景 | 用 Task 子代理 | 自己做 |
|------|-------------|--------|
| 多文件、多步骤实现 | 每个独立模块派一个子代理并行 | — |
| 单文件、单步骤实现 | — | 直接编码 |
| 代码审查 | 用 `code-reviewer` 子代理 | — |
| 探索/搜索代码库 | 用 `explore-agent` 子代理 | 目标明确的单次搜索 |
| 调试调查 | — | 自己逐步排查 |

并行条件：2+ 个子任务彼此无共享状态、无顺序依赖。否则串行。

---

## 测试隔离规则

技能测试或子代理实验**禁止在主工作区执行**，必须用 git worktree 隔离：

```bash
git worktree add /tmp/task-test -b test/task-name
# 子代理在 /tmp/task-test 里操作
git worktree remove /tmp/task-test --force
git branch -D test/task-name
```

## 持久化规则（强制）

有 planning-with-files 用其技能（如可用: `planning-with-files-zh`），否则手工维护这些文件。

| 时机 | 操作 |
|------|------|
| 会话开始 | 检查 task_plan.md 是否存在，读取当前进度 |
| 每完成一个 Task | 更新 task_plan.md（打勾）+ progress.md（记录） |
| 发现关键信息 | 写入 findings.md |
| 上下文即将耗尽时 | **必须先更新 progress.md**，然后开启新会话，新会话从 progress.md 恢复 |

**文件格式**（走"否则"路径时手工创建，放在项目根目录）:

`task_plan.md`:
```markdown
# 任务计划：[任务标题]

- [ ] 步骤1：[描述]
- [ ] 步骤2：[描述]
- [x] 步骤3：[描述] ← 完成后打勾
```

`progress.md`:
```markdown
# 进度记录

## [日期] 会话 N
- 完成了：[描述]
- 当前状态：[在步骤 X / 已完成]
- 下一步：[描述]
- 阻塞项：[无 / 描述]
```

`findings.md`:
```markdown
# 发现记录

## [主题]
- **日期**：YYYY-MM-DD
- **发现**：[内容]
- **证据**：[文件路径 / 代码行 / 数据]
- **影响**：[描述]
- **建议**：[描述]
```

**文件位置与 Git 策略**:

| 文件 | 存放位置 | 是否提交 Git |
|------|---------|-------------|
| task_plan.md | Git 仓库根目录 | 不提交（本地临时任务追踪） |
| progress.md | Git 仓库根目录 | 不提交（本地临时进度记录） |
| findings.md | Git 仓库根目录 | 建议提交（跨会话的长期知识积累） |

## 文档更新规则

代码变更后必须确保项目文档与代码一致。各流程步骤表中已标注"更新文档"步骤及对应工具。通用原则：**改了什么，就更新对应的描述文档。文档落后于代码 = 技术债务。**

## 版本号

手工发布时（`/ship` 不可用）：

- 按语义版本规则 bump 版本号（通常改 `VERSION` 文件、`package.json` 或等效位置）
- 在 CHANGELOG 中记录本次变更
- 版本号和 CHANGELOG 应包含在同一个 commit 中

## 禁止事项

- 无书面计划直接编码
- 修改代码前不写测试
- 上下文耗尽前不更新 progress.md
- 定位根因之前修改代码
- 跳过审查直接发布
- 重构时无测试安全网
- 技能测试/子代理实验在主工作区执行

## 常见错误与 Red Flags

| 反模式 | 后果 | 正确做法 |
|--------|------|---------|
| "这个bug很简单，直接改" | 治标不治本、引入新bug | 先定位根因（如可用 `/investigate`，否则手工排查），写复现测试 |
| "重构顺手加个功能" | 混杂变更、难以审查 | 重构和新功能必须分开走不同流程 |
| "测试后面再补" | TDD 被反转 | 先写失败测试 → 实现 → 通过 |
| "旧代码我大概懂了" | 破坏调用方 | 必须列出所有调用方和接口签名 |
| "跳过审查直接发布" | 缺少审查 | 审查是强制步骤，不能只在脑中过一遍 |
| "线上紧急，先改了再说" | 事后永远不补 | 走流程F，事后必补测试 |
| "升级应该没风险" | 引入兼容性故障 | 先读 CHANGELOG，必须有回滚方案 |
| 跳过架构审查直接写代码 | 方案错误 | 新功能必走架构审查（如可用 `/plan-eng-review`，否则手写设计文档） |
| 不更新 progress.md | 会话中断后丢失进度 | 每次变更后更新 |

## 异常处理

| 情况 | 处理方式 |
|------|----------|
| 步骤执行失败 | 停止当前步骤，记录失败原因到 findings.md，不要跳过继续 |
| 同一问题修复失败 3 次 | 停止，写入 findings.md，向用户汇报。可能是架构问题，考虑升级为流程C。**跨会话计数**：每次失败记录到 progress.md（`失败次数: N/3`），新会话从 progress.md 读取继续计数 |
| 测试套件无法通过 | 不发布。修复测试或修复代码，二选一，不允许跳过测试 |
| 发现计划有遗漏 | 回到规划阶段补充，不要边写边改计划 |
| 上下文即将耗尽 | 立即更新 progress.md → 开启新会话 → 从 progress.md 恢复 |
| 流程执行中途发现走错流程 | 停止当前流程，记录已完成的步骤，切换到正确流程重新开始 |
| 复合任务（涉及多个流程） | 按优先级拆解为独立子任务，依次执行。优先级：流程F（紧急）> 流程B（bug）> 流程G（升级）> 流程D（重构）> 流程A（新功能）。每个子任务独立走完完整流程，不交叉执行 |
