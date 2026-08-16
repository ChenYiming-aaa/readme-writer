<h1 align="center">readme-writer ✨</h1>

> 一个让 AI 自动生成专业 README 的 Agent 技能（Skill），支持 6 种技术栈模板与中英双语。

<p align="center">
  <img src="https://img.shields.io/github/license/ChenYiming-aaa/readme-writer" alt="license" />
  <img src="https://img.shields.io/github/last-commit/ChenYiming-aaa/readme-writer" alt="last commit" />
  <img src="https://img.shields.io/github/languages/count/ChenYiming-aaa/readme-writer" alt="languages" />
  <img src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Node-339933?logo=node.js&logoColor=white" alt="Node" />
  <img src="https://img.shields.io/badge/standard--readme-1.0.0-blue" alt="standard-readme" />
</p>

> 🤖 本 README 就是由本技能自己生成的，快来体验吧 (≧▽≦)ﾉ

---

## 功能特性

- **🎯 自动检测技术栈** — 扫描 `package.json`、`Cargo.toml`、`go.mod`、`pom.xml` 等 9 类清单文件，自动识别 Node/Python/Rust/Go/Java 等项目
- **🌍 中英双语支持** — 支持纯中文、纯英文、双语（`docs/i18n/`）三种模式，自动生成语言切换器
- **📛 专业徽章系统** — 内置 shields.io 徽章模板（版本/许可证/下载量/构建状态），动态或静态徽章均可
- **📋 遵循 standard-readme 规范** — 13 段标准结构 + 6 种可扩展章节，生成物即 GitHub 社区标准格式
- **🤖 全程交互确认** — 每个决策点向你确认，绝不猜测；信息足够时自动走快速通道
- **✅ 内置质量检查** — 13 项静默检查（无占位符、无死链、代码块带语言标识等）

## 快速开始

1. 将本仓库的 `SKILL.md` 复制到你的 AI 助手技能目录：

   ```bash
   # Claude Code
   mkdir -p ~/.claude/skills/readme-writer
   cp SKILL.md ~/.claude/skills/readme-writer/

   # OpenCode
   mkdir -p ~/.config/opencode/skills/readme-writer
   cp SKILL.md ~/.config/opencode/skills/readme-writer/
   ```

2. 在任意项目目录中直接说：

   ```
   帮我写一个 README
   ```

3. 回答几个问题（语言、特性、安装方式），然后坐等生成 🎉

## 使用示例

对话式交互，全自动生成：

```text
你: 帮我写个 README，这是一个 Python 爬虫库
AI: 检测到 pyproject.toml，是 Python 项目 ✓
    用中文还是英文？ [English / 中文 / Both]
你: 中文
AI: 已生成草稿（标题、9 个章节、中文模式）……
    质量检查通过 ✓  开始写入 README.md？
你: 好
AI: ✅ 完成！README.md 已写入项目根目录
```

生成效果就是你现在看到的这份文档 (๑•̀ㅂ•́)و✧

## 技术栈

| 类别 | 支持 |
|------|------|
| 语言模板 | Node.js、Python、Rust、Go、Java、通用 |
| 清单检测 | package.json、pyproject.toml、Cargo.toml、go.mod、pom.xml、build.gradle、composer.json、Dockerfile、index.html |
| 输出语言 | 中文、英文、双语（docs/i18n/） |
| 兼容助手 | OpenCode、Claude Code、Codex CLI |

## 项目结构

```
readme-writer/
├── SKILL.md   # 技能本体（含 6 套模板、徽章系统、质量检查清单）
└── LICENSE    # MIT 许可证
```

## 相关链接

- [standard-readme 规范](https://github.com/RichardLitt/standard-readme)
- [shields.io 徽章](https://shields.io)

## 贡献指南

欢迎提交 Issue 或 PR 完善模板与流程 (｡•̀ᴗ-)✧

## 许可证

[MIT](LICENSE) © readme-writer contributors
