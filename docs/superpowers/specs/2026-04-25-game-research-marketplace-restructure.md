# Game Research Plugin 重构设计

## 背景

当前 marketplace 的 `game-research` plugin 将 GaaS 和买断制两套调研框架放在同一个 plugin 的不同 skill 目录下，但未做统一路由。当用户发出"调研一款游戏"这种未指明类型的请求时，Claude Code 可能错误地将买断制游戏（如看门狗2）路由到 GaaS 调研框架。

此外，考虑到未来 marketplace 可能包含多个 plugins，需将 plugin 相关文件集中封装到独立目录中。

## 目标

1. 将 `game-research` plugin 的所有文件集中到 `plugins/game-research/` 目录下
2. 新增一个路由 Skill，在调研前自动判断游戏的商业模式类型（GaaS vs 买断制）
3. 收窄两个子 Skill 的触发描述，降低误触概率

## 架构

### 目录结构

```
.claude-plugin/marketplace.json
plugins/game-research/.claude-plugin/plugin.json
plugins/game-research/skills/game-research/SKILL.md              ← 新增：路由 Skill
plugins/game-research/skills/gaas-game-research/SKILL.md         ← 现有内容迁移
plugins/game-research/skills/gaas-game-research/expansion-lifecycle/SKILL.md
plugins/game-research/skills/gaas-game-research/gaas-score-review/SKILL.md
plugins/game-research/skills/gaas-game-research/references/...
plugins/game-research/skills/premium-game-research/SKILL.md      ← 现有内容迁移
plugins/game-research/skills/premium-game-research/premium-score-review/SKILL.md
plugins/game-research/skills/premium-game-research/references/...
```

### marketplace.json 变更

```json
{
  "name": "sansuai-skills",
  "owner": { "name": "bit-collusion" },
  "metadata": { "description": "...", "version": "1.0.0" },
  "plugins": [
    {
      "name": "game-research",
      "source": "./plugins/game-research",
      "description": "...",
      "version": "1.0.0",
      "author": { "name": "bit-collusion" },
      "license": "MIT",
      "keywords": ["game", "research", "gaas", "premium", "review", "analysis"]
    }
  ]
}
```

- `source` 从 `"./"` 改为 `"./plugins/game-research"`
- 根目录的 `plugin.json` 移入 `plugins/game-research/.claude-plugin/`

### 路由 Skill（game-research）

**触发描述**：当用户提到「调研一款游戏」「分析XX游戏」「给XX游戏打分」「生成游戏属性维度表」等通用游戏调研意图时触发。

**工作流程**：
1. 接收游戏名称
2. 快速搜索该游戏的商业模式（搜索词：`游戏名 business model`、`游戏名 收费方式`、`游戏名 是网游吗`）
3. 判断类型：
   - GaaS 特征：免费+内购、赛季、通行证、抽卡、Live Service、MMO、持续运营
   - 买断制特征：一次性购买、DLC、单机、无持续运营
4. 判断为 GaaS → 读取并执行 `skills/gaas-game-research/SKILL.md` 的完整工作流
5. 判断为买断制 → 读取并执行 `skills/premium-game-research/SKILL.md` 的完整工作流
6. 无法判断 → 询问用户确认

### 子 Skill 触发描述调整

- **gaas-game-research**：仅匹配「调研一款 GaaS 游戏」「分析XX游戏的运营」「持续运营」「赛季」「通行证」「抽卡」「Live Service」「MMO」等关键词
- **premium-game-research**：仅匹配「调研一款买断制游戏」「分析XX单机游戏」「给XX单机游戏打分」「生成买断制游戏维度表」「Core Game Loop」等关键词

## 边界情况处理

| 场景 | 处理策略 |
|------|----------|
| 用户已明确指定类型 | 路由 Skill 直接调用对应框架，跳过检测 |
| 游戏兼具两种特征（如 GTA Online）| 以主要模式为准，默认优先 GaaS 框架 |
| 搜索不到游戏 | 告知用户并终止 |
| 检测结果与已知事实矛盾 | 以已知事实为准，跳过检测 |
