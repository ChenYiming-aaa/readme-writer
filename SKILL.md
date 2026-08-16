---
name: readme-writer
description: >
  Generate professional README for any project. Auto-detects tech stack from
  manifest files, follows standard-readme spec with shields.io badges.
  Supports English & Chinese (bilingual mode). Use when user says "write
  README", "generate README", "improve README", or "add badges".
license: MIT
compatibility:
  - opencode
  - claude-code
  - codex-cli
metadata:
  author: opencode-user
  version: "2.1.0"
  languages: en, zh
---

# README Writer

Professional README generator that follows [standard-readme](https://github.com/RichardLitt/standard-readme) spec and top open-source project patterns (React, Vue, Tailwind CSS, FastAPI, Express, Docker).

## Core Principle

**DO NOT assume. ASK at every decision point.** The user knows their project best. Your job is to guide, not guess.

**Exception — Fast Path:** If the user already supplied enough context (tech stack, language, description, install method) in their first message, skip straight to Step 3 and draft. Do not re-ask what they already told you.

## Workflow

### Step 1: Scan Project → Show Results → Confirm

Use file tools (Glob/Read), not shell commands, to detect manifest files. Check project root for:

- `package.json` (Node.js)
- `pyproject.toml` / `setup.py` / `requirements.txt` (Python)
- `Cargo.toml` (Rust)
- `go.mod` (Go)
- `pom.xml` / `build.gradle` (Java)
- `composer.json` (PHP)
- `Dockerfile` / `docker-compose.yml` (Docker)
- `index.html` / `src/*.vue` / `src/*.jsx` (Web frontend)

Present findings, then use the `question` tool (ONE call, multiple questions is fine):

```
Detected: package.json (Node.js), tsconfig.json, tailwind.config.js
I'll extract name, version, description, and dependencies from package.json.

[Q1] Proceed with these? [Yes / Let me add more info]
[Q2] Do you have a GitHub owner/repo for dynamic badges? (e.g. "user/repo" / No, use static)
```

If user selects "Let me add more info", use follow-up questions to collect missing data.

If NO manifest file is found, ask directly:

```
What's your project's tech stack?
  [Node.js / Python / Rust / Go / Java / PHP / Ruby / C/C++ / Docker / Other]
```

### Step 2: Ask Language Preference

**Mandatory question — always ask before generating** (unless already stated):

```
Use `question` tool:
"Which language for the README?
  [English / 中文 / Both (bilingual) / Other (specify)]"
```

- **English**: `README.md` in English. Optionally ask if they want translations.
- **中文**: `README.md` in Chinese. Optionally ask if they want translations.
- **Both (bilingual)**: primary language goes in `README.md`, second language in `docs/i18n/README.{lang}.md` (see Bilingual Mode rules below). Primary defaults to Chinese unless user requests English-first.
- **Other (specify)**: use a follow-up question to get the exact language name (e.g., 日本語, 한국어, Español, Français, Deutsch). Generate `README.md` in that single language.

If user selects "Other", use a follow-up question to get the exact language name. Use that language for all section titles and content. Generate `README.md` in the project root in that single language.

### Step 3: Ask About Project Details

Use the `question` tool. Combine related questions into ONE call to minimize round-trips (the tool accepts multiple questions).

First call:

```
Use `question` tool (batch these):
"1. Describe your project in 1-2 sentences. What does it do and who is it for?"
"2. What are the key features / selling points? (list 3-7)"
"3. How is it installed? (npm/pip/cargo/go get/docker/manual)"
"4. Does it have a CLI, GUI, or library API?"
```

Then follow up based on their answers, covering only what's still unknown:

- Screenshots or demo URL
- Target audience (developers / end-users / researchers)
- License (if no LICENSE file found; suggest MIT for open source)
- Whether they have a CONTRIBUTING.md or Code of Conduct
- Any special sections needed: FAQ, Changelog, Roadmap, Benchmarks, API Reference

### Step 4: Existing README Handling

If the project root already contains a `README.md` (or README.en/zh files), ask BEFORE overwriting:

```
Use `question` tool:
"Found existing README.md. What should I do?
  [Overwrite with new version / Merge (keep useful sections) / Generate a new file elsewhere]"
```

### Step 5: Present Section Plan → Get Approval

Before writing, **output the planned sections as visible text** (so the user can see them), then ask for approval.

First, write the section list as plain visible output:

```
I'll generate this README structure:
  - Title + Badges
  - Features
  - Quick Start
  - Usage Examples
  - Tech Stack
  - Contributing
  - License
```

Then use `question` tool to ask:

```
"Above is the planned section structure. Add or remove any sections?"
```

### Step 6: Generate Draft → Present → Ask for Feedback

Write the README draft to a temp file. Then **output a summary of what was generated** (title, section count, language mode) as visible text, so the user knows what to expect:

```
README draft ready:
  - Title: {name}
  - Sections: 8 ({list key sections})
  - Language: {en/zh/both}
```

Then use `question` tool:

```
"README draft ready above. Any changes?
  [Looks good / Add a section / Rewrite section / Different tone]"
```

If they request changes, iterate. If "Looks good", proceed to quality check.

### Step 7: Quality Check → Final Confirmation

Run through the checklist below silently. Then ask:

```
Use `question` tool:
"Quality check passed:
  - All badges use correct shields.io URLs
  - No placeholder text remaining
  - Code blocks have language identifiers
  - License section is last
  - Quick Start is copy-pasteable

Ready to write README.md?"
```

### Step 8: Write the File(s)

Only after user confirms:
- **Single language**: write `README.md` to the project root (exact uppercase name)
- **Bilingual**: write `README.md` (primary language) + `docs/i18n/README.en.md` (or `.zh-CN.md` if English is primary), creating the `docs/i18n/` directory if needed

## Section Templates

All templates below use 4 backticks (````markdown) for the outer fence so inner language fences parse correctly.

### Template: Node.js Project (English)

````markdown
<h1 align="center">{name}</h1>

> {description}

<p align="center">
  <img src="https://img.shields.io/npm/v/{name}" alt="npm version" />
  <img src="https://img.shields.io/github/license/{owner}/{repo}" alt="license" />
  <img src="https://img.shields.io/npm/dm/{name}" alt="downloads" />
  <img src="https://img.shields.io/badge/node-%3E%3D{nodeVersion}-green" alt="Node" />
</p>

## Features

- **{feature1}** — {benefit1}
- **{feature2}** — {benefit2}
- **{feature3}** — {benefit3}

## Install

```bash
npm install {name}
```

## Usage

```javascript
import { something } from '{name}';

const result = something({ option: 'value' });
console.log(result);
```

## API

### `something(options)`

| Param | Type | Default | Description |
|-------|------|---------|-------------|
| options.option | string | `'default'` | Description here |

## License

{license} © {author}
````

### Template: Python Project (English)

````markdown
<h1 align="center">{name}</h1>

> {description}

<p align="center">
  <img src="https://img.shields.io/pypi/v/{name}" alt="PyPI version" />
  <img src="https://img.shields.io/pypi/pyversions/{name}" alt="Python" />
  <img src="https://img.shields.io/github/license/{owner}/{repo}" alt="license" />
  <img src="https://img.shields.io/pypi/dm/{name}" alt="downloads" />
</p>

## Features

- **{feature1}** — {benefit1}
- **{feature2}** — {benefit2}
- **{feature3}** — {benefit3}

## Install

```bash
pip install {name}
```

## Usage

```python
from {name} import something

result = something(option="value")
print(result)
```

## License

{license} © {author}
````

### Template: Rust Project (English)

````markdown
<h1 align="center">{name}</h1>

> {description}

<p align="center">
  <img src="https://img.shields.io/crates/v/{name}" alt="crates.io version" />
  <img src="https://img.shields.io/github/license/{owner}/{repo}" alt="license" />
  <img src="https://img.shields.io/crates/d/{name}" alt="downloads" />
  <img src="https://img.shields.io/badge/rust-{rustVersion}-orange" alt="Rust" />
</p>

## Features

- **{feature1}** — {benefit1}
- **{feature2}** — {benefit2}
- **{feature3}** — {benefit3}

## Install

Add to your `Cargo.toml`:

```toml
[dependencies]
{name} = "{version}"
```

## Usage

```rust
use {name}::something;

fn main() {
    let result = something(option::Value);
    println!("{:?}", result);
}
```

## License

{license} © {author}
````

### Template: Go Project (English)

````markdown
<h1 align="center">{name}</h1>

> {description}

<p align="center">
  <img src="https://img.shields.io/github/go-mod/go-version/{owner}/{repo}" alt="Go version" />
  <img src="https://img.shields.io/github/license/{owner}/{repo}" alt="license" />
  <img src="https://img.shields.io/badge/go-%3E%3D{goVersion}-blue" alt="Go" />
</p>

## Features

- **{feature1}** — {benefit1}
- **{feature2}** — {benefit2}
- **{feature3}** — {benefit3}

## Install

```bash
go get {modulePath}
```

## Usage

```go
package main

import "{modulePath}"

func main() {
    result := something.Do(option)
    fmt.Println(result)
}
```

## License

{license} © {author}
````

### Template: Java Project (English)

````markdown
<h1 align="center">{name}</h1>

> {description}

<p align="center">
  <img src="https://img.shields.io/maven-central/v/{groupId}/{artifactId}" alt="Maven Central" />
  <img src="https://img.shields.io/github/license/{owner}/{repo}" alt="license" />
  <img src="https://img.shields.io/badge/java-%3E%3D{javaVersion}-red" alt="Java" />
</p>

## Features

- **{feature1}** — {benefit1}
- **{feature2}** — {benefit2}
- **{feature3}** — {benefit3}

## Install

Add to your `pom.xml`:

```xml
<dependency>
    <groupId>{groupId}</groupId>
    <artifactId>{artifactId}</artifactId>
    <version>{version}</version>
</dependency>
```

## Usage

```java
import {package}.{Something};

public class Example {
    public static void main(String[] args) {
        Something result = new Something();
        System.out.println(result.doWork("value"));
    }
}
```

## License

{license} © {author}
````

### Template: Generic Project (English)

````markdown
<h1 align="center">{name}</h1>

> {description}

<p align="center">
  <img src="https://img.shields.io/github/license/{owner}/{repo}" alt="license" />
  <img src="https://img.shields.io/github/last-commit/{owner}/{repo}" alt="last commit" />
  <img src="https://img.shields.io/github/languages/count/{owner}/{repo}" alt="languages" />
</p>

## Features

- **{feature1}** — {benefit1}
- **{feature2}** — {benefit2}
- **{feature3}** — {benefit3}

## Getting Started

{steps}

## License

{license} © {author}
````

## Chinese Templating Rules

When user selects Chinese (zh), all content is in Chinese:

| English | 中文 |
|---------|------|
| Features | 功能特性 |
| Install | 安装 |
| Usage | 使用示例 |
| API | API 参考 |
| Prerequisites | 前置要求 |
| Contributing | 贡献指南 |
| License | 许可证 |
| Quick Start | 快速开始 |
| Configuration | 配置说明 |
| Acknowledgements | 致谢 |
| Tech Stack | 技术栈 |
| Project Structure | 项目结构 |
| Development | 开发指南 |

### Both (Bilingual) Mode

When user selects "Both", generate **two separate files**, one per language. This is the standard GitHub i18n pattern. Each file is a complete, standalone README.

- **`README.md`** — Primary language (Chinese by default)
- **`docs/i18n/README.en.md`** — English translation (when primary is Chinese)
- **`docs/i18n/README.zh-CN.md`** — Chinese translation (when primary is English)

**Naming rule:** the translated file is always `README.<lang>.md` under `docs/i18n/`, where `<lang>` is `en` or `zh-CN` (GitHub convention: `zh-CN`, not `zh`).

Each file includes a language selector at the top linking to the other version.

#### Template: `README.md` (Chinese)

````markdown
<h1 align="center">{name}</h1>

> {description-zh}

<p align="center">
  [badges]
</p>

<p align="center">
  <b>中文</b> · <a href="docs/i18n/README.en.md">English</a>
</p>

---

## 功能特性

...

## 许可证
````

#### Template: `docs/i18n/README.en.md` (English)

````markdown
<h1 align="center">{name}</h1>

> {description-en}

<p align="center">
  [badges]
</p>

<p align="center">
  <a href="../../README.md">中文</a> · <b>English</b>
</p>

---

## Features

...

## License
````

**Rules:**
- `README.md` is always the primary language (Chinese unless user requests otherwise)
- Translation goes to `docs/i18n/README.en.md` or `docs/i18n/README.zh-CN.md` (if English is primary)
- Each file is a complete, standalone README with no cross-file dependencies
- Language selector links at the top linking to the other version
- Badges appear in both files (badges are language-agnostic)
- All section titles, descriptions, code comments, and labels translated according to the chosen language
- Badge labels always remain in English (shields.io standard)

## Badge System

### Essential Badges (include if data available)

| Badge | URL Pattern |
|-------|-------------|
| Build | `https://img.shields.io/github/actions/workflow/status/{owner}/{repo}/ci.yml` |
| Version | `https://img.shields.io/npm/v/{name}` |
| License | `https://img.shields.io/github/license/{owner}/{repo}` |
| Downloads | `https://img.shields.io/npm/dm/{name}` |
| Last Commit | `https://img.shields.io/github/last-commit/{owner}/{repo}` |
| Languages | `https://img.shields.io/github/languages/count/{owner}/{repo}` |
| Code Size | `https://img.shields.io/github/languages/code-size/{owner}/{repo}` |

### Tech Stack Badges

```
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?logo=react&logoColor=61DAFB)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Rust](https://img.shields.io/badge/Rust-000000?logo=rust&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?logo=go&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?logo=openjdk&logoColor=white)
![Node](https://img.shields.io/badge/Node-339933?logo=node.js&logoColor=white)
```

### Static Badge Format

```
![label](https://img.shields.io/badge/{label}-{value}-{color})
```

Allowed colors: `brightgreen` `green` `yellowgreen` `yellow` `orange` `red` `blue` `lightgrey`

### Badge Placement Rules

- Place badges after the description, one line
- 4-6 badges maximum
- Always include if available: version + license + language/runtime
- If the user doesn't know their GitHub owner/repo, use static badges instead

## Section Structure (standard-readme spec)

### 1. Title
`# {name}` — match the manifest name exactly.

### 2. Badges
4-6 shields.io badges, single line.

### 3. Short Description
One line under 120 characters.

### 4. Hero Image / Screenshot (Optional)
Only if user provides one.

### 5. Features
3-7 bullet points. Format: `**{Feature}** — {Benefit}`.

### 6. Table of Contents
Skip if README < 100 lines.

### 7. Background (Optional)
Only if user wants to explain motivation.

### 8. Quick Start
Must go from zero to working result in under 60 seconds.

### 9. Usage Examples
2-4 examples. Each: heading → explanation → runnable code → expected output.

### 10. API Reference (Libraries only)
Table: Parameter, Type, Default, Description.

### 11. Configuration (Tools only)
Env vars, config format, CLI flags.

### 12. Contributing
Link to CONTRIBUTING.md or inline fork-PR workflow.

### 13. License
Must be last section.

```
## License

{license} © {author}. See [LICENSE](LICENSE) for details.
```

## Extended Sections (Add on User Request)

- **FAQ** — Common questions
- **Changelog** — Link to CHANGELOG.md
- **Roadmap** — Planned features checklist
- **Benchmarks** — Performance comparisons
- **Screenshots** — Image gallery
- **Troubleshooting** — Common errors

## Quality Checks (Run Silently Before Delivering)

- [ ] Title matches manifest name
- [ ] Short description < 120 characters
- [ ] All badges use correct URLs
- [ ] Quick Start is copy-pasteable
- [ ] Usage example is complete (import + call + output)
- [ ] No placeholder text (TODO, your-name, etc.)
- [ ] License section is last
- [ ] Code blocks have language identifiers
- [ ] Table of Contents matches sections (if present)
- [ ] Consistent heading style
- [ ] No dead links
- [ ] File named exactly `README.md` (uppercase)
- [ ] Existing README overwrite confirmed with user (if applicable)

## Interaction Checkpoints Summary

| Step | Tool | What to Ask |
|------|------|-------------|
| 1 | `question` | Confirm detected tech stack + badge repo info (batched) |
| 2 | `question` | Language preference (en/zh/both) |
| 3 | `question` | Project details (batched: description, features, install, interface) |
| 4 | `question` | Overwrite vs merge if README.md exists |
| 5 | `question` | Approve section plan |
| 6 | `question` | Feedback on draft |
| 7 | `question` | Confirm quality check and write |

**Never skip Step 2, 5, or 7. These are mandatory.** Steps 1/3/4 can be skipped when the user already provided the information in their request (Fast Path).
