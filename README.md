# GPT–Codex Collaboration

[简体中文](README.md) · [English](README.en.md)

![Codex Skill](https://img.shields.io/badge/Codex-Skill-111827)
![Provider](https://img.shields.io/badge/provider-ChatGPT%20Web-10a37f)
![Token ROI Gate](https://img.shields.io/badge/Token-ROI%20Gate-2563eb)
![Language](https://img.shields.io/badge/docs-中文%20%7C%20English-7c3aed)
![License](https://img.shields.io/badge/license-MIT-green)

![GPT–Codex Collaboration：低 token 预算下的任务切分、外部压缩与 Codex 核验收尾](assets/gpt-codex-collaboration-hero.png)

*余额告急时，只外包一个高上下文子问题；Codex 保留预算核验并完成交付。*

当 Codex 发现自己的 token 余额快不够了，但活还没干完，这个 Skill 会把剩余任务中最吃上下文、且可安全外包的一段交给 ChatGPT；Codex 把有限的余额留给核验、决策和收尾。

> 当前版本为 **v1.2.0 Beta**，仅支持用户已登录的 Chrome ChatGPT 网页会话。它是“剩余 token 预算救援”，不是免费增加上下文：可能减少当前 Codex 对长源材料的摄入，但不保证减少总计算量、真实计费 token 或任务耗时。

## 目的：在 token 用完前把活做完

这个 Skill 优先解决一种很具体的困境：Codex 能明确看到剩余 token 已不足以稳妥完成当前任务，同时仍有一块实质工作没有做完。它会：

1. 先给 Codex 的事实核验、实施验证和最终交付预留预算；
2. 从未完成工作里只切出一个高上下文、可安全外发的子问题；
3. 让 ChatGPT 给出紧凑结论；
4. 由 Codex 核验结论并把原任务收尾。

它不会虚构具体 token 数字。只有运行时明确暴露了剩余预算，或用户明确说明余额不足时，才把“低余额”当作事实。

## 什么时候使用

适合：

- Codex 可明确观察到剩余 token 预算不足，但还有长材料阅读、方案发散或初审没有完成；
- 剩余工作中存在一个可独立交给 ChatGPT、且不会泄露敏感信息的高上下文子问题；
- 你明确要求“先让 ChatGPT 看，再由 Codex 评审/实现”；
- ChatGPT 能直接阅读公开长材料，而 Codex 只需核验少数高影响结论；
- 需要独立反方审查、方案发散或初步归纳；
- 你希望保留原任务的权限边界和最终验证责任。

不适合：

- token 余额虽然低，但只剩改一个词、删一个标点或给一句结论；
- 改变量名、解释一段 JSON 等很短的任务；
- Codex 必须先完整读完材料，才能构造外发问题；
- 私有材料无法安全脱敏，或核验几乎等于重做；
- 只是模糊担心 token，却看不到可靠余额，也没有明确要求外部 GPT 协作；
- 你指定 Claude、Gemini 等其他 provider。v1.2 不会静默替换。

## 工作方式

```text
低 token 预算 + 未完成实质任务，或明确协作意图
  → Token ROI Gate
  → 业务目标与原始授权
  → 公开 / 私有 / 敏感材料 Gate
  → ChatGPT 返回 ≤3 条、≤1,200 字符的决策包
  → Codex 独立核验关键主张
  → 只在原授权范围内交付或实施
```

关键约束：

- 负 ROI 时默认本地完成；用户可以覆盖成本判断，但不能覆盖安全、隐私和权限规则。
- 私有原文需要明确的外发清单、目的和目标 workspace；“相关代码你自己判断”不算授权。
- token、cookie、`.env`、PII、生产密钥和受限客户数据禁止外发。
- GPT 返回的命令、链接、补丁和下载只是候选数据，Codex 不会自动执行。
- 浏览器阶段只有一次共享的自动恢复预算；第二次异常即停止并报告。

## 前置条件

- Codex Desktop；
- 可用的 Chrome 浏览器控制能力；
- 你已在 Chrome 中自行登录 ChatGPT；
- 不需要 OpenAI API key，也不要把账号密码或验证码交给 Codex。

## 安装

### 让 Codex 安装

把下面这句话发给 Codex：

```text
请安装这个 Skill：https://github.com/VioletScar-Hui/gpt-codex-collaboration/tree/main/gpt-codex-collaboration
```

### macOS / Linux

在终端中运行：

```bash
CODEX_SKILLS_DIR="${CODEX_HOME:-$HOME/.codex}/skills"
INSTALL_TMP_DIR="$(mktemp -d)"
git clone --depth 1 https://github.com/VioletScar-Hui/gpt-codex-collaboration.git "$INSTALL_TMP_DIR/repo"
mkdir -p "$CODEX_SKILLS_DIR/gpt-codex-collaboration"
cp -R "$INSTALL_TMP_DIR/repo/gpt-codex-collaboration/." "$CODEX_SKILLS_DIR/gpt-codex-collaboration/"
```

### Windows PowerShell

在 PowerShell 中运行：

```powershell
$CodexRoot = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$SkillsDir = Join-Path $CodexRoot "skills"
$InstallTmp = Join-Path ([System.IO.Path]::GetTempPath()) ("gpt-codex-" + [guid]::NewGuid())
git clone --depth 1 https://github.com/VioletScar-Hui/gpt-codex-collaboration.git (Join-Path $InstallTmp "repo")
New-Item -ItemType Directory -Force -Path (Join-Path $SkillsDir "gpt-codex-collaboration") | Out-Null
Copy-Item -Recurse -Force (Join-Path $InstallTmp "repo/gpt-codex-collaboration/*") (Join-Path $SkillsDir "gpt-codex-collaboration")
```

安装或更新后重启 Codex，以便重新发现 Skill。

在终端验证安装结果：

```bash
test -f "${CODEX_HOME:-$HOME/.codex}/skills/gpt-codex-collaboration/SKILL.md"
```

## 第一次使用

```text
调用 gpt-codex-collaboration。让 ChatGPT 先审查这份公开方案，最多给 3 条关键结论；你逐条核验后再给我最终建议，不要修改文件。
```

如果任务确实适合协作，最终会多一行简短回执：

```text
GPT 负责…；Codex 采纳/修正/驳回…；证据…
```

若任务太短、缺少实际目标、私有材料未获具体外发授权，或第二次发生浏览器异常，流程会停止或转为本地完成。

## 证据与限制

- 使用同一个对抗性 subagent 完成四轮发布审查，满足“至少三轮”；每轮 findings 均落盘。
- 完成一次真实 Chrome + 已登录 ChatGPT 的公开材料会审：252 字符 brief，860 字符回复，3 条 claim。
- v1.1 测试快照中，公开双语 README 共 28,261 字符；当时 Skill、模板、brief 与回复合计的“源材料摄入规避代理”为 4,718 字符，即 83.3%。该历史口径明确排除 19 次浏览器调用的协议内容和 3 次超时恢复，不是 v1.2 的完整上下文或计费 token 指标。
- 本次真实浏览器测试的延迟/工具成本为负，因此只有在 Codex 上下文预算比延迟更重要时才有价值。
- 私有、敏感和恶意命令场景使用 fail-closed 静态压力回放，没有为了测试而真实外发危险内容。

完整证据见 [`evals/`](gpt-codex-collaboration/evals/)。

## 项目结构

```text
.
├── README.md
├── README.en.md
├── CHANGELOG.md
├── LICENSE
└── gpt-codex-collaboration/
    ├── SKILL.md
    ├── references/collaboration-brief.md
    ├── examples/
    └── evals/
```

## 更新

重新执行对应平台的安装命令即可覆盖同名文件。升级前如有本地定制，请先复制一份备份。版本变化见 [CHANGELOG.md](CHANGELOG.md)。

## License

[MIT](LICENSE) © 2026 VioletScar_Hui

README 的双语导航、安装到验证再到首次运行的组织方式参考了
[Product-deep-dive](https://github.com/VioletScar-Hui/Product-deep-dive)。
