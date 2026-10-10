# 搜极星 SouGEO CLI Skills — 一句话生成 GEO 品牌报告 🛰️

> **让任意 AI Agent 一句话搞定品牌 GEO 洞察报告**：`sougeo-brand-report` 是面向 Agent 的 Skill，把「搜极星 SouGEO」平台的完整 GEO 报告能力（登录 → 引导式生成 → 轮询状态 → 取报告链接 → 分享 → 套餐管理）封装成一条对话闭环。**无门槛上手、无配置文件、无编程要求**，覆盖国内外 **11 个主流 AI 平台**。

[English](./README_EN.md) | 中文

---

## 📖 这是什么

**GEO（Generative Engine Optimization，生成引擎优化）** 是 SEO 的 AI 时代继任者：衡量品牌在 AI 大模型（豆包、DeepSeek、ChatGPT、Claude 等）生成回答中的**可见度、引用率、排名与口碑**。
<div style="position: relative; width: 100%; height: 300px; overflow: hidden; border-radius: 10px;">
  <img src="https://image.yoojober.com/upload-m/2026-10/6aca16c0b944f.png" style="position: absolute; width: 100%; height: 100%; object-fit: cover; opacity: 1; animation: fade 9s infinite; animation-delay: 0s;">
  <img src="https://image.yoojober.com/upload-m/2026-10/6aca16d2605b2.png" style="position: absolute; width: 100%; height: 100%; object-fit: cover; opacity: 0; animation: fade 9s infinite; animation-delay: 3s;">
  <img src="https://image.yoojober.com/upload-m/2026-10/6aca18d8b263b.png" style="position: absolute; width: 100%; height: 100%; object-fit: cover; opacity: 0; animation: fade 9s infinite; animation-delay: 6s;">
  <img src="https://image.yoojober.com/upload-m/2026-10/6aca18e3555b0.png" style="position: absolute; width: 100%; height: 100%; object-fit: cover; opacity: 0; animation: fade 9s infinite; animation-delay: 6s;">
</div>

<style>
@keyframes fade {
  0% { opacity: 0; }
  10% { opacity: 1; }
  33% { opacity: 1; }
  43% { opacity: 0; }
  100% { opacity: 0; }
}
</style>
当越来越多用户开始"有事问 AI"，品牌在 AI 回答里是否被提及、被推荐，直接决定生意流向。**SouGEO CLI**（`npm i -g @yooai/sougeo-cli`）是搜极星 GEO 平台的命令行客户端，本 Skill 让 AI Agent 通过自然语言对话驱动它，替用户完成全流程。

## 🎯 核心解决的问题

| 痛点 | SouGEO 的答案 |
|---|---|
| **品牌在 AI 里的表现是黑盒** —— 用户问 AI"推荐哪款手机"，AI 提没提你、怎么评价你，无处可知 | 生成 GEO 洞察报告，量化 **AI 可见度 / 品牌指数 / 引用率 / 排名 / 口碑**，并给出在线报告链接 |
| **测不准** —— 测试问题里带品牌名，等于把答案喂给 AI | Skill 强制生成**不含被测品牌名的「用户视角 + GEO 场景」测试问题**（含决策引导词、植入卖点关键词），由用户点选，保证测试真实有效 |
| **传统监测门槛高** —— 要懂参数、会写脚本、搭环境 | **零门槛**：一句话（"帮我测测小米在 AI 里怎么样"）即可触发；Agent 引导式收集参数，缺什么问什么，其余全用推荐默认值 |
| **国内海外平台割裂** —— 各家 AI 分散、逐一人工提问成本高 | 一份报告同时覆盖 **11 个平台**（国内 7 + 海外 4），一次生成、横向对比 |
| **报告交付难** —— 生成后还要截图、导出、转发 | 报告自动生成**在线链接**，一键拼装 `share_code` 分享链接，支持官网查看查看者与访问记录 |
| **成本顾虑** —— 不敢轻易尝鲜 | **极速版品牌报告 / 免费版行业报告无需任何权益校验**，安装即用、免费可跑 |

## ✨ 特性

- 🗣️ **零门槛对话式生成**：自动识别品牌/行业报告需求，逐步引导收集参数（企业/个人品牌判定、基于品牌名推断的产品类型/职业类型候选、地域按 IP 归属地解析），全程可点选、有默认值
- 🌏 **11 个 AI 平台覆盖**：默认勾选国内 7 大平台，可追加海外 4 大平台
- ⚡ **完整闭环**：`create → status --watch（轮询至完成）→ read --type link（取链接 + 基础小结）→ share（二次分享）`
- 📊 **三类报告**：品牌报告（企业/个人）、行业报告、星盾报告；支持分页查询历史报告
- 💰 **权益与套餐管理**：余额查询、套餐列表、下单购买、订单校验
- 🔐 **双登录方式**：设备码登录 / Token 登录，`auth status -o json` 结构化登录态
- 🖥️ **跨平台开箱即用**：Windows / Linux / macOS（x64 / arm64），**0 npm 依赖**，安装时自动下载对应原生二进制

## 🌍 覆盖的 11 个 AI 平台

| ID | 平台 | 地区 | 默认 |
|----|------|------|------|
| `1` | 豆包 | 国内 | ✅ |
| `3` | 腾讯元宝 | 国内 | ✅ |
| `4` | DeepSeek | 国内 | ✅ |
| `5` | Kimi | 国内 | ✅ |
| `6` | 智谱清言 | 国内 | ✅ |
| `7` | 千问 | 国内 | ✅ |
| `8` | 文心一言 | 国内 | ✅ |
| `9` | ChatGPT | 海外 | ☐ 可选 |
| `10` | Claude | 海外 | ☐ 可选 |
| `11` | Gemini | 海外 | ☐ 可选 |
| `12` | Grok | 海外 | ☐ 可选 |

## 📦 安装

> ### ⚠️ 完整安装分两步：先装 Skill，再装 CLI
> Skill 本身只是给 Agent 看的「使用说明书」——**真正执行报告生成的是 SouGEO CLI**，只装 Skill 是跑不了报告的。流程：① 通过下方任一方式安装 Skill → ② **根据 Skill 的指引安装 CLI**（SKILL.md 内置了完整的 CLI 安装与校验步骤，Agent 首次运行时会自动检测并引导安装；也可参照下文提前手动装好）。

**方式一：`skills` CLI 安装（推荐）**

```bash
npx -y skills add https://github.com/YOOTeam/SouGEO-CLI
```

安装后查看已安装 skills：

```bash
npx -y skills list
```

**方式二：让 AI Agent 自动安装（零操作）**

直接对任意已接入 Skill 机制的 AI Agent（Trae、WorkBuddy、Claude Code、Cursor 等）说一句话，Agent 会自动拉取并安装到自己的技能目录。仓库二选一：GitHub（海外）或 Gitee（国内，访问更快）：

```
帮我安装搜极星 SouGEO 的技能。技能文件在仓库下的 skills/sougeo-brand-report/ 目录（含 SKILL.md）：
- GitHub：https://github.com/YOOTeam/SouGEO-CLI
- Gitee（国内更快）：https://gitee.com/AiDoDesign/sougeo_cli
请从其中任一仓库下载该目录并放入你的技能目录，
然后验证 sougeo CLI 是否可用，不可用就用 npm 帮我装好（npm i -g @yooai/sougeo-cli）。
```

适合不想碰命令行的用户——装完直接说"帮我做个 GEO 报告"就能用。

**方式三：手动安装**

从仓库下载 [`skills/sougeo-brand-report/`](https://github.com/YOOTeam/SouGEO-CLI/skills/sougeo-brand-report) 目录（Gitee 仓库同样路径），复制到你的 Agent 技能目录即可（如 `~/.qwenworkcn/skills/`、`~/.workbuddy/skills/`、`~/.claude/skills/` 等）。

> 📁 仓库结构：技能本体位于仓库的 **`skills/`** 子目录（`skills/sougeo-brand-report/SKILL.md`），而非仓库根目录；手动安装或让 Agent 下载时请认准该路径。

### Skill 安装完成后：安装 SouGEO CLI（必做，Skill 会引导）

Skill 装好后还**不能直接出报告**——必须安装它所驱动的 SouGEO CLI。Skill 内部已内置完整的 CLI 安装、校验与故障排查指引：**你只需对 Agent 说"帮我做个 GEO 报告"，Agent 会自动检测 CLI，未安装时按 Skill 指引用 npm 装好**。也可以提前手动安装：

```bash
npm i -g @yooai/sougeo-cli      # 要求 Node >= 16，安装后命令名为 sougeo
sougeo version show             # 能打印版本即安装成功
```

> 💡 国内网络提示：若 npm 默认镜像源（淘宝源 npmmirror）报 `404 not in this registry`（新包同步有延迟），改用官方源安装：
> `npm i -g @yooai/sougeo-cli --registry=https://registry.npmjs.org`

## 🚀 快速开始

对 Agent 说一句话即可：

```
帮我分析一下小米在 AI 里的表现
```

Agent 会自动走完整个闭环：

```
用户："帮我分析一下小米"
  ↓ 检测 CLI 可用性（未安装则 npm i -g @yooai/sougeo-cli）
  ↓ 识别：品牌报告 → 询问企业/个人品牌
  ↓ 据品牌名生成产品类型候选（智能手机/智能家居/SU7 汽车…）
  ↓ 生成一批用户视角 GEO 测试问题 → 用户点选（品牌 ≤3 条，不含"小米"）
  ↓ 地域解析（模糊词按 IP 归属地兜底）→ 参数确认单 → 用户确认
  ↓ sougeo report create → sougeo report status <id> --watch → sougeo report read <id> --type link
  ↓ 交付：品牌指数等基础小结 + 🔗 在线报告链接
```

也可以直接命令行操作：

```bash
sougeo auth login                                        # 首次登录
sougeo report create -r brand -b 小米 -a company -p 智能手机 \
  -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
  -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
  -l 1 -P 1,3,4,5,6,7,8                                  # 极速版 + 国内 7 平台，免费无需权益
sougeo report status <report_id> --watch                 # 轮询至完成
sougeo report read <report_id> --type link               # 取在线报告链接
```

## 📋 报告类型

| 类别 | ID | 名称 | 权益校验 |
|------|----|------|----------|
| 品牌报告 | `1` | 极速版 | ❌ 免费，无需校验 |
| 品牌报告 | `2` | 专业版 | ✅ 需权益 |
| 品牌报告 | `3` | 动态分析版 | ✅ 需权益 |
| 行业报告 | `31` | 免费版 | ❌ 免费，无需校验 |
| 行业报告 | `32` | 专业版 | ✅ 需权益 |

<div style="position: relative; width: 100%; height: 2500px; overflow: hidden; border-radius: 10px;">
  <img src="https://image.yoojober.com/upload-m/2026-10/6aca19b233e90.jpg" style="position: absolute; width: 100%; height: 100%; object-fit: cover; opacity: 1; animation: fade 9s infinite; animation-delay: 0s;">
  <img src="https://image.yoojober.com/upload-m/2026-10/6aca1b432cf5c.png" style="position: absolute; width: 100%; height: 250%; object-fit: cover; opacity: 0; animation: fade 9s infinite; animation-delay: 3s;">
</div>

<style>
@keyframes fade {
  0% { opacity: 0; }
  10% { opacity: 1; }
  33% { opacity: 1; }
  43% { opacity: 0; }
  100% { opacity: 0; }
}
</style>
## 💡 使用建议

- 对 Agent 说"帮我做个 GEO 报告 / 测测 XX 品牌 / 行业分析"即可触发，**不要自己拼参数**——Skill 内置引导式收集流程。
- 关注问题是 GEO 测试的关键：必须从用户视角写、含决策引导词、**不得含被测品牌名**（品牌 ≤3 条 / 行业 ≤6 条）。
- 报告生成为异步任务，使用 `report status --watch` 等待完成后再取链接。
- 分享链接（`https://sougeo.com/ReportResults?share_code=<code>`）打开后仍需登录 SouGEO 账号查看；分享后可在官网查看访问记录。
- 如果 Skill 内容与当前 CLI 输出不一致，以 CLI 输出为准。

## 🔗 相关链接

- npm 包：https://www.npmjs.com/package/@yooai/sougeo-cli
- 官网 / 报告查看：https://sougeo.com
- License：GPL-3
