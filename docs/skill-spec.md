# Bot Skills 开发规范

本仓库是所有 Grok Bot / Agent 共用的 skill 源码库。任何 Agent 新增或修改 skill 必须遵守本文，保证结构统一、可复用、可审查。

## 1. 仓库布局

```
bot-skills/
├── README.md
├── LICENSE
├── docs/
│   └── skill-spec.md          # 本规范（唯一权威）
└── skills/
    └── <skill-id>/
        ├── SKILL.md           # 必填：技能本体
        └── …                  # 可选：辅助文件（脚本、模板、示例）
```

- 一个 skill = `skills/<skill-id>/` 一个目录。
- `<skill-id>` 使用 **kebab-case**（小写字母、数字、连字符），由 `name` 派生，例如 `Tech hot-news digest` → `tech-hot-news-digest`。
- 禁止在仓库根目录散落多个 `SKILL.md`。
- 禁止把 Agent 专属配置（频道 ID、个人邮箱、某 bot 的 agent id）写进 skill。

## 2. SKILL.md 必填结构

```markdown
---
name: Human-readable Title
description: >-
  Use when …（一句说明何时启用；这是目录里唯一用于匹配的说明）
---

# Human-readable Title

（正文：可复用步骤，见第 3 节）
```

### Frontmatter

| 字段 | 必填 | 规则 |
|------|------|------|
| `name` | 是 | 给人看的标题；简洁、名词短语；勿塞版本号 |
| `description` | 是 | **何时使用**，以 “Use when …” / “当…时使用” 起笔；一行或短折叠块；勿写长步骤 |
| 其他 Cursor 键（`globs`、`alwaysApply` 等） | 可选 | 若已有则保留；不要擅自删除 |

### 正文建议章节（按需取舍，顺序建议固定）

1. **目的 / 何时用**（可与 description 呼应，勿重复堆砌）
2. **输入 / 调用方参数**（语言、范围、目的地、日程等由 routine 或聊天传入）
3. **步骤**（编号；写意图，不写死工具 API 参数）
4. **输出格式**
5. **失败与兜底**（已知 blocker、静默/必达规则）
6. **交付对象**（默认：本聊天的用户；其它目的地由调用方覆盖）

## 3. 内容原则

### 必须

- **通用**：任何 Agent 读完都能执行；不出现具体 bot 名、agent id、私有频道。
- **意图式步骤**：写“用公开 X 工具搜 AI 热点”，不写冻结的 MCP 工具名+参数 JSON（schema 会变）。
- **可配置默认值**：在正文写清 defaults，并写明 “调用方可覆盖”。
- **交付写清楚**：结果给谁、无新内容时是静默还是仍要回一句。
- **中英文均可**，但同一 skill 内术语与章节标题保持一致；面向 James 的日常产出默认可用中文说明。

### 禁止

- 写入密钥、token、cookie、个人隐私。
- 把 routine 的 cron、某个 bot 的汇报线写进 skill（那些属于 routine / memory）。
- 为凑字数复制大段无关政策或工具说明书。
- 同义 skill 重复创建：先搜仓库是否已有相近 `skill-id`，应扩展而非分叉。

## 4. 与 Routine / Memory 的边界

| 放进 Skill | 放进 Routine / Memory |
|------------|------------------------|
| 可复用步骤与格式 | 几点跑、谁触发 |
| 默认领域/条数/语言 | 发给哪个聊天、哪个 Slack |
| 已知平台兜底（如 Reddit 403） | 某个 bot 向谁汇报 |
| 质量门槛（去重、禁止注水） | 用户偏好的语气昵称 |

Routine 里用指针引用 skill（例如运行 `@Tech hot-news digest`），**不要**把整份 SKILL 正文复制进 routine。

## 5. 新增 / 变更流程

1. 读本规范与仓库现有 `skills/*`，确认无重复。
2. 本地或通过 Bot Boss 用 `UpdateSkill` 写入共享库（立刻对所有 Agent 生效）。
3. 将同内容提交到本仓库：
   - 路径：`skills/<skill-id>/SKILL.md`
   - 分支命名：`cursor/<skill-id>-…` 或 `skill/<skill-id>`
   - PR 标题：`Add|Update <skill-id> skill`
4. PR 描述需包含：用途一句话、破坏性变更（如有）、是否已同步到共享技能库。
5. 合并后：通知相关 Agent 改用仓库版本；删除过时的一次性草稿。

## 6. Code review 检查清单

- [ ] `skill-id` kebab-case，目录名与 id 一致
- [ ] frontmatter 含 `name` + `description`（description 写清 WHEN）
- [ ] 无 bot/agent id、无密钥、无私人频道
- [ ] 步骤为意图，无冻结工具 schema
- [ ] 写明 defaults、覆盖方式、交付与空结果行为
- [ ] README / 本规范无需改时不要无关改动
- [ ] 若修改已有 skill：说明兼容性

## 7. 示例（最小骨架）

见 `skills/tech-hot-news-digest/SKILL.md` 作为完整范例；最小骨架：

```markdown
---
name: Example skill
description: >-
  Use when doing X for the user in a reusable, multi-step way.
---

# Example skill

## Defaults
- …（可被调用方覆盖）

## Steps
1. …
2. …

## Output
- …

## Delivery
- Send the result to the user in this chat unless the caller names another destination.
```

## 8. 维护者

- 规范变更：改 `docs/skill-spec.md`，并视需要同步 `skills/skill-authoring-standards/SKILL.md`。
- 技能目录的日常监护可由 Bot Boss 协调；各领域 Agent 可提 PR，但结构必须过本清单。
