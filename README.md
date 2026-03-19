# skill-project-butcher

> OpenClaw Skill：GitHub 项目深度研究专家

帮你快速理解任意代码库的核心架构、技术选型和实现细节。支持有 GitHub URL 时的 API 快速分析，也支持已 clone 项目的本地深度研究。

## 功能特性

- 🔍 **GitHub API 快速概览**：不 clone 也能拿到项目基本信息、语言占比、目录结构
- 📊 **本地深度分析**：已 clone 项目可做完整的技术栈、模块依赖、架构解读
- 🏗️ **Monorepo 自动检测**：识别 pnpm workspaces、npm workspaces、lerna、rush、nx
- 📦 **多语言支持**：Python、Go、Rust、TypeScript/JavaScript、Java 等
- 🛡️ **防卡死设计**：自动排除 node_modules/vendor/target 等大型目录
- 📝 **标准化报告**：一键输出结构化分析报告

## 适用场景

- 技术调研：了解某个开源项目的架构和实现
- 代码学习：深入理解优质项目的设计思路
- 选型评估：对比多个项目的技术栈和活跃度
- 快速概览：在不 clone 的情况下快速了解一个项目

## 安装

### 方式一：作为 OpenClaw Skill 安装

将 `SKILL.md` 复制到 OpenClaw skills 目录：

```
~/.openclaw/workspace/skills/skill-project-butcher/
└── SKILL.md
```

重启 OpenClaw 后自动加载。

### 方式二：独立使用

本 skill 的命令也可独立使用（需要 curl、git、python3）：

```bash
# 获取项目概览
curl -s "https://api.github.com/repos/<owner>/<repo>" | python3 -c "
import json,sys
d=json.load(sys.stdin)
print('名称:', d.get('name'))
print('描述:', d.get('description'))
print('语言:', d.get('language'))
print('Stars:', d.get('stargazers_count'))
"

# 获取语言占比
curl -s "https://api.github.com/repos/<owner>/<repo>/languages" | python3 -c "
import json,sys
d=json.load(sys.stdin)
total=sum(d.values())
for k,v in sorted(d.items(), key=lambda x:-x[1]):
    print(f'{k}: {v/total*100:.1f}%')
"
```

## 使用方式

### 路径 A：只有 GitHub URL（未 clone）

```
分析这个项目：https://github.com/cli/cli
帮我看看这个代码库：https://github.com/vercel/next.js
研究一下这个 repo
```

Skill 会先用 GitHub API 拿概览，再决定是否需要深度 clone。

### 路径 B：本地已有代码（已 clone）

```
帮我理解一下这个项目（路径：~/code/myproject）
分析这个 clone 了的项目
```

## 研究流程

### 路径 A：GitHub API 概览（~10 秒）

```
Step 1: 项目基本信息（curl GitHub API）
Step 2: 语言占比分析
Step 3: 顶级目录结构
Step 4: README 摘要
Step 5: 决策（快速概览 or 建议 clone 深度分析）
```

### 路径 B：本地深度分析（需要 clone）

```
Step 1: 项目概览（README + git 信息）
Step 2: 目录结构（防卡死排除）
Step 3: 项目类型识别（依赖管理文件）
Step 4: Monorepo 检测
Step 5: 技术栈指纹（依赖分析）
Step 6: 核心模块定位
Step 7: README 深度提取
Step 8: 构建配置分析
Step 9: 测试文件定位
```

## 输出示例

### 快速概览

```markdown
## cli/cli 概览

**描述**：GitHub's official command line tool
**语言**：Go 99.2%, Shell 0.7%
**Stars**：43,204
**活跃度**：2026-03-17
**主题**：cli, git, github-api-v4, golang
**架构**：单体（Go 项目）
**技术栈**：Go + GitHub API v4

## 值得注意
- GitHub 官方 CLI 工具，权威实现
- Go 语言主导，代码质量高
- 43k Stars，活跃度高
```

### 深度分析

```markdown
# GitHub CLI 深度分析报告

## 基本信息
- **类型**：单体 Go CLI 项目
- **语言**：Go 99.2%, Shell 0.7%
- **Stars**：43,204
- **技术栈**：Go + GitHub API v4 + cobra（CLI框架）
- **最近更新**：2026-03-17

## 架构解读

### 核心目录结构
├── cmd/        # 入口命令定义
├── internal/   # 内部包
├── pkg/        # 公共库
├── script/     # 构建脚本
└── docs/       # 文档

### 主要模块
- `pkg/cmd`：命令实现
- `pkg/api`：GitHub API 封装
- `pkg/iostreams`：输入输出流

## 技术亮点
1. 基于 cobra 构建 CLI，体验专业
2. GraphQL API 封装优雅
3. 测试覆盖率高的典型

## 快速上手
```bash
gh auth login
gh repo view
```
```

## 项目结构

```
skill-project-butcher/
├── SKILL.md    # OpenClaw Skill 指令
└── README.md   # 本文档
```

## 依赖工具

- `curl` — GitHub API 请求
- `git` — 版本控制
- `find` / `grep` / `xargs` / `wc` — 文件搜索和统计
- `python3` — JSON 解析（可选，jq 也可）

## License

MIT
