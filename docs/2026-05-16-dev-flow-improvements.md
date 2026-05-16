# dev-flow 全面改进实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 修复评估报告中发现的全部问题，将 dev-flow skill 从单文件 553 行拆分为分层加载架构，修复旧名残留，补充文档和使用示例。

**Architecture:** SKILL.md 拆分为索引文件（~200行，含决策路由+通用规则）+ 7 个流程独立文件（按需加载）。同步修复 triggers、流程C跳转说明、README使用示例，新增 CHANGELOG。

**Tech Stack:** Markdown 文件编辑，无代码依赖。

---

## File Structure

| 文件 | 职责 |
|------|------|
| `SKILL.md` | 索引文件：frontmatter + 工程原则 + 阶段自检 + 场景引导/路由 + 工具套件 + 前置检查 + 子代理判断 + 通用规则（持久化/禁止/异常） |
| `flows/A.md` | 流程A: 新功能开发（独立文件） |
| `flows/B.md` | 流程B: Bug修复（独立文件） |
| `flows/C.md` | 流程C: 功能重做（独立文件） |
| `flows/D.md` | 流程D: 模块优化重构（独立文件） |
| `flows/E.md` | 流程E: 功能评估（独立文件） |
| `flows/F.md` | 流程F: 紧急修复（独立文件） |
| `flows/G.md` | 流程G: 依赖升级（独立文件） |
| `README.md` | 项目说明 + 使用示例段落 |
| `CHANGELOG.md` | 版本变更记录 |

---

### Task 1: 创建 flows 目录和流程A独立文件

**Files:**
- Create: `flows/A.md`

- [ ] **Step 1: 创建 flows 目录**

Run: `mkdir -p /Users/rengang/Documents/tools/dev-flow/flows`

- [ ] **Step 2: 提取流程A内容到独立文件**

从 SKILL.md 第 195-227 行（流程A: 新功能开发）提取，包装为独立文件。文件头部加引用说明，让 AI 知道这是被索引文件按需加载的：

```markdown
# 流程A: 新功能开发

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

从零到一，架构先行。新功能必须有设计文档，不允许边想边写。

```
阶段1(意图) → 阶段2(规划) → 阶段3(执行) → 阶段4(收尾)
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 架构审查 | 如可用: `/plan-eng-review` · 否则: 手写设计文档(数据流、接口、边界、权衡) | 新功能必做 |
| 2. 产品决策 | 如可用: `/office-hours` 或 `/plan-ceo-review` · 否则: 与用户直接确认范围和验收标准 | 可选 |
| 3. 写实施计划 | 如可用: `writing-plans` · 否则: 手写 markdown 实施步骤清单 | |
| 4. 初始化进度 | 如可用: `planning-with-files-zh` · 否则: 手工创建 `task_plan.md` 带复选框 | |
| 5. 编码 | 如可用: `subagent-driven-development` 或 `executing-plans` · 否则: 逐步实现(写失败测试→通过→重构) | 严格 TDD |
| 6. 每task审查 | 如可用: `requesting-code-review` · 否则: 自查每个完成步骤的 diff | |
| 7. 全部完成审查 | 如可用: `/review` · 否则: 完整自查 diff (SQL安全/错误处理/边界/风格) | |
| 8. 更新文档 | 如可用: `/document-release` · 否则: 手工更新 README/CHANGELOG/架构文档 | |
| 9. 提交发布 | 如可用: `/ship` · 否则: 手工 merge → 测试 → commit → push → 创建 PR | |

## 输出物

- 步骤1 → 设计文档（数据流图、接口定义、边界条件、技术权衡）
- 步骤2 → 确认的范围文档或聊天记录（验收标准、用户故事）
- 步骤3 → `task_plan.md`（带复选框的实施步骤清单）
- 步骤4 → `progress.md`（初始进度记录）
- 步骤5 → 代码 + 测试文件
- 步骤6-7 → 审查记录（工具输出或 diff 分析）
- 步骤8 → 更新后的 README / CHANGELOG / 架构文档
- 步骤9 → PR 链接

## 完成条件

设计文档已写 → 实施计划已确认 → 所有测试通过 → 代码已审查 → 文档已同步 → PR 已创建。

## 禁止

- 新功能跳过架构审查（步骤1不可跳过）
- 跳过审查直接发布
- 边写边改实施计划
- 未更新文档就提交
```

Write the above content to `flows/A.md`.

---

### Task 2: 创建流程B独立文件

**Files:**
- Create: `flows/B.md`

- [ ] **Step 1: 写入流程B内容**

```markdown
# 流程B: Bug修复

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

先定位再修改，禁止先改代码再找原因。

```
定位根因 → 复现测试 → 修复 → 审查 → 提交
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 定位根因 | 如可用: `/investigate` · 否则: 手工追踪代码路径、加日志、git bisect | **禁止先改代码** |
| 2. 写复现测试 | 如可用: `test-driven-development` · 否则: 手工写失败测试复现 bug | 先红 |
| 3. 修复 | 编码 | 最少改动让测试通过（绿） |
| 4. 跑全量测试 | 运行全量测试 | 确认无回归 |
| 5. 审查 | 如可用: `requesting-code-review` · 否则: 自查 diff 的正确性和最小性 | |
| 6. 更新文档 | 如可用: `/document-release` · 否则: 手工更新文档 | 如根因值得记录 |
| 7. 提交 | 如可用: `/ship` · 否则: 手工 commit → push → 创建 PR | |

> 跨模块 bug → 步骤1如 gstack 可用: `/investigate` + `/freeze`，否则手工锁定调查范围。同一问题失败3次 → 写入 findings.md，停止并汇报。

## 输出物

- 步骤1 → `findings.md`（根因分析记录：症状→排查路径→根因→影响范围）
- 步骤2 → 失败测试文件（红）
- 步骤3 → 修复代码 + 测试通过（绿）
- 步骤4 → 测试运行报告
- 步骤5 → diff 审查记录
- 步骤6 → 更新后的文档（如适用）
- 步骤7 → PR 链接

## 完成条件

根因已定位并记录 → 复现测试已通过（修复前红，修复后绿） → 全量测试无回归 → diff 已审查确认最小性。

## 禁止

- 跳过根因定位直接改代码
- 越过复现测试直接修复
- 修复时顺手改无关代码或重构
- 不跑全量测试就提交
```

Write the above content to `flows/B.md`.

---

### Task 3: 创建流程C独立文件（含改进的跳转说明）

**Files:**
- Create: `flows/C.md`

- [ ] **Step 1: 写入流程C内容，改进步骤3的跳转说明**

改进点：原 SKILL.md 步骤3 写"跳过A的步骤1-3"，但未明确后续步骤与A的对应关系。改为明确标注：

```markdown
# 流程C: 功能重做

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

先理解旧实现的接口契约和调用方，再设计新方案。

```
分析旧实现 → 架构审查 → 接流程A（步骤4→编码→审查→发布）
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 分析旧实现 | 读代码 + Explore 子代理 | 理清：接口签名、调用方、数据流、测试覆盖 |
| 2. 架构审查 | 如可用: `/plan-eng-review` · 否则: 手写新旧方案对比文档 | |
| 3. 初始化进度 | 如可用: `planning-with-files-zh` · 否则: 手工创建 `task_plan.md` | 已完成步骤1-2（旧实现分析+架构审查），对应流程A的步骤1-3，此后进入执行阶段 |
| 4. 编码 | 如可用: `subagent-driven-development` 或 `executing-plans` · 否则: 逐步实现(写失败测试→通过→重构) | 严格 TDD，等同于流程A步骤5 |
| 5. 每task审查 | 如可用: `requesting-code-review` · 否则: 自查每个完成步骤的 diff | 等同于流程A步骤6 |
| 6. 全部完成审查 | 如可用: `/review` · 否则: 完整自查 diff | 等同于流程A步骤7 |
| 7. 更新文档 | 如可用: `/document-release` · 否则: 手工更新 README/CHANGELOG/架构文档 | 等同于流程A步骤8 |
| 8. 提交发布 | 如可用: `/ship` · 否则: 手工 merge → 测试 → commit → push → PR | 等同于流程A步骤9 |

> **关键约束:** 接口签名不兼容变更时必须列出所有调用方。有测试覆盖的旧代码，先确认测试是否仍然有效。

## 步骤对应关系

| 流程C步骤 | 等同于流程A步骤 | 说明 |
|-----------|----------------|------|
| 1-2 | A步骤1-3 | C的分析旧实现+架构审查 = A的架构审查+产品决策+实施计划 |
| 3 | A步骤4 | 初始化进度追踪 |
| 4-8 | A步骤5-9 | 编码→审查→发布，执行逻辑完全一致 |

## 输出物

- 步骤1 → 旧实现分析文档（接口签名列表、调用方列表、数据流图、测试覆盖状况）
- 步骤2 → 新旧方案对比文档（接口变更矩阵、迁移策略、风险点）
- 步骤3 → `task_plan.md` + `progress.md`
- 步骤4 → 代码 + 测试文件
- 步骤5-6 → 审查记录
- 步骤7 → 更新后的文档
- 步骤8 → PR 链接

## 完成条件

旧实现所有调用方已列出 → 新旧方案已对比 → 所有测试通过 → 代码已审查 → 文档已同步 → PR 已创建。
```

Write the above content to `flows/C.md`.

---

### Task 4: 创建流程D独立文件

**Files:**
- Create: `flows/D.md`

- [ ] **Step 1: 写入流程D内容**

```markdown
# 流程D: 模块优化重构

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

测试先行作为安全网，小步重构，每步可独立验证。

```
补测试 → 识别瓶颈 → 小步重构 → 逐步验证 → 审查 → 提交
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 补测试 | 编码 | 如果模块测试覆盖不足，**先补测试**建立安全网 |
| 2. 识别瓶颈 | 如可用: `/plan-eng-review` · 否则: 手工分析性能/结构问题，定重构边界 | |
| 3. 拆解计划 | 如可用: `writing-plans` · 否则: 手工写小步重构清单，每步可独立跑测试 | |
| 4. 逐步重构 | 如可用: `executing-plans` · 否则: 手工逐步执行：改 → 测试 → 绿 → 下一步 | |
| 5. 每步审查 | 如可用: `requesting-code-review` · 否则: 每步完成后自查 diff | |
| 6. 全量测试 | 运行全量测试 + 代码格式检查 | 确认无回归 |
| 7. 审查 | 如可用: `/review` · 否则: 完整自查 diff | |
| 8. 更新文档 | 如可用: `/document-release` · 否则: 手工更新架构文档 | |
| 9. 提交 | 如可用: `/ship` · 否则: 手工 commit → push → PR | |

## 输出物

- 步骤1 → 测试文件（安全网，重构前全部通过）
- 步骤2 → 瓶颈分析 + 重构边界文档
- 步骤3 → `task_plan.md`（小步重构清单，每步可独立测试）
- 步骤4 → 重构后代码 + 每步测试通过记录
- 步骤5-7 → 审查记录
- 步骤8 → 更新后的架构文档
- 步骤9 → PR 链接

## 完成条件

测试安全网已建立 → 每步重构测试通过 → 全量测试无回归 → 行为与重构前一致。

## 禁止

- 没有测试安全网就重构
- 跨多模块一次性大改
- 重构同时加新功能
```

Write the above content to `flows/D.md`.

---

### Task 5: 创建流程E独立文件

**Files:**
- Create: `flows/E.md`

- [ ] **Step 1: 写入流程E内容**

```markdown
# 流程E: 功能评估

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

基于事实分析，不凭直觉判断。评估结论必须有代码/数据支撑。

```
明确目标 → 信息收集 → 分析评估 → 输出结论
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 明确目标 | 如可用: `/office-hours` · 否则: 与用户直接确认评估范围和判定标准 | |
| 2. 收集信息 | 读代码 + Explore 子代理 | 理清现状：架构、调用链、数据流、性能数据、瓶颈 |
| 3. 分析评估 | 如可用: `/plan-eng-review` · 否则: 手工多维度分析(可行性/风险/成本/收益) | 至少对比2种方案 |
| 4. 输出结论 | 写入 findings.md | 结论+建议+行动计划，标注关键证据来源 |

## 评估维度参考

| 维度 | 关注点 |
|------|--------|
| 技术可行性 | 现有架构是否支持？需要改多少？引入新依赖？ |
| 风险 | 是否影响核心流程？回滚难度？数据一致性？ |
| 成本 | 开发量、测试量、运维复杂度 |
| 收益 | 解决什么问题？影响多少用户？可量化吗？ |
| 替代方案 | 有没有更简单的做法？不做的后果？ |

## 输出物

- 步骤1 → 确认的评估范围文档（维度、判定标准、利益相关方）
- 步骤2 → 现状分析笔记（架构图、调用链、数据流、性能数据）
- 步骤3 → 至少 2 种方案的对比分析（每种覆盖 5 个评估维度）
- 步骤4 → `findings.md`（结论 + 建议 + 行动计划 + 证据来源）

## 完成条件

至少对比2种方案 → 每种方案覆盖5个评估维度 → 结论文档已写入 findings.md → 结论明确标注证据来源。

## 禁止

- 没看代码就下结论
- 只列优点不提风险
- 评估后直接开写（必须先输出结论文档）
```

Write the above content to `flows/E.md`.

---

### Task 6: 创建流程F独立文件

**Files:**
- Create: `flows/F.md`

- [ ] **Step 1: 写入流程F内容**

```markdown
# 流程F: 紧急修复

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

线上优先止损，恢复服务第一，根因分析第二。事后必须补测+复盘。

```
定位止损 → 最小修复 → 验证上线 → 事后补测
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 定位止损 | 如可用: `/investigate` · 否则: 手工快速定位根因 | **先止血**（回滚/切流/关功能） |
| 2. 最小修复 | 编码 | 只改必要代码，跳过完整 TDD。**禁止顺手重构** |
| 3. 验证上线 | 如可用: `/ship` · 否则: 手工确认修复有效并提交推送 | |
| 4. 事后补测 | 如可用: `test-driven-development` · 否则: 手工补复现测试+回归测试 | 修复上线后**同一会话内**必做 |
| 5. 复盘 | 写入 findings.md | 记录根因、修复过程、预防措施 |

> **规则:** 流程F 可跳过审查和 TDD（当时），但事后必须补测试。同一问题出现 2 次紧急修复 → 升级为流程C（重做）。**禁止**以"紧急"为借口跳过事后补测。

## 输出物

- 步骤1 → 止血操作记录（回滚/切流/关功能的操作和确认）
- 步骤2 → 最小修复代码（无测试，仅修复代码）
- 步骤3 → 上线确认（修复已生效的证据）
- 步骤4 → 复现测试 + 回归测试文件
- 步骤5 → `findings.md`（根因、修复过程、预防措施、时间线）

## 完成条件

线上已止血 → 修复已验证上线 → 事后补测已完成 → 复盘记录已写入 findings.md。
```

Write the above content to `flows/F.md`.

---

### Task 7: 创建流程G独立文件

**Files:**
- Create: `flows/G.md`

- [ ] **Step 1: 写入流程G内容**

```markdown
# 流程G: 依赖升级

> 本文件是 dev-flow skill 的流程定义之一，由 SKILL.md 按需加载。执行前需确认前置检查已完成。

## 核心原则

升级不改功能，兼容性检查优先，保留回滚路径。

```
兼容分析 → 升级计划 → 逐步升级 → 全量测试 → 审查 → 更新文档 → 提交
```

## 步骤

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 兼容分析 | 读 CHANGELOG + Explore | 确认 breaking changes、废弃 API、新要求（语言/运行时版本） |
| 2. 升级计划 | 如可用: `/plan-eng-review` · 否则: 手工确定升级范围、顺序、回滚方案 | |
| 3. 逐步升级 | 编码 | 逐包升级：更新依赖 → 运行测试 → 绿 → 下一个 |
| 4. 全量测试 | 运行全量测试 + 代码格式检查 | 确认无回归 |
| 5. 审查 | 如可用: `/review` · 否则: 完整自查 diff | |
| 6. 更新文档 | 如可用: `/document-release` · 否则: 手工更新技术栈版本、依赖说明 | |
| 7. 提交 | 如可用: `/ship` · 否则: 手工 commit → push → PR | |

## 升级清单

| 检查项 | 说明 |
|--------|------|
| CHANGELOG | 必读每个包的 release notes |
| breaking changes | 搜索 `BREAKING`、`deprecated`、`removed` |
| 版本约束 | 确认依赖配置文件版本范围正确 |
| 环境要求 | 语言版本、运行时要求、系统依赖 |
| 锁文件 | 提交依赖锁文件变更 |
| 回滚方案 | 记录回滚步骤，必要时保留旧版分支 |

## 输出物

- 步骤1 → 兼容性分析笔记（每包 breaking changes、废弃 API、新环境要求）
- 步骤2 → 升级计划（包顺序、回滚方案、风险点）
- 步骤3 → 更新后的依赖配置文件 + 锁文件
- 步骤4 → 全量测试运行报告
- 步骤5 → diff 审查记录
- 步骤6 → 更新后的技术栈文档 / README
- 步骤7 → PR 链接

## 完成条件

CHANGELOG 已逐包阅读 → 逐包升级每包测试通过 → 全量测试无回归 → 回滚方案已记录 → 文档已更新。

## 禁止

- 不看 CHANGELOG 就升级
- 多包混合批量升级
- 升级同时加新功能或重构
- 无回滚方案直接升级
```

Write the above content to `flows/G.md`.

---

### Task 8: 重写 SKILL.md 为索引文件（含 triggers 修复）

**Files:**
- Modify: `SKILL.md` (全文重写为索引文件)

- [ ] **Step 1: 重写 SKILL.md**

将 553 行的单文件重写为 ~200 行的索引文件。保留：frontmatter（修复 triggers）、工程原则、阶段自检、场景引导、场景路由、可选工具套件、流程入口前置检查、子代理使用判断、测试隔离规则、持久化规则、文档更新规则、版本号、禁止事项、常见错误、异常处理。

删除：7 个流程的完整步骤表（已移入 flows/*.md）。

替换为流程索引表 + 加载说明。

关键改动：

**frontmatter triggers 修复**（第21行）：
- 删除: `- 工程方法论`
- 新增: `- dev-flow`（已在 name 里，但 triggers 里没有直接匹配）

**新增流程索引段落**（替代 7 个流程完整内容）：

```markdown
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
```

完整的新 SKILL.md 内容：

```markdown
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
```

Write the above content to SKILL.md (full rewrite).

---

### Task 9: 更新 README.md 添加使用示例

**Files:**
- Modify: `README.md`

- [ ] **Step 1: 在 README.md 的 Optional Tool Suites 段落前插入使用示例段落**

在 `## Installation` 段落和 `## Optional Tool Suites` 段落之间插入：

```markdown
## Usage Example

User says: "用户登录功能偶尔报 500 错误"

dev-flow routes through the decision tree:
1. Will it change code? **Yes**
2. Is it production emergency? **No** (occasional, not down)
3. Is it a dependency upgrade? **No**
4. Existing behavior but broken? **Yes** → **Flow B: Bug Fix**

Execution:
1. Locate root cause → trace error logs, find null pointer in auth service
2. Write reproduction test → test that reproduces the 500 with the same input
3. Fix → minimal code change to handle null case
4. Run full test suite → all green, no regression
5. Review diff → confirm fix is minimal and correct
6. Update docs → add known issue to troubleshooting guide
7. Commit & PR

Each flow produces specific artifacts (findings.md, test files, review records) and enforces completion gates before proceeding.

---
```

Insert this section after the `## Installation` section (after line 33 `cp -r . ~/.claude/skills/dev-flow`) and before `## Optional Tool Suites`.

---

### Task 10: 创建 CHANGELOG.md

**Files:**
- Create: `CHANGELOG.md`

- [ ] **Step 1: 创建 CHANGELOG.md**

```markdown
# Changelog

All notable changes to dev-flow will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.0.0] - 2026-05-16

### Changed
- **Breaking**: Split single SKILL.md (553 lines) into layered architecture — index file (~200 lines) + 7 independent flow files (`flows/A.md` through `flows/G.md`) loaded on demand
- Replaced trigger `工程方法论` (legacy name) with direct `dev-flow` trigger
- Clarified Flow C step mapping: steps 4-8 now explicitly reference corresponding Flow A steps

### Added
- Flow index table in SKILL.md with file paths and loading instructions
- Usage example section in README.md with end-to-end walkthrough
- CHANGELOG.md (this file)

## [2.0.0] - 2026-05-16

### Changed
- Renamed project from `engineering-methodology` to `dev-flow`
- Restructured from multi-skill collection to single-skill repository
- Rewrote SKILL.md with 7 flows, inline dual-path tool fallbacks, engineering principles, completion conditions, per-flow prohibitions, error handling table, and version bump rules
- Added user-invocable, triggers, and allowed-tools to frontmatter

## [1.0.0] - 2026-05-15

### Added
- Initial release as `engineering-methodology`
- 7-path decision tree for development task routing
- Basic flow definitions with tool suite integration
```

Write the above content to `CHANGELOG.md`.

---

### Task 11: 验证所有改动一致性

**Files:**
- All files in project

- [ ] **Step 1: 验证无旧名残留**

Run: `grep -rn "dev-workflow\|engineering-methodology\|工程方法论" /Users/rengang/Documents/tools/dev-flow/ --include="*.md" || echo "OK: no legacy names found"`
Expected: `OK: no legacy names found`

- [ ] **Step 2: 验证所有流程文件存在**

Run: `for f in A B C D E F G; do test -f /Users/rengang/Documents/tools/dev-flow/flows/$f.md && echo "flows/$f.md OK" || echo "flows/$f.md MISSING"; done`
Expected: all 7 files report OK

- [ ] **Step 3: 验证 SKILL.md 行数**

Run: `wc -l /Users/rengang/Documents/tools/dev-flow/SKILL.md`
Expected: ~200 lines (significantly less than original 553)

- [ ] **Step 4: 验证 SKILL.md 流程索引表引用正确**

Run: `grep "flows/" /Users/rengang/Documents/tools/dev-flow/SKILL.md`
Expected: 7 lines with `flows/A.md` through `flows/G.md`

- [ ] **Step 5: 验证 version 号一致性**

Run: `grep "version:" /Users/rengang/Documents/tools/dev-flow/SKILL.md`
Expected: `version: 3.0.0`

Run: `head -3 /Users/rengang/Documents/tools/dev-flow/CHANGELOG.md`
Expected: CHANGELOG header

---

### Task 12: 提交全部改动

**Files:**
- All changed files

- [ ] **Step 1: Stage all files**

Run: `cd /Users/rengang/Documents/tools/dev-flow && git add -A`

- [ ] **Step 2: Verify staged changes**

Run: `git diff --cached --stat`
Expected: SKILL.md modified, README.md modified, CHANGELOG.md created, flows/A.md through flows/G.md created

- [ ] **Step 3: Commit**

```bash
git commit -m "feat: v3.0.0 — split to layered architecture + fix legacy names + add usage examples

- Split SKILL.md (553→~200 lines) into index + 7 flow files (flows/A-G.md)
- Remove legacy trigger '工程方法论', add direct 'dev-flow' trigger
- Clarify Flow C step mapping with explicit A-step correspondence table
- Add end-to-end usage example to README.md
- Add CHANGELOG.md with version history"
```

Expected: commit succeeds
