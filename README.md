# Self-improving for Codex

让 Codex 通过 `AGENTS.md` 与本地 Markdown 记忆文件形成可审计、可维护的长期改进闭环。

> A Codex-native, file-based memory loop. No background service, telemetry, or OpenClaw-only runtime required.

## 为什么需要它

Codex 不会自动把任意文件当作长期记忆。这个 Skill 使用 Codex 原生的 `AGENTS.md` 作为稳定入口，把长期偏好、可复用经验、环境报错和能力缺口分别写入职责清晰的文件，避免把所有内容堆进一个不断膨胀的提示词。

```text
AGENTS.md 启动规则
        │
        ├── 读取 PROFILE.md + ACTIVE.md
        │
        ▼
     执行日常任务
        │
        ├── LEARNINGS.md
        ├── ERRORS.md
        └── FEATURE_REQUESTS.md
                │
                ▼
         定期精炼、去重与晋升
                │
                ├── ACTIVE.md
                └── PROFILE.md
```

## 核心能力

- 审计已有的全局 `AGENTS.md`、记忆目录和相关自动化，优先保留现有配置。
- 建立并维护五类职责分离的记忆文件。
- 生成适合 Codex 的 `AGENTS.md` 集成片段。
- 定义从原始记录晋升到 `ACTIVE.md` / `PROFILE.md` 的保守规则。
- 可选设计夜间精炼任务，用于去重、合并和清理过时内容。
- 明确区分 Codex 原生能力与 OpenClaw 的 `SOUL`、`HEARTBEAT.md` 等专用机制。

## 安装

将仓库克隆到 Codex 的全局 Skill 目录：

```powershell
$skillsRoot = Join-Path $env:USERPROFILE '.codex\skills'
New-Item -ItemType Directory -Force -Path $skillsRoot | Out-Null
git clone https://github.com/GODGOD126/self-improving-for-codex.git `
  (Join-Path $skillsRoot 'self-improving-for-codex')
```

重新打开 Codex，确认技能列表中出现 `self-improving-for-codex`。

## 使用

在 Codex 中直接说明要使用这个 Skill，例如：

```text
Use $self-improving-for-codex to audit my current global memory setup and propose the safest Codex-native configuration.
```

也可以用中文：

```text
使用 self-improving-for-codex 检查我现有的 AGENTS.md 和 memories，保留已有内容并补齐缺失的记忆闭环。
```

建议先让 Codex 审计并给出变更清单，再授权修改本地文件。默认情况下，Skill 只应提出 `AGENTS.md` 的准确片段；除非用户明确授权，不应自动修改 `AGENTS.md`。

## 记忆文件分工

| 文件 | 用途 | 不应该存放 |
| --- | --- | --- |
| `PROFILE.md` | 长期稳定的用户画像、沟通和工作偏好 | 临时任务、一次性要求 |
| `ACTIVE.md` | 每次任务开始前都值得读取的高优先级规则 | 冗长过程、未经确认的推断 |
| `LEARNINGS.md` | 可复用经验、用户纠正、知识更新 | 显而易见的小失误和噪音 |
| `ERRORS.md` | 环境故障、工具异常和可复用排障方法 | 没有复用价值的偶发输出 |
| `FEATURE_REQUESTS.md` | 当前尚未闭环、值得长期跟踪的能力需求 | 已完成的普通待办 |

默认推荐的全局位置是：

```text
~/.codex/AGENTS.md
~/.codex/memories/
```

实际路径应根据操作系统和 Codex 配置调整。

## 仓库结构

```text
.
├── SKILL.md                    # Skill 主流程、边界和交付标准
├── agents/openai.yaml          # Codex 展示名称与默认提示词
└── references/
    ├── agents-snippet.md       # AGENTS.md 推荐集成片段
    ├── memory-files.md         # 五类记忆文件模板与职责
    └── nightly-review.md       # 可选夜间精炼规则和提示词
```

## 安全与隐私

- 这个仓库本身只包含 Markdown 指令和 Skill 元数据，没有后台服务或遥测代码。
- 记忆文件可能包含个人偏好、环境路径或项目经验，不要把真实的 `~/.codex/memories` 直接提交到公开仓库。
- 不应把密码、令牌、私钥或完整 `.env` 内容写入记忆文件。
- 夜间精炼只维护已有记忆，不应默认回放全部历史对话。
- 自动化不得自行修改 `AGENTS.md`；顶层规则应由用户保持最终控制。

## 贡献

欢迎提交 Issue 或范围清晰的 PR。请说明使用场景、预期行为和安全边界；如果修改了记忆分类或晋升规则，请同步更新相关 reference 文件。

## License

[MIT License](LICENSE) © 2026 GODGOD126
