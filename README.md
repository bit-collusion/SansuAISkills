# SansuAI Skills

Claude Code 技能集，提供游戏调研相关的标准化 SKILL.md。

## 安装

### 方式一：通过 Marketplace 安装（推荐）

将本仓库添加为 Claude Code marketplace，然后安装 plugin：

```bash
/plugin marketplace add https://github.com/bit-collusion/SansuAISkills
/plugin install game-research@sansuai-skills
```

安装后，Claude Code 会自动加载 skills。更新 marketplace：

```bash
/plugin marketplace update
```

### 方式二：项目级手动安装

仅对当前项目生效，技能文件跟随仓库共享给团队成员。

```bash
# 在项目根目录执行
mkdir -p .claude/skills
cp -r skills/* .claude/skills/
```

Windows (PowerShell):
```powershell
New-Item -ItemType Directory -Force -Path .claude\skills
Copy-Item -Recurse -Force skills\* .claude\skills\
```

### 方式三：用户级手动安装

对所有项目生效，技能文件保存在用户目录下。

**macOS/Linux:**
```bash
mkdir -p ~/.claude/skills
cp -r skills/* ~/.claude/skills/
```

**Windows (PowerShell):**
```powershell
$skillsDir = "$env:USERPROFILE\.claude\skills"
New-Item -ItemType Directory -Force -Path $skillsDir
Copy-Item -Recurse -Force skills\* $skillsDir\
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
│   ├── marketplace.json    # Marketplace 定义
│   └── plugin.json         # Plugin 清单
├── skills/
│   ├── gaas-game-research/
│   │   ├── SKILL.md
│   │   ├── expansion-lifecycle/
│   │   ├── gaas-score-review/
│   │   └── references/
│   └── premium-game-research/
│       ├── SKILL.md
│       ├── premium-score-review/
│       └── references/
└── README.md
```

## License

MIT
