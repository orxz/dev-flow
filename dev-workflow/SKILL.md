---
name: dev-workflow
description: Use when starting any non-trivial development task, feature implementation, or bug fix spanning multiple files. Use when unsure which tool (gstack vs Superpowers vs planning-with-files) to use at each phase. Use when tasks span multiple sessions and need progress tracking. Use when you find yourself jumping straight to coding without planning or skipping code review before shipping.
user-invocable: true
---

# 开发工作流

## 概述

**关键规则**: 每个阶段必须通过 Skill 工具实际调用对应技能。内部思考不算完成。你说"我先分析一下"然后直接写代码 = 跳过了该阶段。

## 每阶段自检（强制）

完成任何阶段后，**必须**输出以下格式：

```
### 阶段自检
✅ 已调用: /skill-name — 简要结果
❌ 跳过: /skill-name — 原因
```

**规则**:
- 出现 ❌ 且无正当理由 → **停止并等待用户确认**才能继续
- "项目无测试框架" 是跳过 TDD 的正当理由
- "觉得没必要" **不是**正当理由

## 完整生命周期

7 个流程覆盖一条任务从"要不要做"到"做完上线"的完整链路：

```
收到任务
  │
  1. 会改代码吗？
  ├── 不会 → 【流程E: 评估】
  │            │
  │            明确目标 → 信息收集 → 分析评估 → 输出结论
  │            │
  │            └── 结论：做 → 进入步骤2
  │            └── 结论：不做 → 结束
  │
  └── 会 ──→
        2. 紧急吗？（线上挂了、用户大面积受影响）
          ├── 是 → 【流程F: 紧急修复】→ 快速修复 → 事后补测
          └── 否 →
                3. 是依赖/框架升级吗？
                  ├── 是 → 【流程G: 依赖升级】
                  └── 否 →
                        4. 现有功能有参考吗？ ──→ 选择流程 A/B/C/D
                        │
                        流程A/C/D ──→ 共性阶段链：
                        │
                        规划 ──→ 执行(TDD) ──→ 审查 ──→ ship
                        │         │              │          │
                        gstack     Superpowers    gstack     gstack
                        │         + pwf          + pwf
                        │         (F走快速通道)
                        │
                        └── 完成 ✅
```

**关键衔接点**:

| 衔接 | 说明 |
|------|------|
| E → A/B/C/D/G | 评估结论为"做"时，进入对应实现流程 |
| C → A | 重做分析完旧实现后，接新功能开发流程 |
| F → /ship | 紧急修复快速发布，事后补测+复盘 |
| 所有流程 → /ship | A/B/C/D/G 最终都通过 `/ship` 收尾 |
| 评估循环 | 结论为"不做"，任务到此结束 |

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
                          └── 没有，这是全新的 → 流程A
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
| 依赖升级 | "升级"、"更新版本"、"composer"、"依赖" | 流程G: 依赖升级 |

**不适用:** 单行修复、改配置、改文案 — 直接改即可。

## 三工具定位

| 工具 | 职责 | 产出 |
|------|------|------|
| gstack | 决策/审查 | plan docs, review reports |
| Superpowers | 执行/质量 | 代码, 测试, commits |
| planning-with-files | 状态持久化 | task_plan.md, findings.md, progress.md |

---

## 流程A: 新功能开发

```
阶段1(意图) → 阶段2(规划) → 阶段3(执行) → 阶段4(收尾)
  gstack        Superpowers     Superpowers      gstack
                + pwf           + pwf
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 架构审查 | `/plan-eng-review` | gstack 审查方案（新功能必做） |
| 2. 产品决策 | `/office-hours` 或 `/plan-ceo-review` | 可选 |
| 3. 写实施计划 | `/writing-plans` | Superpowers 产出计划文档 |
| 4. 初始化进度 | `/plan-zh` | planning-with-files 创建 task_plan.md |
| 5. 编码 | `/subagent-driven-dev` 或 `/executing-plans` | Superpowers + 严格 TDD |
| 6. 每task审查 | `/requesting-code-review` | 每完成一个 task 后 |
| 7. 全部完成审查 | `/review` | gstack 最终代码审查 |
| 8. 更新文档 | `/document-release` | gstack |
| 9. 提交发布 | `/ship` | gstack 提交推送创建PR |

---

## 流程B: Bug修复

**核心原则:** 先定位再修改，禁止先改代码再找原因。

```
定位根因 → 复现测试 → 修复 → 审查 → 提交
investigate  TDD         TDD     review   ship
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 定位根因 | `/investigate` | gstack 定位根因，写入 findings.md。**禁止先改代码** |
| 2. 写复现测试 | `/test-driven-dev` | 先写失败测试复现 bug（红） |
| 3. 修复 | 编码 | 最少改动让测试通过（绿） |
| 4. 跑全量测试 | `php artisan test` | 确认无回归 |
| 5. 审查 | `/requesting-code-review` | Superpowers |
| 6. 更新文档 | `/document-release` | 如根因值得记录，更新 docs |
| 7. 提交 | `/ship` | gstack |

> 跨模块 bug → 步骤1用 `/investigate` + `/freeze`。同一问题失败3次 → 写入 findings.md，停止并汇报。

---

## 流程C: 功能重做

**核心原则:** 先理解旧实现的接口契约和调用方，再设计新方案。

```
分析旧实现 → 架构审查 → 走流程A 阶段2-4
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 分析旧实现 | 读代码 + Explore 子代理 | 理清：接口签名、调用方、数据流、测试覆盖 |
| 2. 架构审查 | `/plan-eng-review` | gstack 新旧方案对比 |
| 3. 之后 | 接流程A 步骤2-9 | 同新功能流程 |

> **关键约束:** 接口签名不兼容变更时必须列出所有调用方。有测试覆盖的旧代码，先确认测试是否仍然有效。

---

## 流程D: 模块优化重构

**核心原则:** 测试先行作为安全网，小步重构，每步可独立验证。

```
补测试 → 识别瓶颈 → 小步重构 → 逐步验证 → 审查 → 提交
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 补测试 | 编码 | 如果模块测试覆盖不足，**先补测试**建立安全网 |
| 2. 识别瓶颈 | `/plan-eng-review` | gstack 定位性能/结构问题，定重构边界 |
| 3. 拆解计划 | `/writing-plans` | 拆为小步重构，每步可独立跑测试验证 |
| 4. 逐步重构 | `/executing-plans` | 逐步执行：改 → `php artisan test` → 绿 → 下一步 |
| 5. 每步审查 | `/requesting-code-review` | 每完成一步后 |
| 6. 全量测试 | `php artisan test` + `./vendor/bin/pint --test` | 确认无回归 |
| 7. 审查 | `/review` | gstack |
| 8. 更新文档 | `/document-release` | 更新架构文档、CLAUDE.md 对应章节 |
| 9. 提交 | `/ship` | gstack |

> **禁止:** 没有测试安全网就重构。跨多模块一次性大改。重构同时加新功能。

---

## 流程E: 功能评估

**核心原则:** 基于事实分析，不凭直觉判断。评估结论必须有代码/数据支撑。

```
明确目标 → 信息收集 → 分析评估 → 输出结论
  gstack     Explore      gstack      gstack
             + pwf                    + pwf
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 明确目标 | `/office-hours` | 澄清评估范围、维度、判定标准 |
| 2. 收集信息 | 读代码 + Explore 子代理 | 理清现状：架构、调用链、数据流、性能数据、瓶颈 |
| 3. 分析评估 | `/plan-eng-review` | 多维度分析（可行性/风险/成本/收益），至少对比 2 种方案 |
| 4. 输出结论 | 写入 findings.md | 结论+建议+行动计划，标注关键证据来源 |

**评估维度参考**:

| 维度 | 关注点 |
|------|--------|
| 技术可行性 | 现有架构是否支持？需要改多少？引入新依赖？ |
| 风险 | 是否影响核心流程？回滚难度？数据一致性？ |
| 成本 | 开发量、测试量、运维复杂度 |
| 收益 | 解决什么问题？影响多少用户？可量化吗？ |
| 替代方案 | 有没有更简单的做法？不做的后果？ |

> **禁止:** 没看代码就下结论。只列优点不提风险。评估后直接开写（必须先输出结论文档）。

---

## 流程F: 紧急修复

**核心原则:** 线上优先止损，恢复服务第一，根因分析第二。事后必须补测+复盘。

```
定位止损 → 最小修复 → 验证上线 → 事后补测
investigate   编码       verify     TDD(事后)
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 定位止损 | `/investigate` | gstack 快速定位根因，**先止血**（回滚/切流/关功能） |
| 2. 最小修复 | 编码 | 只改必要代码，跳过完整 TDD。**禁止顺手重构** |
| 3. 验证上线 | `/ship` | gstack 快速确认修复有效，提交推送 |
| 4. 事后补测 | `/test-driven-dev` | **事后必做**：补复现测试 + 回归测试 |
| 5. 复盘 | 写入 findings.md | 记录根因、修复过程、预防措施 |

> **规则:** 流程F 可跳过 `/review` 和 TDD（当时），但事后必须补测试。同一问题出现 2 次紧急修复 → 升级为流程C（重做）。**禁止**以"紧急"为借口跳过事后补测。

---

## 流程G: 依赖升级

**核心原则:** 升级不改功能，兼容性检查优先，保留回滚路径。

```
兼容分析 → 升级计划 → 逐步升级 → 全量测试 → 审查 → 更新文档 → 提交
```

| 步骤 | 命令 | 说明 |
|------|------|------|
| 1. 兼容分析 | 读 CHANGELOG + Explore | 确认 breaking changes、废弃 API、新要求（PHP/Core 版本） |
| 2. 升级计划 | `/plan-eng-review` | gstack 确定升级范围、顺序、回滚方案 |
| 3. 逐步升级 | 编码 | 逐包升级：`composer update pkg` → test → 绿 → 下一个 |
| 4. 全量测试 | `php artisan test` + `./vendor/bin/pint --test` | 确认无回归 |
| 5. 审查 | `/review` | gstack |
| 6. 更新文档 | `/document-release` | 更新 CLAUDE.md 技术栈版本、composer.json 说明 |
| 7. 提交 | `/ship` | gstack |

**升级清单**:

| 检查项 | 说明 |
|--------|------|
| CHANGELOG | 必读每个包的 release notes |
| breaking changes | 搜索 `BREAKING`、`deprecated`、`removed` |
| 版本约束 | 确认 `composer.json` 版本范围正确 |
| 环境要求 | PHP 版本、扩展、系统依赖 |
| 锁文件 | 提交 `composer.lock` 变更 |
| 回滚方案 | 记录回滚步骤，必要时保留旧版分支 |

> **禁止:** 不看 CHANGELOG 就升级。多包混合批量升级。升级同时加新功能或重构。无回滚方案直接升级。

---

## 测试隔离规则

技能测试或子代理实验**禁止在主工作区执行**，必须用 git worktree 隔离：

```bash
git worktree add /tmp/task-test -b test/task-name
# 子代理在 /tmp/task-test 里操作
git worktree remove /tmp/task-test --force
git branch -D test/task-name
```

## 持久化规则（强制，planning-with-files）

| 时机 | 操作 |
|------|------|
| 会话开始 | 检查 task_plan.md 是否存在，读取当前进度 |
| 每完成一个 Task | 更新 task_plan.md（打勾）+ progress.md（记录） |
| 发现关键信息 | 写入 findings.md |
| /clear 之前 | **必须先更新 progress.md** |
| 上下文达 60% | 主动 /clear + 依赖 progress.md 恢复 |

## 文档更新规则（强制）

代码变更后必须确保项目文档与代码一致：

| 变更类型 | 更新内容 | 命令 |
|----------|----------|------|
| 新功能/新接口 | 路由文档、API 文档、CLAUDE.md 技术栈 | `/document-release` |
| Bug 修复 | 如有根因值得记录 → findings.md → 必要时更新 docs | `/document-release` |
| 功能重做 | 架构文档、接口文档、调用方说明 | `/document-release` |
| 模块重构 | 架构文档、CLAUDE.md 对应章节 | `/document-release` |
| 紧急修复 | 复盘记录 → findings.md + 根因文档（事后） | 写入 findings.md |
| 依赖升级 | CLAUDE.md 技术栈版本、composer.json 说明 | `/document-release` |

**原则**: 改了什么，就更新对应的描述文档。文档落后于代码 = 技术债务。

## 禁止事项

- ❌ 直接进入 EnterPlanMode（必须先通过 gstack/Superpowers 规划）
- ❌ 修改代码前不写测试
- ❌ /clear 前不更新 progress.md
- ❌ /investigate 之前修改代码
- ❌ 跳过 /review 直接 /ship
- ❌ 重构时无测试安全网
- ❌ 技能测试/子代理实验在主工作区执行

## 常见错误

| 错误 | 后果 | 正确做法 |
|------|------|---------|
| Bug没定位就改代码 | 治标不治本、引入新bug | 先 /investigate |
| 重构无测试安全网 | 引入回归 | 先补测试 |
| 重做不理解旧接口 | 破坏调用方 | 先分析所有调用方 |
| 跳过阶段1直接写代码 | 方案错误 | 新功能必走 /plan-eng-review |
| 不更新 progress.md | /clear 后丢失进度 | 每次变更后更新 |
| 内部思考代替工具调用 | 缺少审查 | 必须 invoke Skill 工具 |

## Red Flags

- "这个bug很简单，直接改" → 简单bug也要写复现测试
- "重构顺手加个功能" → 重构和新功能必须分开
- "测试后面再补" → TDD 不允许反转
- "旧代码我大概懂了" → 必须确认所有调用方
- "跳过审查直接发布" → /review 是强制步骤
- "线上紧急，先改了再说" → 走流程F，事后必补测试
- "升级应该没风险" → 先读 CHANGELOG，必须有回滚方案
