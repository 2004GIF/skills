# Skills

Claude Code 技能集合 — 可复用的 AI 编码技能库，为 Claude Code 提供专业领域知识和代码生成能力。

## 什么是 Skill？

Skill 是 Claude Code 的扩展模块，包含特定领域的专业知识、代码模板和可复用模式。每个 Skill 都是一个独立目录，包含：

- `SKILL.md` — 技能定义文件，描述技能的目标、输入、输出和执行流程
- `templates/` — 代码模板文件（可选）
- `references/` — 参考文档和示例（可选）

## 可用技能

| 技能 | 描述 | 适用场景 |
|------|------|----------|
| [ruoyi-code-generator](skills/ruoyi-code-generator/SKILL.md) | 基于若依(RuoYi)框架规范生成 CRUD 代码 | 创建新模块、添加业务功能、根据表结构生成前后端代码 |

### ruoyi-code-generator

根据数据表信息自动生成完整的 CRUD 代码，支持三种技术栈组合：

| 选项 | 前端 | 后端 ORM |
|------|------|----------|
| 选项一 | Vue 2 + Element UI | MyBatis |
| 选项二 | Vue 3 + Element Plus | MyBatis |
| 选项三 | Vue 3 + Element Plus | MyBatis-Plus |

**生成内容：**
- Java 后端代码（Domain、Mapper、Service、ServiceImpl、Controller）
- MyBatis XML 映射文件
- Vue 前端页面组件（列表页、树形页、详情页）
- 前端 API 封装（JavaScript）
- 菜单初始化 SQL

## 目录结构

```
skills/
├── ruoyi-code-generator/          # 若依代码生成器
│   ├── SKILL.md                   # 技能定义
│   ├── references/                # 参考文档与示例
│   │   ├── coding-standards.md    # 编码规范
│   │   ├── examples.md            # 选项一/二 示例
│   │   └── examples-mybatisPlus.md # 选项三 示例
│   └── templates/                 # 代码模板
│       ├── java/                  # Java 后端模板
│       │   └── mybatis-plus/      # MyBatis-Plus 变体
│       ├── xml/                   # MyBatis XML 模板
│       ├── js/                    # 前端 API 模板
│       ├── vue/                   # Vue 2 模板
│       │   ├── v3/                # Vue 3 JS 模板
│       │   └── v3ts/              # Vue 3 TS 模板
│       └── sql/                   # 菜单 SQL 模板
└── README.md
```

## 安装与使用

### 安装技能

将技能目录复制到 Claude Code 的 skills 目录：

```bash
# 安装单个技能
cp -r skills/ruoyi-code-generator ~/.claude/skills/

# 或安装全部技能
cp -r skills/* ~/.claude/skills/
```

### 使用技能

在 Claude Code 会话中，通过以下方式调用技能：

```
/ruoyi-code-generator
```

或直接在对话中描述需求，Claude 会自动匹配并加载对应技能。

## 贡献指南

欢迎贡献新的 Skill！创建 Skill 时请遵循以下结构：

```
skills/
└── your-skill-name/
    ├── SKILL.md           # 必需：技能定义（含 name、description frontmatter）
    ├── templates/         # 可选：代码模板目录
    └── references/        # 可选：参考文档目录
```

`SKILL.md` 的 frontmatter 格式：

```yaml
---
name: your-skill-name
description: |
  技能的简要描述，说明其用途和适用场景。
---
```

## License

MIT
