# SansuAI Skills

Claude Code 插件市场包，提供游戏调研相关的标准化技能集。

## 安装

### 方式一：本地安装
```bash
claude plugin install https://github.com/bit-collusion/SansuAISkills
```

### 方式二：添加为 Marketplace 源
```bash
claude plugin marketplace add bit-collusion/SansuAISkills
claude plugin install game-research-skills@bit-collusion/SansuAISkills
```

## Skills 列表

| 名称 | 描述 |
|------|------|
| **gaas-game-research** | 联网调研一款 GaaS 游戏的持续运营方式，按标准化维度框架生成评分画像。适用于想了解游戏运营结构、内容更新节奏、玩家代入方式的场景。 |
| **premium-game-research** | 联网调研一款买断制游戏的体验特征与产品结构，按标准化维度框架生成评分画像。适用于想了解游戏叙事设计、核心循环、世界结构或整体体验特征的场景。 |

## 使用触发词

### gaas-game-research
- 「调研一款游戏」
- 「分析XX游戏的运营」
- 「给XX游戏打分」
- 「XX游戏的内容组织方式」
- 「生成游戏属性维度表」

### premium-game-research
- 「调研一款买断制游戏」
- 「分析XX游戏的设计」
- 「给XX单机游戏打分」
- 「XX游戏的Core Game Loop」
- 「生成买断制游戏维度表」

## 目录结构

```
.
├── .claude-plugin/
│   ├── plugin.json          # 插件清单
│   └── marketplace.json     # Marketplace 索引
└── skills/
    ├── gaas-game-research/
    │   ├── SKILL.md
    │   ├── expansion-lifecycle/
    │   ├── gaas-score-review/
    │   └── references/
    └── premium-game-research/
        ├── SKILL.md
        ├── premium-score-review/
        └── references/
```

## License

MIT
