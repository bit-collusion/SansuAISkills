# Game Research Plugin 重构与路由 Skill 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 game-research plugin 文件集中到 `plugins/game-research/` 目录，新增路由 Skill 实现游戏类型自动检测（GaaS vs 买断制），收窄子 Skill 触发描述。

**架构:** 采用 Marketplace → Plugin → Skill 三层目录结构。Plugin 内新增 `game-research` 路由 Skill 负责类型检测和框架分发；两个子 Skill 保留独立触发能力，但描述收窄以降低误触。

**Tech Stack:** Markdown (SKILL.md), JSON (marketplace.json, plugin.json)

---

### Task 1: 创建 Plugin 目标目录结构

**Files:**
- Create: `plugins/game-research/.claude-plugin/plugin.json`
- Create: `plugins/game-research/skills/game-research/SKILL.md`

- [ ] **Step 1: 创建目录结构**

Run:
```bash
mkdir -p plugins/game-research/.claude-plugin
mkdir -p plugins/game-research/skills/game-research
```

Expected: 目录创建成功，无报错。

- [ ] **Step 2: Commit 目录骨架**

```bash
git add plugins/
git commit -m "chore: create game-research plugin directory structure

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 2: 迁移 plugin.json

**Files:**
- Create: `plugins/game-research/.claude-plugin/plugin.json`
- Delete: `.claude-plugin/plugin.json` (根目录)

- [ ] **Step 1: 创建迁移后的 plugin.json**

Create `plugins/game-research/.claude-plugin/plugin.json`:
```json
{
  "name": "game-research",
  "description": "联网调研游戏的 Claude Code skills，包含 GaaS 游戏运营调研与买断制游戏调研",
  "version": "1.0.0"
}
```

- [ ] **Step 2: 删除根目录旧文件**

Run:
```bash
git rm .claude-plugin/plugin.json
```

- [ ] **Step 3: Commit**

```bash
git add plugins/game-research/.claude-plugin/plugin.json .claude-plugin/plugin.json
git commit -m "refactor: move plugin.json into plugins/game-research/

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 3: 迁移 GaaS 调研 Skill

**Files:**
- Create: `plugins/game-research/skills/gaas-game-research/SKILL.md`
- Create: `plugins/game-research/skills/gaas-game-research/expansion-lifecycle/SKILL.md`
- Create: `plugins/game-research/skills/gaas-game-research/gaas-score-review/SKILL.md`
- Create: `plugins/game-research/skills/gaas-game-research/references/framework-definition.md`
- Create: `plugins/game-research/skills/gaas-game-research/references/lifecycle-dimensions.md`
- Create: `plugins/game-research/skills/gaas-game-research/references/reference-scores.md`
- Delete: `skills/gaas-game-research/` (根目录)

- [ ] **Step 1: 复制所有 GaaS Skill 文件**

Run:
```bash
cp -r skills/gaas-game-research/* plugins/game-research/skills/gaas-game-research/
```

- [ ] **Step 2: 修改 gaas-game-research/SKILL.md 触发描述**

Modify `plugins/game-research/skills/gaas-game-research/SKILL.md` line 1-4:

Old:
```yaml
---
name: gaas-game-research
description: 联网调研一款 GaaS 游戏的持续运营方式，按标准化维度框架生成评分画像。当用户提到「调研一款游戏」「分析XX游戏的运营」「给XX游戏打分」「XX游戏的内容组织方式」「生成游戏属性维度表」等意图时触发。只要用户想了解一款具体游戏的运营结构、内容更新节奏、玩家代入方式，就应该使用这个技能。
---
```

New:
```yaml
---
name: gaas-game-research
description: 联网调研一款 GaaS（持续运营）游戏的运营方式，按标准化维度框架生成评分画像。当用户明确提到「调研一款 GaaS 游戏」「分析XX游戏的运营」「给XX持续运营游戏打分」「XX游戏的赛季/通行证/抽卡」「Live Service」「MMO」「手游运营」等意图时触发。不适用于买断制/单机游戏——那些应使用 game-research 路由技能或 premium-game-research 技能。
---
```

- [ ] **Step 3: 删除旧文件并 Commit**

Run:
```bash
git rm -r skills/gaas-game-research/
```

```bash
git add plugins/game-research/skills/gaas-game-research/
git commit -m "refactor: migrate gaas-game-research skill into plugin subdirectory

- Narrow trigger description to GaaS-specific keywords
- Remove root-level skills/gaas-game-research/

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 4: 迁移买断制调研 Skill

**Files:**
- Create: `plugins/game-research/skills/premium-game-research/SKILL.md`
- Create: `plugins/game-research/skills/premium-game-research/premium-score-review/SKILL.md`
- Create: `plugins/game-research/skills/premium-game-research/references/framework-definition-premium.md`
- Create: `plugins/game-research/skills/premium-game-research/references/reference-scores-premium.md`
- Delete: `skills/premium-game-research/` (根目录)

- [ ] **Step 1: 复制所有买断制 Skill 文件**

Run:
```bash
cp -r skills/premium-game-research/* plugins/game-research/skills/premium-game-research/
```

- [ ] **Step 2: 修改 premium-game-research/SKILL.md 触发描述**

Modify `plugins/game-research/skills/premium-game-research/SKILL.md` line 1-4:

Old:
```yaml
---
name: premium-game-research
description: 联网调研一款买断制游戏的体验特征与产品结构，按标准化维度框架生成评分画像。当用户提到「调研一款买断制游戏」「分析XX游戏的设计」「给XX单机游戏打分」「XX游戏的Core Game Loop」「生成买断制游戏维度表」等意图时触发。只要用户想了解一款买断制游戏的叙事设计、核心循环、世界结构或整体体验特征，就应该使用这个技能。不适用于 GaaS/持续运营类游戏——那些应使用 gaas-game-research 技能。
---
```

New:
```yaml
---
name: premium-game-research
description: 联网调研一款买断制（Premium / Buy-to-Play）游戏的体验特征与产品结构，按标准化维度框架生成评分画像。当用户明确提到「调研一款买断制游戏」「分析XX单机游戏的设计」「给XX单机游戏打分」「XX游戏的Core Game Loop」「生成买断制游戏维度表」等意图时触发。只要用户想了解一款买断制游戏的叙事设计、核心循环、世界结构或整体体验特征，就应该使用这个技能。不适用于 GaaS/持续运营类游戏——那些应使用 game-research 路由技能或 gaas-game-research 技能。
---
```

- [ ] **Step 3: 删除旧文件并 Commit**

Run:
```bash
git rm -r skills/premium-game-research/
```

```bash
git add plugins/game-research/skills/premium-game-research/
git commit -m "refactor: migrate premium-game-research skill into plugin subdirectory

- Narrow trigger description to premium-specific keywords
- Remove root-level skills/premium-game-research/

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 5: 更新 marketplace.json

**Files:**
- Modify: `.claude-plugin/marketplace.json`

- [ ] **Step 1: 更新 source 路径**

Modify `.claude-plugin/marketplace.json`:

Old:
```json
      "source": "./",
```

New:
```json
      "source": "./plugins/game-research",
```

- [ ] **Step 2: Commit**

```bash
git add .claude-plugin/marketplace.json
git commit -m "refactor: update marketplace.json source to plugin subdirectory

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 6: 创建路由 Skill

**Files:**
- Create: `plugins/game-research/skills/game-research/SKILL.md`

- [ ] **Step 1: 写入路由 Skill 内容**

Create `plugins/game-research/skills/game-research/SKILL.md`:
```markdown
---
name: game-research
description: 联网调研一款游戏的完整画像。当用户提到「调研一款游戏」「分析XX游戏」「给XX游戏打分」「生成游戏属性维度表」「XX游戏怎么样」等通用游戏调研意图时触发。只要用户想了解一款具体游戏的运营结构、体验特征或内容组织方式，且没有明确指定游戏类型（GaaS 或买断制），就应该使用这个技能。本技能会自动判断游戏类型并调用对应的子技能框架。
---

# 游戏调研路由技能

对一款游戏进行联网调研，首先自动判断其商业模式类型（GaaS 持续运营 vs 买断制 Premium），然后调用对应的子技能框架生成完整的属性画像报告。

## 输入

用户提供一款游戏的名称。如果用户没有指定输出路径和文件名，主动询问。

## 工作流程

### 第一步：类型判断

使用 WebSearch 搜索该游戏的商业模式信息，搜索关键词：
- `游戏名 business model`
- `游戏名 monetization`
- `游戏名 收费方式`
- `游戏名 是网游吗`
- `游戏名 free to play`

根据搜索结果判断游戏类型：

| 判断依据 | 类型 |
|----------|------|
| 免费+内购、赛季/通行证、抽卡/Gacha、Live Service、MMO、持续内容更新 | GaaS |
| 一次性购买、DLC/扩展包、单机、无持续运营机制 | 买断制 |
| 兼具两种特征（如 GTA Online）| 以主要模式为准，默认优先 GaaS |

如果搜索结果无法明确判断，询问用户：「这款游戏是买断制单机还是持续运营类（GaaS）游戏？」

### 第二步：调用对应子技能

- 判断为 **GaaS** → 读取并完整执行 `skills/gaas-game-research/SKILL.md` 的工作流程
- 判断为 **买断制** → 读取并完整执行 `skills/premium-game-research/SKILL.md` 的工作流程

**注意：** 子技能包含完整的框架定义读取、联网调研、事实核查、逐维度评分、报告生成流程。路由技能本身不替代这些步骤，而是负责启动正确的框架。

## 注意事项

- 如果用户已经明确指定了类型（如「用 GaaS 框架调研原神」），直接调用对应子技能，跳过类型判断
- 如果用户说「调研看门狗2」这种未指明类型的请求，执行完整类型判断流程
- 如果游戏不存在或搜索不到有效信息，告知用户并终止
```

- [ ] **Step 2: Commit**

```bash
git add plugins/game-research/skills/game-research/SKILL.md
git commit -m "feat: add game-research router skill with auto type detection

- Detects GaaS vs Premium before dispatching to sub-skills
- Searches business model info to determine correct framework
- Falls back to user confirmation when ambiguous

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

### Task 7: 清理根目录 skills 空文件夹并验证

**Files:**
- Delete: `skills/` (根目录，如为空)

- [ ] **Step 1: 检查并清理**

Run:
```bash
rmdir skills 2>/dev/null || true
```

- [ ] **Step 2: 验证最终目录结构**

Run:
```bash
find . -type f -not -path './.git/*' -not -path './.claude/*' -not -path './.playwright-mcp/*' | sort
```

Expected output 应包含：
```
./.claude-plugin/marketplace.json
./plugins/game-research/.claude-plugin/plugin.json
./plugins/game-research/skills/game-research/SKILL.md
./plugins/game-research/skills/gaas-game-research/SKILL.md
./plugins/game-research/skills/gaas-game-research/expansion-lifecycle/SKILL.md
./plugins/game-research/skills/gaas-game-research/gaas-score-review/SKILL.md
./plugins/game-research/skills/gaas-game-research/references/framework-definition.md
./plugins/game-research/skills/gaas-game-research/references/lifecycle-dimensions.md
./plugins/game-research/skills/gaas-game-research/references/reference-scores.md
./plugins/game-research/skills/premium-game-research/SKILL.md
./plugins/game-research/skills/premium-game-research/premium-score-review/SKILL.md
./plugins/game-research/skills/premium-game-research/references/framework-definition-premium.md
./plugins/game-research/skills/premium-game-research/references/reference-scores-premium.md
```

不应再包含：
- `./.claude-plugin/plugin.json`（根目录的 plugin.json）
- `./skills/gaas-game-research/...`
- `./skills/premium-game-research/...`

- [ ] **Step 3: Commit 清理（如有变更）**

```bash
git add -A
git diff --cached --quiet || git commit -m "chore: clean up root-level skills directory after migration

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>"
```

---

## Spec 覆盖检查

| 设计需求 | 对应任务 |
|----------|----------|
| Plugin 文件集中到 `plugins/game-research/` | Task 1-4, 7 |
| 新增路由 Skill 实现类型检测 | Task 6 |
| 收窄子 Skill 触发描述 | Task 3 (Step 2), Task 4 (Step 2) |
| 更新 marketplace.json source 路径 | Task 5 |
| 边界情况处理（已明确类型、兼具特征、搜索不到）| Task 6 (Skill 内容) |

## Placeholder 检查

- [x] 无 "TBD" / "TODO"
- [x] 无 "implement later" / "fill in details"
- [x] 无 "add appropriate error handling" 等模糊描述
- [x] 所有文件路径明确
- [x] 所有代码/配置内容完整
