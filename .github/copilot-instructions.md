# GitHub Copilot 指南 — cloud-native-security

说明：我已扫描仓库（未发现 `README.md` 或现有的 AI 指南文件），因此下面内容以“发现导向”为主：先说明当前可观察到的仓库状态与明确的检查动作，再给出可直接执行的查询/命令与合并规则。请在仓库中补充实际源码或 CI 文件后让我再次运行本文件以自动补全项目特定细节。

## 目标
- 让 AI 编码代理在打开此仓库后 1–2 分钟内能定位关键组件、常用构建/测试命令与集成点。

## 初始检查（必须先做）
- 搜索关键文件：`README.md`、`Dockerfile`、`go.mod`、`package.json`、`pyproject.toml`、`Makefile`、`.github/workflows/`、`charts/`、`deploy/`、`cmd/`、`pkg/`、`internal/`、`tests/`。
- 若找到上述任一文件，打开并提取：构建命令、运行命令、依赖管理文件、CI 配置、部署清单。

示例指令（在仓库根目录）：
```
git status --porcelain
rg "^FROM |go mod|package.json|pyproject|Makefile|chart:" || true
ls -la .github/workflows || true
```

## 当仓库包含 Go 服务时（发现 `go.mod`）
- 构建：`go build ./...`。测试：`go test ./...`。
- 主要入口通常在 `cmd/` 下；库代码在 `pkg/` 或 `internal/`。
- 查找 `Dockerfile` 与 `Makefile` 以获取镜像标签、构建参数和生成步骤的细节。

## 当仓库包含 Node/JS 服务时（发现 `package.json`）
- 常见脚本：检查 `scripts` 字段以获取 `build`、`start`、`test` 命令。
- 若存在 `Dockerfile`，优先使用其中的运行/暴露端口配置。

## CI/CD 与部署线索
- 打开 `.github/workflows/*`：标注构建矩阵、触发分支、使用的 actions（例如 build/test/publish）。
- 若存在 `charts/` 或 `deploy/`：把 `values.yaml`、K8s manifests 和 Helm charts 作为部署契约参考。

## 代码风格与约定（可自动检测）
- 代码组织：优先识别 `cmd/`、`internal/`、`pkg/`、`api/`、`controllers/` 等目录。
- 配置约定：查找 `config/`、`*.yaml`、`env.example`、`.env`、`config.yaml`。若存在 `config/`，AI 应优先遵循其中的默认值。

## 集成点与外部依赖（要查证）
- 检查依赖文件（`go.mod`、`package.json`、`requirements.txt`）并提取关键外部服务（数据库、消息队列、外部 API）。
- 在代码中搜索常见 SDK 关键字：`kubernetes`、`redis`、`postgres`、`grpc`、`aws`、`gcp`、`azure`。

## 合并规则（如果已有 `.github/copilot-instructions.md`）
- 保留原文的项目特定命令和示例；在顶部添加一段“自动合并注记”并保留作者与时间戳。
- 仅替换或补充缺失的、可验证的信息；不要删除现有可执行命令或 CI 配置示例。

## 写入/编辑建议（对 AI 代理）
- 变更代码前：运行快速静态检查（如 `go vet`、`npm run lint`、`flake8`）并列出潜在问题。
- 小步提交：每次修改创建小的、描述明确的 commit（例如：`feat: add health endpoint`、`fix: handle nil config`）。
- 不要对未知外部系统做破坏性更改（删除 DB migration、移除 CI 触发等），除非 PR 说明了完整替代方案并有回退步骤。

## 本仓库当前状态说明（自动化扫描得出）
- 未检测到 `README.md` 或已有的 AI 指南文件；本文件为初始占位/指导模板。

——
如果你希望我基于实际源码自动填充“架构概述”“关键文件与命令示例”与“典型开发流程（本项目专属）”，请先把源码或 CI 文件推送到仓库，或允许我运行进一步的文件读取操作。我会把发现的具体示例（文件路径与行号）写入并再次提交一个合并后的版本。

请确认或指出缺失的具体文件/目录让我继续自动补全。谢谢！
