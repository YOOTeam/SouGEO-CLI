---
name: sougeo-brand-report
description: "搜极星 SouGEO CLI — 面向 Agent 的商业品牌 GEO 洞察工具。**需先安装 CLI（npm i -g @yooai/sougeo-cli，命令名 sougeo）**。支持 Token/设备码登录（auth status 可输出 JSON 登录态）、引导式生成品牌/行业报告（自动识别报告需求并逐步收集参数：企业/个人品牌判定、品牌名、**基于品牌名推断生成的「产品类型 / 职业类型」候选（禁止通用候选池）**、行业名识别、**必填的关注问题（默认生成一批适 GEO 场景的用户视角问题、由用户**点选**；**品牌 ≤3 条 / 行业 ≤6 条**；禁含品牌名 + 含决策引导词 + 植入卖点关键词 + 四类标签）**、AI 平台、报告类型），创建后**轮询生成状态直至完成、再取出报告在线链接**并给出基础小结（create → report status --watch → report read --type link 闭环，默认不拉完整 JSON）；分页查询报告列表（brand/industry/shield）、报告分享（**分享后拼装 `https://sougeo.com/ReportResults?share_code=<code>` 分享链接供二次分享；⚠️ 打开后仍需登录 SouGEO 账号才能查看，非免登录公开页；分享后可在官网查看有哪些人查看过该报告及查看记录**）、可购买套餐查询（rights sku list）与下单购买、订单校验。跨平台（Windows/Linux/macOS）。触发词：安装sougeo、sougeo-cli、@yooai/sougeo-cli、npm安装、搜极星、SouGEO、sougeo、GEO报告、品牌报告、行业报告、星盾报告、个人品牌报告、企业品牌、产品类型、职业类型、关注问题、测试问题、问题生成、问题点选、GEO问题、地域解析、模糊地址、IP归属地、报告状态、生成状态、AI可见度、品牌指数、竞品对比、AI平台曝光、报告分享、分享链接、share_code、二次分享、报告链接、套餐、sku、geo report、brand report、report status。"
description_zh: "搜极星 GEO 品牌/行业/星盾报告 CLI（需先 `npm i -g @yooai/sougeo-cli`）：登录、引导式生成（产品/职业类型基于品牌名生成；**关注问题由 Agent 默认生成一批「用户视角 + GEO 场景」问题供用户点选，品牌 ≤3 条 / 行业 ≤6 条**；模糊地域按 IP 归属地解析或用户确认）、创建后轮询状态并取出报告链接与基础小结（品牌报告不含「行业归属」）、分页查询、分享报告并拼装 share_code 分享链接（打开需登录，分享后可在官网查看查看者与查看记录）、权益/套餐管理。"
description_en: "SouGEO GEO brand/industry/shield report CLI (install first: npm i -g @yooai/sougeo-cli): auth, guided create (brand-derived product/role options; **Agent pre-generates user-perspective GEO test questions for the user to pick, brand ≤3 / industry ≤6**; vague locations resolved via IP geolocation or user confirmation), poll status then fetch report link + brief summary (brand reports exclude industry attribution), paged list, share (build https://sougeo.com/ReportResults?share_code=<code>; requires login to view; on-site viewer & access records after sharing), rights/SKU. Cross-platform."
version: 3.12.0
display_name: "搜极星 GEO 品牌报告"
display_name_en: "sougeo-brand-report"
visibility: "public"
allowed-tools: Read, Write, Bash, Glob, WebSearch, WebFetch
agent_created: true
metadata:
  clawdbot:
    emoji: "🛰️"
---

# 搜极星 SouGEO CLI — GEO 品牌报告服务

> 本 Skill 让任意 Agent 通过命令行调用「搜极星」GEO 平台的完整报告能力：**登录 → 引导式生成报告 → 轮询状态至完成 → 取出报告链接并给基础小结 → 分享 → 套餐与权益管理**。

## 一、这是什么

**SouGEO CLI**（命令名 `sougeo`）是搜极星 GEO（Generative Engine Optimization，生成引擎优化）平台的命令行客户端，定位"面向于 Agent 的商业品牌洞察工具"。

- **GEO** 指品牌在 AI 大模型（豆包、DeepSeek、千问、文心一言、Kimi、ChatGPT、Claude、Gemini 等）生成回答中的**可见度、引用率、排名与口碑**优化，区别于传统 SEO。
- 本 CLI 把上述 GEO 指标体系封装为一条命令：既能在多个 AI 平台上**生成**品牌/行业分析报告，也能把报告**生成在线预览链接**（默认交付形态）或**读取为结构化 JSON**（按需）。

**核心能力：**
1. **认证管理** — 设备码登录、Token 登录、状态查询（`-o json` 结构化登录态）、退出登录
2. **报告管理** — **引导式生成**（自动识别品牌/行业需求并逐步收集参数；**关注问题 Agent 默认生成一批、由用户点选，品牌 ≤3 条 / 行业 ≤6 条，且不得含品牌名**）、**生成状态查询**（`report status`，支持 `--watch` 轮询至终态）、**完成后取报告链接 + 基础小结**、分页查询列表（brand/industry/shield）、分享管理（**拼装 `https://sougeo.com/ReportResults?share_code=<code>` 分享链接供二次分享；⚠️ 打开后仍需登录 SouGEO 账号；分享后可在官网查看查看者及查看记录**）
3. **权益管理** — 权益余额查询、可购买套餐列表（`rights sku list`）、下单购买、订单校验
4. **版本管理** — 查看版本（check/update 标注"暂未开放"）

### ⭐ 一句话上手：用户说"帮我做个 GEO 报告"时怎么做

> **不要**直接拼参数调用。**先确保 CLI 可用**（未安装则 `npm i -g @yooai/sougeo-cli`，见 §2.0），再按 §4.2「引导式参数收集流程」走：**识别需求 → 缺什么问什么 → 确认单 → 调用 → 轮询状态 → 完成后取链接（+品牌报告小结）**。

```
用户："帮我分析一下小米"
  ↓ 前置：CLI 是否可用？（command -v sougeo || command -v SouGEO）
          未安装 → npm i -g @yooai/sougeo-cli（见 §2.0）
  ↓ 识别：品牌报告（提到具体品牌）
  ↓ 缺什么问什么：品牌类型（企业/个人）？
  ↓ 产品类型：**先拿到"小米"，再据其产品线生成候选**（智能手机/智能家居/SU7 汽车…），禁止通用池
  ↓ ★关注问题：**Agent 默认生成一批「用户视角 + GEO 场景」问题**（场景/抉择/痛点/地域）
              → 摆给用户**点选序号**；品牌 ≤3 条、行业 ≤6 条
              默认问题【不含"小米"】← GEO 测试的关键
  ↓ 地域：说了"我们这边/本地"→ 按 **IP 归属地**解析为明确地名；解析不出就**问用户**
  ↓ 默认值：AI 平台 7 个国内 ⭐、报告类型 极速版 ⭐
  ↓ 权益校验（非极速版才需要）
  ↓ 展示「参数确认单」→ 用户确认
  ↓ 调用 report create（-q 必填）→ 取回 report_id
  ↓ report status <id> --watch --timeout 600     ← 轮询至 completed / opened
  ↓ report read <id> --type link                 ← 状态就绪后取出在线报告链接
  ↓ 交付：基础小结（品牌指数等）+ 🔗 链接，引导用户点开看完整报告
          （默认不拉 report read --type json）
```

| 用户需求 | 需要收集的关键参数 |
|---|---|
| 分析某**企业品牌** | 品牌类型=`company`、品牌名（或产品名）、**产品类型（须据品牌名生成候选）**、**关注问题（点选，≤3 条）** |
| 分析某**个人品牌** | 品牌类型=`personal`、个人品牌名、**职业类型（须据该人名/身份生成候选）**、**关注问题（点选，≤3 条）** |
| 分析某**行业** | 行业名（识别不到须引导选择/输入）、子行业（可选）、**关注问题（点选，≤6 条）** |
| 以上全部 | AI 平台（默认 **7 个国内平台**）、报告类型（默认极速版） |
| **产品/职业类型** | ⭐ **必须先拿到品牌名，再据该品牌真实产品线/职业生成 3~4 个强相关候选**，禁止通用候选池 |
| **关注问题（必填）** | ⭐ **Agent 默认生成一批「用户视角 + GEO 场景」的问题 → 用户点选序号**；**品牌 ≤3 条 / 行业 ≤6 条**；**不含被测品牌名**、**含决策引导词**、**植入卖点关键词** ★ |
| **查报告进度/是否生成完** | `report status <id>`（`--watch` 等待终态）★ |
| **拿到报告链接** | 状态 `completed`/`opened` 后 → `report read <id> --type link` ★ |
| 分享报告 | **有效期类型**（3 天 ⭐ / 7 天 / 永久）+ 是否允许查看历史 → **拼装 `https://sougeo.com/ReportResults?share_code=<share_code>` 分享链接**（⚠️ 打开后仍需登录 SouGEO 账号才能查看；分享后可在官网查看查看者与查看记录） |

> 完整状态机与话术见 **§4.2 引导式参数收集流程**（Step 0~7，含**关注问题规则**与**创建后轮询取链接闭环**）、**§4.2 `report status`**、**§4.2 `report share` 引导式分享**。

---

## 二、环境与准备

### 2.0 ⭐ 安装 CLI（npm 全局安装，首选）

**调用本 Skill 前，必须先确认 `sougeo` 可用；若未安装，用 npm 全局安装：**

```bash
npm i -g @yooai/sougeo-cli      # 安装后命令名仍为 sougeo
```

| 项 | 说明 |
|---|---|
| npm 包名 | **`@yooai/sougeo-cli`** |
| 包地址 | https://www.npmjs.com/package/@yooai/sougeo-cli |
| 安装命令 | `npm i -g @yooai/sougeo-cli` |
| 安装后可执行文件 | **`SouGEO`**（npm `bin` 字段：`{"SouGEO": "script/run.js"}`）→ 即命令行里用 **`sougeo`** 调用（大小写不敏感，见下） |
| 版本 | `latest` = **1.0.0** |
| 依赖 | **0 依赖** |
| Node 要求 | **`node >= 16`**（`engines` 字段） |
| 支持系统 | **`darwin` / `linux` / `win32`**；CPU：`x64` / `arm64` |
| 安装机制 | `postinstall` 执行 `script/run.js` **按当前平台自动下载对应原生二进制**（即 §2.1 的 8 平台产物） |
| License / 仓库 | GPL-3；https://github.com/YOOTeam/SouGEO-CLI |

**🔍 命令名与大小写（重要）**

- npm 包声明的是 **`SouGEO`** 这个 bin 名；在 **Windows 上大小写不敏感**，`sougeo` / `SouGEO` 均可调用。
- 在 **Linux / macOS 上文件名大小写敏感**：若 `sougeo`（全小写）提示 `command not found`，改用 **`SouGEO`**（首字母大写）。
- 探测：`command -v sougeo || command -v SouGEO`。

**✅ 安装校验：**
```bash
command -v sougeo || command -v SouGEO      # 有输出即已可用
sougeo version show                          # 或 SouGEO version show（能打印版本即正常）
```

**⚠️ 安装两个已知坑（务必避开）**

| 坑 | 现象 | 正确做法 |
|---|---|---|
| **`-g` 与包名之间漏空格** | `npm install -g@yooai/sougeo-cli` → 报 `npm warn Unknown cli config "--g@yooai/sougeo-cli"`，**实际什么都没装**（"up to date, audited 1 package" 是空跑），随后 `sougeo` 提示"不是内部或外部命令" | 必须写 **`npm install -g @yooai/sougeo-cli`**（`-g` 后有空格） |
| **镜像源未同步新包** | 淘宝源（npmmirror）报 `404 ... is not in this registry`（新发布的包同步有延迟） | 加官方源：**`npm install -g @yooai/sougeo-cli --registry=https://registry.npmjs.org`** |

> **二进制下载源**（`postinstall` 内部）：先试 `http://file.static.yoojober.cn/Sougeo/v<版本>/SouGEO_<平台>_<架构>[.exe]`（实测可用），失败才回落到 npmmirror（该镜像路径**实测 404**，不用管）。

**⬆️ 升级到最新版：**
```bash
npm i -g @yooai/sougeo-cli@latest --registry=https://registry.npmjs.org
```

> - 若全局安装后仍提示找不到命令，检查 npm 全局 bin 目录是否在 `PATH` 中（`npm bin -g` / `npm prefix -g`）。
> - `postinstall` 需要联网下载二进制；**内网 / 代理环境失败时**，可退回 §2.1 手动下载对应平台产物使用。
> - 安装/升级属**环境变更动作**，执行前应告知用户（见 §八）。

### 2.1 二进制（npm 安装后落地的原生产物，提供 8 个平台）

| 平台 | 文件 |
|------|------|
| Windows x64 | `SouGEO_windows_amd64.exe` |
| Windows x86 | `SouGEO_windows_386.exe` |
| macOS Intel / Apple Silicon | `SouGEO_darwin_amd64` / `SouGEO_darwin_arm64` |
| Linux 386 / x64 / ARM / ARM64 | `SouGEO_linux_386` / `_amd64` / `_arm` / `_arm64` |

> `npm i -g @yooai/sougeo-cli`（§2.0）会按当前平台**自动下载**上表中对应的一个产物；也可**手动**下载后直接调用。
>
> **路径解析优先级**：`npm 全局 bin（首选，即命令 `sougeo` / `SouGEO`）` → `$SOUGEO_CLI` → 上述本地路径按新到旧 → `glob` 查找 `SouGEO*` / `sougeo*`。**不要假定文件名**。

### 2.2 配置与后端环境（重要）

| 项 | 值 |
|----|----|
| 配置文件 | **`-c, --config`**，默认 `$HOME/.SouGEO/config.yaml` |
| 配置加载策略 | 先加载**内嵌配置**，再用用户配置**合并覆盖**（`-d` 下可见日志 `[配置] 已合并用户配置覆盖: ...`）——不存在用户配置时直接用内嵌配置 |
| 凭证存储 | 本地凭证文件 `$HOME/.SouGEO/credentials.json`（仅当前用户可读写；**明文 JSON**，`auth logout` 会删除该文件） |
| 后端环境 | **由配置决定**：内嵌配置指向测试环境 `test.geo.api.copyai.cn`；若 `$HOME/.SouGEO/config.yaml` 存在且指向生产环境（如 `geo.api.copyai.cn`），则所有命令打到**生产**。Agent 判断环境可看 `version show -d` 的合并日志，或配置文件中的 `api.url` |
| 环境变量 | 支持 **`.env` 文件**（读取**当前工作目录**下的 `.env`，不覆盖已注入的 shell 变量）与 **`SouGEO_` 前缀**的环境变量（如 `SouGEO_XXX`） |
| 全局参数 | `-c/--config`、`-d/--debug`、`-h`、`-v/--version` |

> ⚠️ `-d/--debug` 会在 stdout **JSON 前插入 `[配置] ...` 日志行**。需要干净 JSON 时**不要加 `-d`**；加了就按"从首个 `{` 截取"处理。

### 2.3 输出流、退出码与校验规范

#### 输出流（已分流，务必分清）

| 流 | 内容 |
|----|------|
| **stdout** | 业务数据（JSON / pretty / 链接字符串）、**`--help` 与 Usage（帮助信息）** |
| **stderr** | **错误消息**、**参数补充提示（`paramHint`）**、`⚠ 未知子命令` 提示 |

> ⚠️ **关键**：缺参时打印的"请通过 `--xxx` …"提示走的是 **stderr**，此时 **stdout 为空**。
> 因此 **不能只读 stdout 判断**——必须同时看 **退出码** 与 **stderr 内容**。

**✅ 正常输出干净**（不加 `-d` 时 stdout 即纯 JSON / pretty / 文本），可直接 `json.loads`。

**ℹ️ 编码**：输出为**标准 UTF-8**；CLI 启动时会**自动把 Windows 控制台代码页切到 UTF-8**，cmd/conhost 中文乱码问题已解决。经管道消费输出始终正常。

#### 退出码

| 退出码 | 含义 |
|--------|------|
| `0` | 成功；**以及"引导性提示"**（缺参，见下） |
| `1` | 参数错误（未知命令/子命令、未知 flag、非法取值如 `-r bogus`、`-l` 与类别不匹配、`-P` 不在对照表、`-p/-l/-i/--timeout ≤ 0`、非法 `-o`、`--config` 缺失、轮询超时） |
| `2` | **网络错误**（连接失败、HTTP 错误、响应解析失败） |
| `3` | **业务错误**（服务端 `code != 200`，如 `10003 用户未授权` / `400 报告不存在` / `500 任务失败` / 订单未支付） |

**⚠️ 陷阱：引导性提示 exit 0。** 缺必填参数时不报错、不往 stdout 写内容，而是往 **stderr** 打印一段"请通过 `--xxx` …"帮助文本（含 `说明：` / `用法示例：` 段）并 **exit 0**。实测触发场景：

| 命令 | 缺失项 |
|------|--------|
| `report list` | `--type` |
| `report create` | `--report-type` |
| `report create -r brand` | `--brand` |
| `report create -r industry` | `--industry` |
| `report read` / `report status` / `report share start` / `report share stop` | `report-id` 位置参数 |

> **判定方法**：`退出码 == 0` **且** `stdout 为空` **且** stderr 含 `说明：` 或 `用法示例：` → 视为"缺参未执行"，补齐参数重试，**不能当作成功**。

**✅ 客户端校验已完善（不再有"脏请求直达服务端"的隐患）**：`report create` 的
`--report-level`（与类别匹配）、`--category`、`--platforms`（对照表白名单），
`report list` 的 `--page/--page-size`，`report status` 的 `--interval/--timeout`，以及所有 `-o` 取值
**均已在客户端本地拦截**，非法即 **exit 1**、**不会创建报告**。Agent 仍建议按 §4.2 预校验，以获得更友好的报错与提示。

**输出参数 `-o/--output`**：

| 命令 | `-o` 取值 | 默认 |
|------|-----------|------|
| `auth status` / `auth token` | `json` \| `pretty` | `pretty` |
| `report list` / `read` / `status` / `create` / `share *` | `json` \| `pretty` | `json` |
| `rights list` / `sku list` / `pay` / `check` | `json` \| `pretty` | `pretty` |
| `version show` / `auth login` / `auth logout` | — | 固定文本 |

**健壮解析片段（bash + python）：**
```bash
CLI="${SOUGEO_CLI:-sougeo}"   # 已全局安装（npm i -g @yooai/sougeo-cli）则直接用命令名

# 分别捕获 stdout / stderr / 退出码（提示走 stderr，业务数据走 stdout）
out=$("$CLI" report list --type brand -o json 2>/tmp/err.txt); rc=$?
if [ $rc -eq 0 ] && [ -z "$out" ]; then
  echo "缺参未执行（引导提示仅在 stderr）：" >&2; cat /tmp/err.txt >&2; exit 1
fi
[ $rc -ne 0 ] && { echo "CLI 失败 exit=$rc" >&2; cat /tmp/err.txt >&2; exit 1; }

echo "$out" | python -c "
import sys,json
raw=sys.stdin.read()
i=raw.find('{')                      # 兼容 -d 模式混入的 [配置] 日志行
d=json.loads(raw[i:] if i>=0 else '{}')
assert d.get('code')==200, d.get('msg')
print(d['data']['count'], 'reports')
"
```

---

## 三、命令全景

> **前置**：CLI 需先安装 → `npm i -g @yooai/sougeo-cli`（包 `@yooai/sougeo-cli`，命令名 `sougeo`，详见 §2.0）。

**模块总览：**

| 模块 | 命令 | 职责 |
|---|---|---|
| 认证 | `auth` | 登录 / Token / 登录态 / 退出 |
| 报告 | `report` | **引导式生成、状态轮询、链接/数据读取、分页列表、分享** |
| 权益 | `rights` | 权益余额、套餐列表、下单购买、订单校验 |
| 版本 | `version` | 版本显示 / 检查 / 更新（后两者暂未开放） |

```
sougeo
├── auth                认证管理
│   ├── login           设备码登录（-b/--open 自动开浏览器）
│   ├── token <token>   Token 登录（-t；-o json/pretty）
│   ├── status          登录状态（-v 详细；-o json 结构化登录态）
│   └── logout          退出登录（删除凭证文件）
├── report              报告管理
│   ├── list            查询报告列表（-t brand|industry|shield；分页 -p/-l；-s 已分享）
│   ├── read <id>       读取报告链接(link ★默认) / 报告数据(json，按需)
│   ├── status <id>     查询生成状态（-w 轮询；-i 间隔；--timeout 超时）★
│   ├── create          生成报告（见 §4.2）
│   └── share
│       ├── start <id>  开始分享（-t/--time 3days|week|always；-a/--allow-history）
│       └── stop  <id>  停止分享
├── rights              权益管理
│   ├── list            查询账户权益余额
│   ├── sku             套餐管理（组命令，查询用 sku list）
│   │   └── list        查询可购买套餐列表
│   ├── pay             创建订单并购买（-s/--sku-id -g/--grade -y/--yes）
│   └── check <order>   校验订单支付状态
└── version
    ├── show            显示当前版本
    ├── check           检查更新（暂未开放，exit 0 + 提示）
    └── update          自动更新（暂未开放，exit 0 + 提示）
```

---

## 四、命令详解

### 4.1 认证 `auth`

#### `auth status` — 查询登录状态（支持 JSON）
```bash
sougeo auth status -o json     # {"logged_in":true,"user":{"id","mobile","nickname"}}
sougeo auth status             # pretty 文本
sougeo auth status -v          # pretty + 手机号(脱敏)/ID
```
| 参数 | 说明 |
|------|------|
| `-o, --output` | `json` \| `pretty`（默认 pretty）。**JSON 模式下 `-v` 不改变输出**，始终包含 id/mobile/nickname |
| `-v, --verbose` | pretty 模式下追加显示手机号（脱敏 `178****5971`）与 ID |

**JSON 返回：**
```jsonc
// 已登录
{ "logged_in": true, "user": { "id": "246301", "mobile": "178****5971", "nickname": "..." } }
// 未登录
{ "logged_in": false }
```
> ✅ 手机号始终脱敏、无 HTTP 报文泄露；`logged_in` 布尔值非常适合脚本判断登录态。

#### `auth token <token>` — Token 登录（**Agent 首选，非交互**）
```bash
sougeo auth token <YOUR_TOKEN>
sougeo auth token -t <YOUR_TOKEN>
sougeo auth token -t <YOUR_TOKEN> -o json
```
| 参数 | 说明 |
|------|------|
| `<token>`（位置参数）或 `-t, --token` | token 值 |
| `-o, --output` | `json` \| `pretty`（默认 pretty） |

#### `auth login` — 设备码登录（需人工/浏览器）
```bash
sougeo auth login            # 默认自动打开浏览器
sougeo auth login -b=false   # 不自动打开浏览器
```
| 参数 | 默认 | 说明 |
|------|------|------|
| `-b, --open` | `true` | 是否自动打开浏览器 |

#### `auth logout` — 退出登录
```bash
sougeo auth logout
```
> ⚠️ 会**删除** `$HOME/.SouGEO/credentials.json`。这是破坏性动作，Agent 未经用户确认不要随意执行。

---

### 4.2 报告管理 `report`

#### `report create` — 生成报告

```bash
# 品牌报告（企业品牌，默认国内 7 平台）
# ⚠️ -q 必填（品牌 ≤3 条）：Agent 默认生成一批用户视角问题、用户点选，且问题里【不要出现"小米"】
# ⚠️ -p 产品类型：须先拿到品牌名，再据其真实产品线生成候选（此处 智能手机 来自小米产品线）
sougeo report create --report-type brand --brand 小米 --product 智能手机 \
  --questions "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
  --questions "适合学生党、预算 3000 以内的手机推荐哪款？" \
  --category company --report-level 1 --platforms 1,3,4,5,6,7,8

# 品牌报告（个人品牌：--brand 填个人品牌名，--product 填职业类型；职业候选据该人名/身份生成）
sougeo report create --report-type brand --brand 罗永浩 --product 带货主播 \
  --questions "想找靠谱主播带货，选哪个？" --questions "中小商家找主播带货怎么避坑？" \
  --category personal --report-level 1 --platforms 1,3,4,5,6,7,8

# 行业报告（--origin 默认与 --industry 相同，可省略）
sougeo report create --report-type industry --industry 新能源汽车 \
  --subIndustry "家用纯电" --questions "家充方便、通勤代步的新能源车，推荐哪个？" --report-level 31

# 多条关注问题：重复 -q（不要用逗号连写）；品牌报告最多 3 条、行业报告最多 6 条
sougeo report create -r brand -b 小米 -a company -p 智能手机 \
  -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
  -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
  -q "续航久、充电快的安卓手机怎么选？" -l 1 -P 1,3,4,5,6,7,8
```

**参数表：**

| 参数 | 类型 | 必填 | 默认 | 说明 |
|------|------|------|------|------|
| `-r, --report-type` | string | ✅ | — | 报告类别：`brand` \| `industry` |
| `-b, --brand` | string | 品牌报告必填 | — | **企业品牌**填品牌名或产品名称；**个人品牌**填个人品牌名（人名/IP 名） |
| `-a, --category` | string | 否 | `company` | **品牌类型**：`company`（企业）\| `personal`（个人），仅品牌报告使用 |
| `-p, --product` | string | 否 | — | 品牌报告使用：**企业品牌**填品牌产品类型/产品名；**个人品牌**填**职业类型**。⚠️ 候选须**先拿到品牌名后再据其产品线/身份生成**（见 Step 1A ③） |
| `-i, --industry` | string | 行业报告必填 | — | 行业名 |
| `-s, --subIndustry` | string | 否 | — | 子行业方向，行业报告使用 |
| `-u, --origin` | string | 否 | 同 `--industry` | 原始用户报告输入，行业报告使用 |
| `-q, --questions` | stringArray | **⭐ 业务必填**<br>（CLI 不校验） | — | **关注问题（GEO 测试题）**，⭐ **Agent 默认生成一批问题摆给用户点选**（不要只给空模板）。**上限：品牌报告 ≤3 条、行业报告 ≤6 条**（CLI 无上限，业务自守）。每条须**从用户视角**写、**含决策引导词**、**植入卖点关键词**、**不得含被测品牌名/产品名**（否则 AI 被"喂"答案，测试失去意义）。用重复 `-q` 追加多条，每次 `-q` 的整句作为一条问题（**不按逗号自动切分**）；**不要留空** |
| `-l, --report-level` | int | 否 | `1`（品牌）/ `31`（行业） | 报告类型ID：品牌 `1`极速版/`2`专业版/`3`动态分析版；行业 `31`免费/`32`专业 |
| `-P, --platforms` | intSlice | 否 | — | 平台 ID 列表，逗号分隔；**默认 7 个国内平台**（见 §4.2 Step 2） |
| `-o, --output` | string | 否 | `json` | `json` \| `pretty` |

**校验现状（实测，重要）：**

| 项 | 行为 |
|---|---|
| 缺 `--report-type` / `--brand` / `--industry` | 打印引导提示到 **stderr**，**exit 0**（不创建报告） |
| **缺 `-q`（不传关注问题）** | 🔥 **不报错、照常创建**——CLI **不做必填校验**。**Agent 必须自行拦截**（品牌 ≤3 / 行业 ≤6、非空、不含品牌名），否则报告失去 GEO 测试意义 |
| `-r` 取值非法（非 brand/industry） | **exit 1**（本地拦截） |
| `-a` 取值非法（非 company/personal） | **exit 1**（本地拦截） |
| `-l` 与类别不匹配（如 brand 传 `99`） | **exit 1**（本地拦截） |
| `-P` 含对照表外 ID（如 `99`） | **exit 1**（本地拦截） |
| `-o` 取值非法 | **exit 1**（本地拦截） |
| `-p / -l(分页) / -i / --timeout ≤ 0` | **exit 1**（本地拦截） |

> ✅ 除 `-q` 外，其余关键参数均已在客户端本地校验，非法值**不会**产生脏请求或意外创建报告。
> ⚠️ **`-q` 是本流程唯一的"Agent 必须自守"项**：CLI 放行空值，请严格按 Step 1A ④ / Step 1B ② 的校验清单自查后再提交。
> 白名单参考：`-r ∈ {brand, industry}`；`-a ∈ {company, personal}`；
> `-l`：brand → `{1,2,3}`，industry → `{31,32}`；`-P` ⊂ `{1,3,4,5,6,7,8,9,10,11,12}`（**无 ID 2**）。

- `report create` 为**异步**且**消耗权益**。**创建后用 `report status <id> --watch` 等待完成**（见 §4.2 `report status`），**完成后再用 `report read <id> --type link` 取出报告链接**——不要手工翻列表猜状态，也不要在未完成时取链接。完整闭环见 §4.2 Step 7。

##### 🧭 引导式参数收集流程（Agent 必读）

> **定位**：用户往往只说"帮我做个报告""测测我品牌在 AI 里怎么样"，信息不完整。
> Agent **不要**读到需求就立刻拼参数调用，而应按下面的状态机 **逐步识别 → 缺什么问什么 → 最终确认后一次调用 → 轮询到完成 → 取出报告链接**。

**总原则**

1. **能从用户原话推断的，绝不追问**；只对"确实无法确定"的项发问。
2. **每轮最多问 1~2 个问题**，用可点选的选择项，不要甩一张大表让用户填空。
3. **每个选择项都给推荐默认值**（标 ⭐），用户可直接接受。
4. **信息收集完整后，必须先展示「参数确认单」**，用户确认后才真正调用 `report create`（因为消耗权益、且 CLI 无法撤回）。
5. **⭐ 关注问题 `-q` 是必收集项**（业务必填）。🎯 **交互范式：Agent 默认把「一批适合 GEO 场景的问题」生成好摆给用户，用户点选序号即可**（不要只给空模板让用户从零描述）。**上限：品牌报告 ≤3 条、行业报告 ≤6 条**。每条须**从用户视角**写（真实买家的推荐/排名/痛点疑问），**绝不能含被测品牌名 / 产品名**——否则 AI 被"喂"了答案，GEO 可见度测试失去意义（详见 Step 1A ④ / Step 1B ②）。
6. **⭐ 产品/职业候选必须"后置生成"**：**先拿到品牌名**，再据该品牌的**真实产品线 / 职业身份**生成 3~4 个强相关候选（拿不准就联网查证），**禁止**抛出与品牌无关的通用产品池（详见 Step 1A ③）。
7. **⭐ 地域必须解析为"明确地名"**：用户给模糊地域（"本地""我们这边""附近"）时，按「上下文推断 → **IP 归属地联网兜底** → **用户确认**」三步解析；解析结果需在 Step 5 确认单里让用户核对。**严禁把模糊词原样写进问题、严禁编造地名**（详见 Step 1A ④「模糊地域解析」）。

---

**Step 0 — 意图识别：先判定「品牌报告」还是「行业报告」**

| 用户表述特征 | 判定 |
|---|---|
| 提到具体公司 / 产品 / 人名 / IP（"小米""ChatPPT""罗永浩"） | → **品牌报告** `-r brand` |
| 提到**行业 / 品类 / 赛道**（"新能源汽车""打印机行业""宠物食品"） | → **行业报告** `-r industry` |
| 两者都有（"分析小米在手机行业的位置"） | 优先 **品牌报告**（品牌是主体），行业语境写入 `--questions` |
| 无法判断 | 直接问：「你想分析的是**某个品牌**，还是**整个行业**？」 |

---

**Step 1A — 品牌报告分支**

**① 先定品牌类型 `--category`：个人 vs 企业（首要判断项）**

| 用户原话线索 | 判定 |
|---|---|
| "我们公司""品牌""产品""XX 科技""XX 有限公司""XX 平台/App" | → `company`（企业品牌） |
| "我""个人 IP""博主""主播""老师""律师""医生"或直接是人名 | → `personal`（个人品牌） |
| 未提及 / 有歧义 | **必须询问** |

询问话术：
> 「这次要分析的品牌是 **个人品牌**（你本人 / 某位博主、专家、IP）还是 **企业品牌**（公司、产品、平台）？」

**② 收集品牌名 `--brand`（必填）**

| 品牌类型 | `--brand` 应填 | 示例 |
|---|---|---|
| `company` 企业品牌 | 企业品牌名 **或** 产品名称（用户给哪个用哪个） | `小米`、`ChatPPT`、`钉钉` |
| `personal` 个人品牌 | **个人品牌名**（人名 / IP 名） | `罗永浩`、`李佳琦`、`半佛仙人` |

- 用户没给 → 问：「要分析的**品牌名**是什么？」
- 该参数缺失会被本地拦截（exit 0 引导文本），**不消耗权益**；但仍应先问清再调用。

**③ 收集产品 / 职业信息 `--product`（⭐ 必须"先拿品牌名、再据其生成候选"）**

> 🔥 **硬性顺序要求**：`--product` 的候选**必须在拿到品牌名（②）之后再生成**。
> **严禁**在不知道品牌名时抛出通用产品池（"手机 / 电脑 / 家电 / 其他"）——那样用户选出的很可能与该品牌**完全无关**。
> 顺序：**② 品牌名 → ③ 产品/职业候选**，不可颠倒。

**生成候选的四步（Agent 内部执行）：**

| 步 | 动作 |
|---|---|
| 1 | 已取得品牌名 `--brand`（② 已完成） |
| 2 | **推断该品牌的真实产品线 / 业务领域**：先用自身知识判断；**拿不准就用联网检索**确认（`<品牌名> 官网`、`<品牌名> 主营产品 / 产品线`）。个人品牌则查其**真实职业身份 / 领域** |
| 3 | 生成 **3~4 个与该品牌强相关的候选**，每个候选**附一句"依据"**（为何推荐它），而不是给通用列表 |
| 4 | 允许用户**自行输入**（候选都不合适时） |

**❌ 错误 vs ✅ 正确 对照：**

| 品牌 | ❌ 通用池（禁止，与品牌脱节） | ✅ 先拿品牌名再生成 |
|---|---|---|
| `小米` | 「要分析哪类产品？手机 / 电脑 / 家电 / 其他」 | 小米产品线里这次重点分析哪个？**智能手机** ⭐ / **智能家居·生态链** / **SU7 汽车** / 其他（自己填） |
| `奔图` | 「产品类型是？打印机 / 软件 / 服务」 | 奔图主营打印设备，这次分析 **家用喷墨打印机** ⭐ / **商用激光打印机** / **耗材（硒鼓）** / 其他 |
| `ChatPPT` | 「产品类型？软件 / 硬件 / 服务」 | ChatPPT 是 AI 生成 PPT 工具，这次分析 **AI 生成 PPT** ⭐ / **AI 办公套件** / **AI 演示汇报** / 其他 |
| `罗永浩`（个人） | 「职业类型？老师 / 医生 / 律师」 | 罗永浩的个人品牌，这次聚焦哪个身份？**带货主播·直播电商** ⭐ / **科技创业者·AR** / **知识分享** / 其他 |

**按品牌类型的 `--product` 语义：**

| 品牌类型 | `--product` 语义 | 示例（基于该品牌真实产品线） |
|---|---|---|
| `company` 企业品牌 | **品牌产品类型 或 产品名** | `小米` → `智能手机` / `智能家居` / `SU7 汽车`；`奔图` → `家用喷墨打印机` |
| `personal` 个人品牌 | **职业类型** | `罗永浩` → `带货主播`；`李佳琦` → `美妆带货主播` |

> - `--product` **非必填**：用户明确表示不需要补充时可省略（服务端会按品牌名自行推断）。
> - 但**建议尽量收集**：产品 / 职业越具体，平台曝光与竞品对比越精准。
> - **个人品牌场景下，若用户只给了人名未说职业，应主动追问职业类型**——这是个人品牌报告质量的关键输入。
> - ⚠️ **候选必须可追溯到该品牌**：若无法确认某候选是否属于该品牌，**宁可联网查证或直接让用户输入**，也不要凭猜测给不相关选项。

**④ 关注问题 `-q --questions`（业务必填 ★，默认生成一批 → 用户**点选** ≤3 条）**

> ⚠️ **这是本流程的硬性要求**：`-q` 是 GEO 测试的**测试问题**，没有它报告就失去意义。
> CLI 本身**不做必填校验、也无数量上限**（不传 `-q` 不会报错），但 **Agent 必须强制收集**，**绝不允许空 `-q` 就提交**。
>
> 🎯 **交互范式（本版重点）**：**Agent 默认直接把「一批适合 GEO 场景的问题」生成好摆给用户**，用户**只需点选**（选 N 条 / 换几条 / 自己写）。
> ❌ **不要**让用户从零描述需求、也不要只给一个空模板让用户填空。

**为什么问题必须"从用户视角、贴合 GEO 场景"：**

GEO 测的是 **AI 在真实用户自然提问下的自发推荐**。所以每道题都应还原成**一个真实买家/使用者会问的话**——是他的**使用场景、推荐需求、排名比价、痛点求解**，而**不是品牌视角的自我描述**。

| ❌ 品牌视角（测不出 GEO） | ✅ 用户视角 + GEO 场景（推荐） |
|---|---|
| 小米手机的性能怎么样？ | 适合学生党、预算 3000 以内的手机，推荐哪款？ |
| 小米的售后服务好不好？ | 手机售后网点多、修得快的是哪几家？口碑怎么样？ |
| 小米有什么优势 | 想买个能用三四年的手机，哪家更靠谱、值得选？ |

> 前者把品牌名写进去了 → AI 只会顺着品牌夸；后者才是**用户真会问的推荐/排名题**，AI 必须自己"点名"推荐谁 —— 这才测得出可见度。

**三条铁律（违反任一即失效）**

| # | 铁律 | 说明 |
|---|---|---|
| 1 | 🔥 **绝不含被测品牌名 / 产品名** | 全称、简称、谐音、英文名**都不行**。问题里带品牌名 = 把答案"喂"给 AI（品牌必然被提到），**GEO 可见度测试就废了** |
| 2 | **必含决策引导词** | 从下列任选≥1：推荐类（推荐 / 值得选 / 口碑好 / 靠谱 / 公认 / 好评 / 种草）、排名类（排名 / 前几名 / Top / 排行榜 / 领先）、对比抉择类（哪个 / 哪家 / 怎么选 / 选哪个 / 更适合 / 还是）。保证 AI 以"推荐列表 / 对比建议"作答 |
| 3 | **深度植入卖点关键词** | 把品牌的**核心卖点 / 功能 / 用户痛点**当"刚性筛选条件"写进问题（如"主动降噪""续航 40 小时""按秒计费""终身质保"），提高高转化长尾词的命中率 |

**四类 GEO 问题标签（候选池须覆盖，尽量均衡）**

| 标签 | 定位（用户视角） | 示例（品类=智能手机，被测品牌=`小米`） |
|---|---|---|
| `scenario_best` | 特定人群 / 使用场景的"最佳推荐" | 适合学生党、预算 3000 以内的手机推荐哪款？ |
| `comparative_alternatives` | 平替 / 抉择类（**不含竞品名**，只描述需求） | 除了几个大牌，还有哪些性价比高的手机值得考虑？ |
| `painpoint_solution` | 围绕行业痛点 / 刚需的解决方案推荐 | 续航久、充电快的安卓手机怎么选？ |
| `local_ranking` | 结合地域的排名 / 口碑推荐 | 成都线下买手机，哪家门店口碑最好？ |

**生成流程（Agent 内部执行，5 步）**

1. **抽关键词**：从 `品牌名 + 产品类型/职业`（如用户额外给了卖点则一并纳入）提炼 **3~6 个卖点 / 痛点词**。
   - 信息不足时可**选填**问一句：「这个产品/服务最想突出的 1~2 个卖点是什么？」——**可不问**，自行合理推断。
2. **判定与解析地域**（决定是否出 `local_ranking`）：
   - **明确地域**（"成都""广东省""海淀区"）→ 直接采用，**必须生成 1 条含该地名的 `local_ranking`**。
   - **模糊地域**（"我们这边""本地""附近""南方""周边城市""这边"）或**粒度不足**（只给省市但需更细）→ 走下方「**模糊地域解析**」流程。
   - **未提地域、面向全国** → 跳过 `local_ranking`，**不要硬造地名**。

   **🔍 模糊地域解析（三步，逐级降级）：**

   | 步 | 动作 |
   |---|---|
   | ① 上下文推断 | 优先用**会话/系统中已知的用户位置信息**（用户资料、系统地区/时区设置、历史对话里出现过的城市）解析为明确地名 |
   | ② 联网兜底 | 上下文无果 → **查询当前网络出口 IP 的归属地**（联网检索"IP 归属地查询 / 我的 IP 在哪个城市"），取到「省 / 市 / 区县」级 |
   | ③ 用户确认 | 仍无法确定（VPN、内网、查不到）→ **必须主动询问用户**：「这次的问题想聚焦哪个城市 / 区域？」——给出候选项或让用户输入明确地域 |

   - ❌ **严禁**把模糊词（"本地""我们这边"）**原样写进问题**；❌ **严禁凭猜测编造地名**。
   - 解析出的地域**必须写进 Step 5 参数确认单**，让用户核对后才可用。
3. ⭐ **默认生成「问题候选清单」**：按上表 **四类各出 1~2 条**，凑成 **5~7 条**候选（每条 ≤30 字、口语化、守三条铁律）。
   - **每条都必须能直接提交**（用户选中即用，无需再加工）。
   - 每条后标注：**标签** + **为什么这条适合测 GEO**（一句话，帮用户判断）。
4. **请用户点选（核心交互）**：把候选清单摆出来，**请用户挑选 ≤3 条**。
   - **预置默认**：先替用户勾好推荐的 3 条（标 ⭐，覆盖度最高：`scenario_best` + `comparative_alternatives` + `painpoint_solution`；**有地域则用 `local_ranking` 替换一条**）；
   - 用户只需回复**序号**（如"1、3、5"）即可；也可说"换个方向"或自己写。
5. **落地校验**：按下方校验清单过一遍，再写入 `-q`（重复 `-q`，不逗号连写）。

**🗳️ 询问话术（默认给好，用户只点选）：**

> 「报告会拿这些问题去 7 个 AI 平台提问，看 AI 会**自然推荐**哪些品牌 —— 这是 GEO 可见度的测试题。
> 我已经按「场景 / 抉择 / 痛点 / 地域」四类拟好了候选，**推荐选前 3 条（标 ⭐）**，你**回复序号**即可（如 `1 3 5`），也可以自己写：
>
> **⭐ ① `scenario_best`**　适合学生党、预算 3000 以内的手机推荐哪款？
> 　　（测：真实选购场景下 AI 会不会点名推荐）
> **⭐ ② `comparative_alternatives`**　除了几个大牌，还有哪些性价比高的手机值得考虑？
> 　　（测：在"要平替"的抉择语境里的被提及率）
> **⭐ ③ `painpoint_solution`**　续航久、充电快的安卓手机怎么选？
> 　　（测：命中卖点关键词"续航/快充"时的可见度）
> 　④ `comparative_alternatives`　不想买贵的，有没有好用又不踩坑的手机？
> 　⑤ `scenario_best`　大学生上课记笔记用，手机选哪个更合适？
> 　⑥ `local_ranking`（你说过地区才出）　成都线下买手机，哪家门店口碑最好？
> 　　　↳ 你说的"我们这边"，我按**你的网络位置**解析为**成都**；若不是，请告诉我具体城市。
> 　⑦ 其他（请直接输入你的问题）
>
> ⚠️ 提醒：**问题里不能写"小米"**，否则 AI 被"喂"了答案，测不出真实可见度。」

**校验清单（提交前自查）：**
- [ ] `-q` 数量 **≥1 且 ≤3**（品牌报告上限；超过须让用户取舍）
- [ ] 每条**不含被测品牌名 / 产品名**（含品牌时须提示用户改写）
- [ ] 每条**含决策引导词**（推荐 / 排名 / 哪个 / 怎么选 …）
- [ ] **至少 1 条植入卖点 / 痛点关键词**；有地域信息时**至少 1 条带真实地名**
- [ ] **地域必须是"明确地名"**：用户给模糊词（"本地""这边"）时已按「模糊地域解析」解析或经用户确认，**未把模糊词原样写入问题**
- [ ] **每条都是"用户视角的自然问句"**（不是品牌视角的自我描述、不是关键词堆砌）
- [ ] 标签尽量分散（不要 3 条全是同一类型）

> 多条问题用**重复 `-q`**（每个 `-q` 一整句），**不要用逗号连写**。
> **候选清单里未被选中的条目不提交**（品牌上限 3 条）；它们可留作下一轮测试的替换项。

---

**Step 1B — 行业报告分支**

**① 行业名称 `--industry`（必填）——能识别就直接用，识别不到必须引导**

| 情况 | 处理 |
|---|---|
| 用户话里**能直接识别**行业名（"分析一下新能源汽车行业"→ `新能源汽车`） | 直接采用，向用户**复述确认**即可 |
| 只给了模糊词（"汽车""吃的""搞个互联网的"） | 引导**收敛**：给 3~4 个候选让用户选，或让用户输入标准行业名 |
| 完全没提行业 | 问：「你想分析**哪个行业**？（请输入行业名，如"新能源汽车""宠物食品""SaaS 软件"）」 |

引导话术示例：
> 「"汽车"范围比较大，你想分析的是下面哪个？
> - 新能源汽车 ⭐
> - 燃油车
> - 汽车零部件
> - 其他（请直接输入行业名）」

**② 关注问题 `-q --questions`（业务必填 ★，默认生成一批 → 用户**点选** ≤6 条）**

- 用途同品牌报告：这是拿去各 AI 平台提问的**测试问题**。**CLI 不校验必填，但 Agent 必须强制收集**，不得留空。
- **交互范式同 Step 1A ④**：**默认直接把一批适合 GEO 场景的问题生成好摆给用户，用户只点选**（回复序号即可），不要只给空模板让用户从零描述。
- ⭐ **行业报告上限是 6 条**（比品牌报告的 3 条更宽，因为行业面更广、需要覆盖更多子场景）。

**行业场景的三点差异**：
1. **关键词来源**：把「行业 + 子行业 + 行业痛点/趋势」转为卖点/痛点词（如"续航虚标""售后网点少""价格战"），而不是某个品牌的产品卖点。
2. **标签适配**：`scenario_best` → 场景化选择建议；`comparative_alternatives` → "除了头部大牌还有哪些"；`painpoint_solution` → 行业共性痛点；`local_ranking` → 带地域的行业口碑/门店排名。
3. **问题视角**：行业题要写成一个**普通用户在面对该行业时的真实疑问**（怎么选、哪家靠谱、有哪些坑），而不是行业分析师的视角。

**生成流程**：同 Step 1A ④ 的 5 步，但**候选清单放大到 8~10 条**（四类各 2~3 条），**用户最多可选 6 条**。

| 标签 | 示例（行业=`新能源汽车`；有地域=`成都`） |
|---|---|
| `scenario_best` | 家充方便、通勤代步的新能源车，推荐哪个？ |
| `comparative_alternatives` | 除了头部几家，还有哪些新能源车品牌值得考虑？ |
| `painpoint_solution` | 冬天续航虚标、保值率低的问题，有没有靠谱的应对选择？ |
| `local_ranking` | 成都买新能源车，哪家门店和售后口碑最好？ |

- **🗳️ 询问话术（同品牌报告：默认给好，用户点选序号）：**
  > 「报告会拿这些问题去 7 个 AI 平台提问，看 AI 会**自然提到**哪些品牌 —— 这是行业 GEO 可见度的测试题。
  > 我已按「场景 / 抉择 / 痛点 / 地域」四类拟好候选，**最多可选 6 条**（推荐前 4 条标 ⭐），**回复序号**即可（如 `1 2 3 5`），也可自己写：
  >
  > **⭐ ① `scenario_best`**　家充方便、通勤代步的新能源车，推荐哪个？
  > **⭐ ② `comparative_alternatives`**　除了头部几家，还有哪些新能源车品牌值得考虑？
  > **⭐ ③ `painpoint_solution`**　冬天续航虚标、保值率低，有没有靠谱的应对选择？
  > **⭐ ④ `scenario_best`**　预算 15 万左右、家用为主的新能源车怎么选？
  > 　⑤ `painpoint_solution`　充电桩少、售后网点远的顾虑，哪些品牌处理得更好？
  > 　⑥ `comparative_alternatives`　不想随大流，还有哪些小众但口碑好的新能源车？
  > 　⑦ `local_ranking`（你说过地区才出）　成都买新能源车，哪家门店和售后口碑最好？
  > 　⑧ 其他（请直接输入，**注意不要写进具体品牌名**）
  >
  > ⚠️ 提醒：**问题里不要写具体品牌名**，否则 AI 被"喂"了答案，测不出行业内的真实可见度格局。」
- **地域处理同 Step 1A ④**：用户给**模糊地域**（"我们这边""本地"）时，按「**模糊地域解析**」三步（上下文推断 → IP 归属地联网兜底 → 用户确认）解析为明确地名；解析不出**必须问用户**，**严禁把模糊词或猜测的地名写进问题**。
- 也可回填用户原话里已含的疑问句（如"商务安全手机哪个牌子最好？"——需先确认其中不含具体品牌名）。
- 多条问题用**重复 `-q`**（每个 `-q` 一整句），**不要用逗号连写**。
- 提交前自查：**数量 1~6**、**不含具体品牌名**、**含决策引导词**、**尽量植入行业痛点词**、**有地域信息时含真实地名**、**是用户视角的自然问句**。

**③ 子行业方向 `--subIndustry`（可选）**

- 用户提到更细分方向时填入（如 `商务/安全手机`）；没提就跳过。
- `--origin` 默认等于 `--industry`，一般**无需显式传**。

---

**Step 2 — AI 平台选择 `--platforms`（默认勾选「7 个国内平台」）**

**默认勾选 7 个国内平台**：

```
-P 1,3,4,5,6,7,8
```

| ID | 平台 | 类型 | 默认勾选 |
|----|------|------|---------|
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

> ⚠️ 合计 **11 个可用平台**（ID `2` 不在对照表内，**切勿加入**）。
> **默认 = 国内 7 个**；海外 4 个为可选项，用户明确要求时才追加。

引导话术：
> 「报告覆盖的 AI 平台 **默认勾选 7 个国内平台** —— 豆包、腾讯元宝、DeepSeek、Kimi、智谱清言、千问、文心一言。
> 是否按默认（推荐 ⭐）？还是要**追加海外平台**（ChatGPT、Claude、Gemini、Grok）或增删其中某几个？」

| 用户回答 | 处理 | `-P` 取值 |
|---|---|---|
| "默认 / 国内就够了 / 你定" ⭐ | 国内 7 个 | `1,3,4,5,6,7,8` |
| "全选 / 都要 / 加上海外" | 11 个全选 | `1,3,4,5,6,7,8,9,10,11,12` |
| "只看国内" | 国内 7 个 | `1,3,4,5,6,7,8` |
| 点名部分平台 | 按名映射 ID 后拼接 | 如 `1,4,9` |

---

**Step 3 — 报告类型 `--report-level`（最终确认）**

| 类别 | `-l` | 名称 | 权益字段 | 定位 |
|------|------|------|----------|------|
| brand | `1` | **极速版** ⭐ 默认 | `base` | 快速出结果，基础指标 |
| brand | `2` | 专业版 | `pro` | 指标更全 + AI 分析 |
| brand | `3` | 动态分析版 | `plus` | 动态追踪 + 舆情 + GEO 建议 |
| industry | `31` | 免费版 | — | 行业基础报告（免费） |
| industry | `32` | 专业版 / 行业版 | `industry_pro` | 行业深度报告 |

引导话术：
> 「报告类型选哪个？
> - **极速版 ⭐（默认，推荐首次使用）**
> - 专业版（指标更全，消耗"专业版"权益）
> - 动态分析版（含动态追踪与舆情，消耗"动态分析版"权益）」

- 用户未指定 → **默认极速版 `-l 1`**；行业报告未指定 → 默认 `31`。

---

**Step 4 — 权益校验（提交生成前必做）⚠️**

> **规则**：提交生成前，**除「极速版」外**，其余档位都必须**先校验权益**；若不具备对应权益，则需**先开通权益**（购买相应套餐）后才能使用。

**① 按报告类型 → 权益字段映射表：**

| 报告类型 | `-l` | 需校验的权益字段 | 是否需校验 |
|---------|------|-----------------|-----------|
| 品牌 · 极速版 | `1` | — | ❌ 无需校验（默认可用） |
| 品牌 · 专业版 | `2` | `pro` | ✅ 必须校验 |
| 品牌 · 动态分析版 | `3` | `plus` | ✅ 必须校验 |
| 行业 · 免费版 | `31` | — | ❌ 无需校验 |
| 行业 · 专业版 / 行业版 | `32` | `industry_pro` | ✅ 必须校验 |

**② 校验流程：**

```bash
# 1) 查询账户权益余额
sougeo rights list -o json     # → data: {base, pro, plus, competitor_base, agent, industry_pro, ""}

# 2) 依据 -l 取出对应字段值（字符串数字）并判断
#    极速版(1) / 行业免费版(31) → 跳过校验，直接提交
#    专业版(2)   → 检查 data.pro           > 0
#    动态分析版(3) → 检查 data.plus          > 0
#    行业专业版(32) → 检查 data.industry_pro > 0
```

**③ 校验结果处理：**

| 情况 | 处理 |
|------|------|
| 余额 > 0 | ✅ 展示「参数确认单」（Step 5），确认后提交 |
| 余额 = 0 / 字段缺失 | ⛔ **中止提交**，提示："当前账户**没有「XXX」权益**，需先开通后才能生成该类型报告。" → 给出开通路径 |
| 极速版 / 行业免费版 | 跳过校验（仍建议展示当前余额供参考） |

**④ 无权益时引导开通（复用工作流 C）：**

```bash
sougeo rights sku list -o json     # 展示可购套餐与实时价格 → 用户确认
sougeo rights pay --sku-id <SKU_ID> [-g base|pro] --yes -o json
#  → 用户完成支付
sougeo rights check <order_no> -o json
sougeo rights list -o json         # 复核余额，通过后继续生成
```

套餐 ↔ 报告类型对应关系（以 `rights sku list` 实时返回为准）：

| 套餐 SKU | 名称 | `extend._type` | 可解锁 |
|---------|------|---------------|--------|
| `geo-rights-base` | 限时极速版 | `base` | 品牌 · 极速版 |
| `geo-rights-pro` | 专业版 | `pro` | 品牌 · 专业版 |
| `geo-rights-plus` | 动态分析版 | `plus` | 品牌 · 动态分析版 |
| `geo-rights-industry-pro` | 搜集星行业版 | `industry_pro` | 行业 · 专业版 |

> ⚠️ 开通权益属**外部付费动作**，必须先向用户确认套餐与价格，**不得擅自下单**。

---

**Step 5 — 参数确认单（调用前必做）**

信息收集齐、**权益校验通过**后，**先把确认单展示给用户**，等用户点头再调 CLI：

```
即将创建报告，请确认：

【报告类型】品牌报告（企业品牌）
【品牌名】小米
【品牌类型】企业（company）
【产品/职业】智能手机   ← 基于"小米"产品线生成的候选，用户已选定
【关注问题】用户从候选清单中点选，**不含"小米"**、均为用户视角的自然问句
          1) 除了几个大牌，还有哪些性价比高的手机值得考虑？   [comparative_alternatives]
          2) 适合学生党、预算 3000 以内的手机推荐哪款？        [scenario_best]
          3) 续航久、充电快的安卓手机怎么选？                  [painpoint_solution]
          └ 品牌报告 ≤3 条；行业报告最多可到 6 条
【地域】成都（用户原话"我们这边"，已按网络位置解析；如不符请纠正）
        └ 仅当关注问题里含 `local_ranking` 时才显示；无地域则不出现此行
【AI 平台】国内 7 个 —— 豆包、腾讯元宝、DeepSeek、Kimi、智谱清言、千问、文心一言
【报告类型】极速版（无需校验权益）
         └ 若为专业版/动态分析版/行业版，此处显示"权益已校验通过，余额 X"

确认后我将立即创建（异步生成，需等待数分钟），完成后我会把报告链接发给你。是否继续？
```

> **关注问题必须非空**（**品牌 ≤3 条 / 行业 ≤6 条**，用户已点选、不含品牌名；建议标注各条的类型标签）；为空时**不得进入本步**，回到 Step 1A ④ / Step 1B ② 补齐。

> **产品/职业**一栏应能说明"候选来自该品牌的产品线"，若用户是自己输入的也一并注明。

> **地域**一栏仅在问题含 `local_ranking` 时出现；**若是解析出来的（非用户原话直接给出），必须注明解析来源并请用户核对**。

> 若用户选的是**非极速版**，确认单中必须体现**权益已校验**的信息；若权益不足，**不进入本步**，先走 Step 4 ④ 开通流程。

> `report create` **消耗权益且 CLI 无法撤回**，因此这一步不可省略。

---

**Step 6 — 参数速查（收集结果 → 命令行映射）**

| 场景 | 必填 | 建议填 | 命令模板 |
|---|---|---|---|
| 企业品牌报告 | `-r brand -b <品牌名/产品名> -q "Q1"` | `-a company -p <产品类型> -q "Q2" -q "Q3"` | `report create -r brand -b X -a company -p Y -q "Q1" -q "Q2" -l 1 -P 1,3,4,5,6,7,8` |
| 个人品牌报告 | `-r brand -b <个人品牌名> -q "Q1"` | `-a personal -p <职业类型> -q "Q2"` | `report create -r brand -b X -a personal -p 律师 -q "Q1" -q "Q2" -l 1 -P 1,3,4,5,6,7,8` |
| 行业报告 | `-r industry -i <行业名> -q "Q1"` | `-s <子行业> -q "Q2" -q "Q3"` | `report create -r industry -i X -s Z -q "Q1" -q "Q2" -l 31 -P 1,3,4,5,6,7,8` |

> `-q` 为**业务必填**（⭐ Agent 默认生成一批问题、用户**点选序号**；**品牌 ≤3 条 / 行业 ≤6 条**；**不含品牌名**）；CLI 不校验也不限数量，Agent 必须自查。
> `-P` 为**国内 7 个平台**（默认）；用户要求加海外时追加 `,9,10,11,12` 即为 11 个全选。
> 多条关注问题用**重复 `-q`**（每个 `-q` 一整句），**不要用逗号连写**。

---

**Step 7 — 创建后等待完成 → 取报告链接（生成闭环，必做）★**

> `report create` 只负责**提交任务**（异步），返回时报告**尚未生成完**。
> Agent 必须在创建后 **① 轮询状态**，**② 确认完成后**再**③ 取出报告链接**给用户。三步缺一不可。

**① 创建并拿到 `report_id`**

```bash
# ⚠️ -q 必填：Agent 默认生成一批用户视角问题 → 用户点选（品牌 ≤3 / 行业 ≤6），且不含品牌名
out=$("$CLI" report create -r brand -b 小米 -a company -p 智能手机 \
        -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
        -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
        -l 1 -P 1,3,4,5,6,7,8 -o json)
```

从返回 JSON 的 `data` 中提取报告 ID：**优先 `data.report_id`，其次 `data.id`**。

**② 轮询状态（标准做法：`report status --watch`）**

```bash
"$CLI" report status "$RID" --watch --interval 5 --timeout 600 -o json
```

| 结果 | 判定 | 下一步 |
|------|------|--------|
| exit 0 且 `data.status == "completed"` | ✅ 已完成 | → 进入 ③ 取链接 |
| exit 0 且 `data.status == "opened"` | ✅ 已公开（终态） | → 进入 ③ 取链接 |
| exit 0 且 `data.status == "failed"` | ❌ 生成失败 | **中止**，告知用户失败并建议调整参数重试 |
| exit 1（等待超时） | ⏳ 仍未完成 | 告知用户"仍在生成中"，稍后再执行 `report status <id>` 继续查询；**不要**取链接 |
| exit 3（如 `code 10003` 未授权） | 🔒 登录失效 | 引导重新登录后再查 |

> - 默认 `--timeout 300`（5 分钟）**通常不够**（报告生成需数分钟以上），**建议显式给 `--timeout 600` 或更大**。
> - 若已超时，**不要**改用手工翻 `report list` 猜状态——重复执行 `report status <id>` 即可续查（幂等、不消耗权益）。
> - 非终态取链接会失败或拿到空/错误内容，**必须等待终态**。

**③ 取出报告链接并交付（默认只给 link，不拉 JSON）★**

```bash
"$CLI" report read "$RID" --type link
```

- 成功时**直接输出链接字符串**（不是 JSON 包裹），形如
  `https://<域名>/ReportResults/?id=<report_id>&reportTypeId=<n>`。
- 建议用 `grep -oE 'https://[^ ]+'` 提取，防止混入其他文本。
- 若返回 `{"code":10003,"msg":"用户未授权"}` → 登录失效，先重新登录。

**🚫 默认不要调用 `report read --type json`**

- 完整报告数据（JSON）体积大、直接展示对用户价值低，**默认不拉取**。
- 只有当用户**明确要求**「给我完整数据 / 要 JSON / 要明细指标做二次分析」时，才执行 `report read <id> --type json`（结构见 `references/report-schema.md`）。

**✅ 默认交付形态 = 基础小结 + 报告链接**（引导用户点链接看完整报告）

**品牌报告**给出 3~5 行**基础小结**（素材轻量获取，不拉完整报告）：
优先用 `report status` 返回的 `data` 中已有的字段（如 `brand_index`）；字段不全时再执行一次
`report list --type brand -p 1 -l 5 -o json` 定位该 `report_id` 的条目，取
`brand_index` / `report_type_id` / `started_at`。

> ⛔ **品牌报告小结不得出现「行业归属」**：`行业归属 / industry_name` **只属于行业报告**，
> 品牌报告的语义里没有"行业归属"这一项。即使 `report list` 返回体里带了 `industry_name`，
> **也不要写进品牌报告的小结**（该字段仅对 `--type industry` 有意义）。

```
📊 小米（企业品牌）GEO 报告已完成

· 品牌指数：60.98
· 报告类型：极速版
· 覆盖平台：国内 7 个（豆包、腾讯元宝、DeepSeek、Kimi、智谱清言、千问、文心一言）
· 测试问题：除了几个大牌，还有哪些性价比高的手机值得考虑？；适合学生党、预算 3000 以内的手机推荐哪款？
· 生成时间：2026-10-08 22:15:03

🔗 查看完整报告：https://<域名>/ReportResults/?id=<report_id>&reportTypeId=1

点击链接即可查看完整报告（AI 可见度、竞品对比、平台曝光等详细指标）。
```

**行业报告**小结同理，可展示：报告名称、**行业归属（行业名）**、报告类型、覆盖平台、测试问题、生成时间 + 链接。

> - 小结**只做陈述**，不要编造报告内的具体结论（那些数据在完整报告页里）。
> - **字段归属要分清**：`品牌指数` 属品牌报告；`行业归属` 属行业报告——两者不要混用。
> - 结尾**必须给出可点击的链接**并一句话引导用户查看完整报告。

**一键闭环（复制即用）**

```bash
CLI="${SOUGEO_CLI:-sougeo}"

# ① 创建 → 提取 report_id（优先 data.report_id，回退 data.id）
#    ⚠️ -q 必须由用户点选确认（品牌 ≤3 / 行业 ≤6），且不含品牌名
RID=$("$CLI" report create -r brand -b 小米 -a company -p 智能手机 \
        -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
        -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
        -l 1 -P 1,3,4,5,6,7,8 -o json \
      | python -c "import sys,json;d=json.load(sys.stdin);a=d.get('data') or {};print(a.get('report_id') or a.get('id') or '')")

# 若未取到 id，回退到列表首条（最新）
[ -z "$RID" ] && RID=$("$CLI" report list --type brand -p 1 -l 5 -o json \
      | python -c "import sys,json;d=json.load(sys.stdin);r=d['data']['data'];print(r[0]['report_id'] if r else '')")
[ -z "$RID" ] && { echo "未获取到 report_id，创建可能失败"; exit 1; }

# ② 轮询至终态（最多 10 分钟）
"$CLI" report status "$RID" --watch -i 5 --timeout 600 -o json
case $? in
  0) ;;
  1) echo "仍在生成中，请稍后重试 report status $RID"; exit 0 ;;
  *) echo "查询失败（exit $?）"; exit 1 ;;
esac

# ③ 确认 completed/opened 后取链接（只取 link，不拉 json）
ST=$("$CLI" report status "$RID" -o json | python -c "import sys,json;d=json.load(sys.stdin);print((d.get('data') or {}).get('status') or '')")
if [ "$ST" = "completed" ] || [ "$ST" = "opened" ]; then
  "$CLI" report read "$RID" --type link | grep -oE 'https://[^ ]+' | head -1
else
  echo "报告状态为 $ST，暂不取链接"
fi
```

> **若 `report create` 的返回体没有 `report_id`**（服务端未回传）：立即执行
> `report list --type brand|industry -p 1 -l 5 -o json`，取**第一条**（最新的报告即刚创建的那份）的 `report_id`，
> 再进入 ② 轮询。不要跳过轮询直接取链接。

---

#### `report list` — 查询报告列表（分页）
```bash
sougeo report list --type brand                    # 品牌报告（第1页，每页10条）
sougeo report list --type industry                 # 行业报告
sougeo report list --type shield                   # 星盾报告
sougeo report list --type brand -s                 # 已分享的报告（-s 即 --shared）
sougeo report list --type brand -p 2 -l 20 -o json
sougeo report list --type brand -o pretty          # [n] 名称 (id: xxxxx)
```
| 参数 | 必填 | 默认 | 说明 |
|------|------|------|------|
| `-t, --type` | ✅ | — | **`brand` \| `industry` \| `shield`（星盾报告）**；缺省/非法 → 引导提示或 exit 1 |
| `-s, --shared` | 否 | false | 查询已分享的报告链接列表 |
| `-p, --page` | 否 | `1` | 页码，从 1 开始；`≤0` → exit 1 |
| `-l, --page-size` | 否 | `10` | 每页条数；`≤0` → exit 1 |
| `-o, --output` | 否 | `json` | `json` \| `pretty` |

**JSON 返回：**
```jsonc
{
  "code": 200, "msg": "success",
  "data": {
    "count": 175,                        // 总数（翻页边界）
    "data": [{                           // ⚠️ 双层嵌套 data.data
      "report_id": "5UvXVM3sqT9wCBtq",
      "report_name": "奔图家庭打印机品牌分析",
      "status": "completed",             // pending / processing / completed / failed / opened
      "progress": 100,                   // 0-100
      "progress_text": "分析完成",        // 可能为 null
      "started_at": "2026-09-22 10:37:49",
      "banner_url": "https://...",       // 上游转义 \/，合法 JSON
      "report_type_id": "极速版",         // 中文类型名
      "report_type": 1,                  // 类型 ID
      "industry_name": "专用设备制造",    // ⚠️ 仅【行业报告】有意义；品牌报告请忽略此字段
      "brand_index": "60.98",            // 品牌指数（字符串）★ 仅品牌报告有
      "download_num": 0,                 // 下载次数
      "view_num": 0,                     // 👀 查看次数（分享后被查看的计数；网站侧另有查看者明细）
      "agent_end_at": null,
      "close_at": null,
      // share_code 非空 → 分享链接 = https://sougeo.com/ReportResults?share_code=<share_code>
      "share": { "share_code": null, "expired_at": null, "is_current": false }
    }]
  }
}
```
> 状态机：`pending` → `processing` → `completed` / `failed`（`opened` 为已公开终态）。
> 用 **`sougeo report status <id>`** 精确查询状态，无需自己翻列表（见下）。
>
> ✅ **已实测字段**（v1.0.0）：`report_id` / `report_name` / `status` / `progress` / `progress_text` /
> `started_at` / `banner_url` / `report_type_id` / `report_type` / `industry_name` / `brand_index` /
> `download_num` / `view_num` / `agent_end_at` / `close_at` / `share{share_code,expired_at,is_current}`；
> 且 `report create` 成功返回体为 **`{code:200, data:{report_id:"..."}, msg:"success"}`**、
> `report status` 返回 **`{code:200, data:{status:"pending"}, msg:"success"}`**。
>
> ⚠️ **字段归属**：`industry_name`（行业归属）**只对行业报告有意义**，品牌报告的语义中不含"行业归属"；
> 上方示例虽是品牌报告（`report_type: 1`），但该字段**不应写入品牌报告的小结**（见 §4.2 Step 7 ③）。
> 反之 `brand_index`（品牌指数）**仅品牌报告有**。

#### `report read <report-id>` — 读取报告数据 / 链接
```bash
sougeo report read <id> --type link    # ★ 默认用法：取在线预览链接
sougeo report read <id> --type json    # 仅按需：完整报告数据（默认不调用）
```
| 参数 | 必填 | 默认 | 说明 |
|------|------|------|------|
| `<report-id>` | ✅ | — | 报告 ID |
| `-t, --type` | ✅ | — | `link`（★ 默认交付）\| `json`（按需） |
| `-o, --output` | 否 | `json` | `json` \| `pretty` |

- **`--type link`（默认交付形态）**：**成功时直接输出链接字符串**（不是 JSON 包裹）；建议用 `grep -oE 'https://[^ ]+'` 提取以防混入其他文本。拿到后**把链接交给用户，并附一段基础小结**（品牌报告：品牌指数等，见 §4.2 Step 7 ③），引导用户点链接看完整报告。
- **`--type json`（按需，默认不用）**：返回 `{code, msg, data:<报告对象>}`，体积大，**仅当用户明确索要完整数据/明细时才调用**；结构见 `references/report-schema.md`。
- 未登录/凭证失效时返回 `{"code":10003,"msg":"用户未授权"}`，此时应先 `auth status` 检查并引导重新登录。
- 报告未完成时读取可能失败（`任务失败`/空数据），**应先用 `report status` 确认 `completed`/`opened` 再读**。

#### `report status <report-id>` — 查询生成状态 ★

> **这是等待报告完成的标准做法**：创建报告后用它轮询，替代手工反复查列表。

```bash
sougeo report status <id>                               # 单次查询当前状态
sougeo report status <id> --watch                        # 轮询直到终态（默认 5s 间隔、300s 超时）
sougeo report status <id> --watch -i 3 --timeout 180     # 自定义 3s 间隔 / 180s 超时
sougeo report status <id> -o pretty                      # 彩色标签输出
```

| 参数 | 默认 | 说明 |
|------|------|------|
| `<report-id>` | 必填 | 报告 ID（缺省时打印引导提示到 stderr，**exit 0**） |
| `-w, --watch` | `false` | 轮询等待，直到进入终态（`completed` / `failed` / `opened`） |
| `-i, --interval` | `5` | 轮询间隔（秒），仅 `--watch` 生效；`≤0` → exit 1 |
| `--timeout` | `300` | 轮询总超时（秒），仅 `--watch` 生效；`≤0` → exit 1 |
| `-o, --output` | `json` | `json` \| `pretty` |

**状态取值：**

| 状态 | 中文 | 是否终态 | `--watch` 是否继续 |
|------|------|---------|-------------------|
| `pending` | 待处理 | 否 | 继续轮询 |
| `processing` | 处理中 | 否 | 继续轮询 |
| `base_completed` | 基础部分已完成 | ❌ **否**（实测出现，**勿当终态**） | 继续轮询 |
| `completed` | 已完成 | ✅ 是 | 停止 |
| `failed` | 失败 | ✅ 是 | 停止 |
| `opened` | 已公开 | ✅ 是 | 停止 |

> ⚠️ **实测补充**：报告生成过程中还会出现 **`base_completed`**（基础数据先就绪，其余指标仍在生成）。
> 它**不是终态**——此时取链接可能拿到不完整内容，**必须继续轮询到 `completed`/`opened`**。
> 判定终态时请用白名单：`status in {completed, opened, failed}`，**不要**用"非 pending/processing 即完成"来推断。

**JSON 返回：**
```jsonc
{ "code": 200, "msg": "success", "data": { "status": "processing", ... } }
```
> 状态字段固定为 `data.status`；其余字段以实际返回为准。

**pretty 输出：**
```
✓ 报告状态: completed（已完成）    # 终态 → 绿色
✗ 报告状态: failed（失败）         # 失败 → 红色
… 报告状态: pending（待处理）      # 非终态 → 黄色
  详情:
  { ...完整 data... }
```

**`--watch` 行为细节：**
- 每轮把当前状态写到 stdout（JSON 或 pretty）；进入终态后以 **exit 0** 结束。
- **超时**（下一轮将越过 `--timeout`）：停止轮询，返回 **exit 1**，提示 `等待超时（N 秒），报告状态仍为 ...；可稍后执行 sougeo report status <id> 继续查询`。
- 响应中**缺少 `status` 字段**时：向 stderr 打印 `提示: 响应中未找到 status 字段，已停止轮询`，返回 **exit 0**。
- 业务失败（`code != 200`，如未授权）：**立即返回 exit 3**，不继续轮询。

**Agent 用法建议（创建 → 轮询 → 取链接 完整闭环）：**
```bash
# 1. 创建（异步，仅提交任务）
#    ⚠️ -q 必填：Agent 生成一批供用户点选（品牌 ≤3 / 行业 ≤6），且问题里不含品牌名
sougeo report create -r brand -b 小米 -a company -p 智能手机 \
  -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
  -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
  -l 1 -P 1,3,4,5,6,7,8 -o json   # → 取 report_id

# 2. 轮询等待至终态（建议 --timeout 600，默认 300 常不够）
sougeo report status <report_id> --watch -i 5 --timeout 600 -o json
# → exit 0 且 data.status=="completed"/"opened" → 报告已就绪，继续第 3 步
# → exit 0 且 data.status=="failed"             → 中止并提示用户（建议调整参数重试）
# → exit 1（等待超时）                           → 告知用户稍后 `report status <id>` 续查，勿取链接
# → exit 3（未授权）                             → 引导重新登录

# 3. 仅当状态为 completed/opened 时取链接
sougeo report read <report_id> --type link | grep -oE 'https://[^ ]+' | head -1
```

#### `report share` — 分享管理

**`share start <report-id>`**：
```bash
sougeo report share start <id> --time 3days      # 3天（默认）
sougeo report share start <id> --time week -a    # 7天 + 允许查看 AI 分析师历史
sougeo report share start <id> --time always     # 永久
```
| 参数 | 默认 | 说明 |
|------|------|------|
| `-t, --time` | `3days` | `3days`(3天) \| `week`(7天) \| `always`(永久) |
| `-a, --allow-history` | false | 允许被分享者查看 AI 分析师历史记录 |
| `-o, --output` | `json` | `json` \| `pretty` |

**🧭 引导式分享（Agent 必读）** —— 分享前必须让用户选择**有效期类型**：

| 选项 | `--time` | 说明 |
|------|----------|------|
| **3 天** ⭐ 默认 | `3days` | 短期分享，适合临时查看 |
| **7 天** | `week` | 一周内有效 |
| **永久** | `always` | 长期有效，适合长期交付客户查看（**被分享者仍需登录账号**） |

引导话术：
> 「报告分享链接的**有效期**选哪个？—— **3 天 ⭐（默认）** / 7 天 / 永久。
> 是否同时**允许被分享者查看 AI 分析师历史记录**？（默认不允许）
>
> （说明：分享后**被分享者需登录 SouGEO 账号**才能查看；你也能在网站上**看到有哪些人查看过这份报告，以及查看记录**。）」

流程：
```
1. report list --type brand|industry -o pretty     # 确认要分享的 report_id
2. 询问有效期（3天/7天/永久）与是否允许查看历史
3. 展示确认（报告名 + 有效期 + 是否含历史）
4. report share start <id> --time <3days|week|always> [-a]   # 取回 share_code（见下）
5. 拼装完整分享链接：https://sougeo.com/ReportResults?share_code=<share_code>
6. 把链接 + **有效期** + **「打开需登录」提示** + **「分享后可追踪查看者」说明**一并交付用户
```

> ⚠️ **交付时必须说清两点，避免用户误解**：
> 1. 该分享链接打开后**仍需登录 SouGEO 账号**才能查看，不是免登录公开页；
> 2. **分享后在 SouGEO 网站上可查看「谁看过你的报告」及查看记录**（谁、何时看、看了几次等）——这是分享的正向收益，应一并告知。
>
> 传给用户的标准话术：
> 「分享链接已生成（有效期 X）：<链接>
> 　· 注意：**打开链接的人需要先登录 SouGEO 账号**才能查看报告内容（不是免登录公开页）；
> 　· 另外，**分享后你能在 SouGEO 网站上看到有哪些人查看过这份报告、以及查看记录**，方便你跟进。」

**👀 分享后可见的查看信息（平台侧功能）**

分享链接被访问后，**报告所有者可在 SouGEO 网站（sougeo.com）上查看该报告的查看情况**，包括：

| 可查看信息 | 说明 |
|---|---|
| 查看者身份 | 有哪些人打开过这份分享报告（网站侧明细） |
| 查看记录 | 访问时间、查看时间线等记录（网站侧明细） |
| 查看次数 | ✅ **CLI 可读到**：`report list` 返回体中的 **`view_num`** 字段即为该报告的查看次数 |

> - **查看者明细**在 **SouGEO 网站的报告页/分享管理页**查看，**CLI 无对应命令**（`report share` 只有 `start` / `stop`），因此 **Agent 不要尝试用 CLI 去拉查看者明细**；
> - **但「查看次数」可以读**：`report list -o json` 里每个报告的 `view_num` 就是查看计数，可用于向用户汇报"已被查看 N 次"；
> - 交付分享链接时，**建议主动告知用户这一收益**，让用户理解「分享 ≠ 链接散发出去不可控」，反而**可追踪查看情况**，避免对分享产生误解或顾虑；
> - ⛔ 不要编造具体的查看人数 / 查看者身份——这些明细 Agent 无法从 CLI 获得，只能提示用户到网站查看。

**🔗 分享链接拼装规则（★ 必做）**

分享成功后，取到 `share_code`，按下面模板**拼出完整对外链接**再交付用户——**不要只回一个裸 share_code**：

```
https://sougeo.com/ReportResults?share_code=<share_code>
```

| 项 | 说明 |
|---|---|
| 固定前缀 | `https://sougeo.com/ReportResults?share_code=` |
| `share_code` | 从 `share start` 返回体取；拿不到则执行 `report list --type <brand\|industry> -s -o json`，在返回条目里取 `share.share_code`（建议同时校验 `share.is_current == true`） |
| 有效期 | 由 `--time` 决定：`3days`（3 天）/ `week`（7 天）/ `always`（永久）；过期后链接自动失效 |
| 用途 | **对外二次分享**的完整链接：被分享者打开后**仍需登录 SouGEO 账号**才能查看报告内容（**不是免登录的公开页**）；分享后**可在网站追踪查看者与查看记录**（见下方「👀 分享后可见的查看信息」） |

```bash
# 取 share_code 并拼装完整分享链接
SHARE_URL=$("$CLI" report list --type brand -s -o json \
  | python -c "
import sys,json
d=json.load(sys.stdin)
for r in (d.get('data') or {}).get('data', []):
    sc=(r.get('share') or {}).get('share_code')
    if sc:
        print('https://sougeo.com/ReportResults?share_code='+sc); break
")
echo "$SHARE_URL"
```

> - 与 `report read --type link` 的链接（`/ReportResults/?id=…&reportTypeId=…`）不同：**分享链接走 `share_code`**，适用于把报告分享给他人；**但两者打开后都需要登录 SouGEO 账号**，分享链接并非免登录公开页。
> - `share_code` 为 `null` 说明**尚未分享或分享已失效**，需先 `report share start`。
> - 报告被 `share stop` 撤回后，该链接立即失效——交付时一并告知用户有效期。

> ⚠️ **务必避免用户误解**：`share_code` 链接**不是"任何人点开即看"的公开链接**——
> - 分享确实是**对外可见**的动作，但**被分享者打开后仍需登录 SouGEO 账号**才能看到报告内容；
> - 交付时应主动说明这一点，**不要让用户以为对方免登录就能打开**，否则会产生"链接发过去对方打不开"的误会；
> - **同时告知正向收益**：分享后**可在 SouGEO 网站上看到有哪些人查看过这份报告以及查看记录**（查看者、时间、次数），让用户明白分享是**可追踪**的，而非把链接散发出去不可控；
> - ⛔ 该查看记录**只能在 SouGEO 网站查看，CLI 无对应命令**，不要编造查看数据；
> - `always`（永久）尤其慎用，务必经用户确认。

**`share stop <report-id>`**：
```bash
sougeo report share stop <id>
```
> 撤回分享，已生成的链接立即失效。

---

### 4.3 权益与套餐管理 `rights`

#### `rights list` — 查询账户权益余额
```bash
sougeo rights list              # 表格
sougeo rights list -o json      # JSON
sougeo rights list -p 1 -l 20   # 分页
```
| 参数 | 默认 | 说明 |
|------|------|------|
| `-o, --output` | `pretty` | `json` \| `pretty` |
| `-l, --limit` | `20` | 每页数量 |
| `-p, --page` | `1` | 页码 |

**JSON 返回：**
```jsonc
{ "code": 200,
  "data": { "":"0", "base":"0", "pro":"0", "plus":"0",
            "competitor_base":"0", "agent":"0", "industry_pro":"0" },
  "msg":"success", "request_id":"yoo-api..." }
```
| 字段 | 含义 | 对应报告 |
|------|------|----------|
| `base` | 极速版余额 | 品牌报告 `--report-level 1` |
| `pro` | 专业版余额 | 品牌报告 `--report-level 2` |
| `plus` | 动态分析版余额 | 品牌报告 `--report-level 3` |
| `competitor_base` | 竞品分析基础版 | — |
| `agent` | AI 分析师次数 | — |
| `industry_pro` | 行业版余额 | 行业报告 |
| `""`（空键） | 未分类余额（**服务端原始数据，忽略或按 0 处理**） | — |

表格标签：`极速版 / 专业版 / 动态分析版 / 分析师 / 行业版`。

#### `rights sku list` — 查询可购买套餐
```bash
sougeo rights sku list          # 表格
sougeo rights sku list -o json  # JSON
```
> ⚠️ `rights sku` 是**组命令**，单独执行只打印帮助。查询必须用 `rights sku list`。

**当前套餐（实测，价格随时可变，勿硬编码）：**

| SKU ID | 名称 | 现价(元) | 原价 | `_type` | 升级价 `_grade` |
|--------|------|---------|------|---------|-----------------|
| `geo-rights-base` | 限时极速版 | 19.9 | 99 | `base` | — |
| `geo-rights-pro` | 专业版 | 199 | 199 | `pro` | `base`: 179.1 |
| `geo-rights-plus` | 动态分析版 | 399 | 399 | `plus` | `base`: 379.1, `pro`: 200 |
| `geo-rights-industry-pro` | 搜集星行业版 | 299 | 299 | `industry_pro` | — |

JSON 每条：`{id, sku_id, type, use_type, name, title, product_name, unit, origin_price, now_price, first_price, order, status, extend:{desc, icon, time, _type, _grade, features[], is_event, event_text, highlight}}`

#### `rights pay` — 创建订单并购买
```bash
# AI / 脚本（非交互）
sougeo rights sku list -o json
sougeo rights pay --sku-id geo-rights-pro --grade base --yes -o json

# 人工交互
sougeo rights pay
```
| 参数 | 说明 |
|------|------|
| `-s, --sku-id` | 要购买的套餐 SKU ID（非交互时必填） |
| `-g, --grade` | 升级来源 `base` \| `pro`（仅从旧套餐升级时指定；不传为普通购买） |
| `-y, --yes` | 跳过确认，直接创建订单（脚本必加） |
| `-o, --output` | `json` \| `pretty`（默认 pretty） |

> ⚠️ **外部动作**（创建订单、拉起支付）：先向用户确认套餐与价格再执行。`--grade` 取值须存在于 `sku` 返回的 `extend._grade`。

#### `rights check <order_no>` — 校验订单支付状态
```bash
sougeo rights check <ORDER_NO>
sougeo rights check <ORDER_NO> -o json
```
未支付/不存在时输出 `✗ 订单未支付或不存在`，**exit 3**。

---

### 4.4 版本管理 `version`

```bash
sougeo version show     # 版本号 / 构建时间 / Git提交 / 平台
sougeo version check    # ⚠️ 暂未开放：打印「功能暂未开放，正在建设中」，exit 0
sougeo version update   # ⚠️ 暂未开放：同上
```
> 不要依赖 `version check` 判断新版本，也不要承诺自更新能力。跨平台新二进制需手动分发（见 §2.1 的 8 个平台产物）。

---

## 五、对照表

### 平台 ID 对照表（`report create --platforms`）

| ID | 平台 | ID | 平台 |
|----|------|----|------|
| `1` | 豆包 | `8` | 文心一言 |
| `3` | 腾讯元宝 | `9` | ChatGPT |
| `4` | DeepSeek | `10` | Claude |
| `5` | Kimi | `11` | Gemini |
| `6` | 智谱清言 | `12` | Grok |
| `7` | 千问 | | |

> 多平台逗号分隔，如 `-P 1,3,7`；不指定则由服务端默认覆盖。
> **默认勾选 7 个国内平台** `-P 1,3,4,5,6,7,8`；需覆盖海外时追加 `9,10,11,12`（全选 11 个）。
> ⚠️ **ID 2 不在对照表内，切勿加入**。

### 报告级别 ID 对照表（`report create --report-level`）

| 类别 | ID | 名称 | 对应权益字段 | 生成前是否需校验权益 |
|------|----|------|--------------|---------------------|
| brand | `1` | 极速版 | `base` | ❌ 无需校验 |
| brand | `2` | 专业版 | `pro` | ✅ 需校验 |
| brand | `3` | 动态分析版 | `plus` | ✅ 需校验 |
| industry | `31` | 免费版 | — | ❌ 无需校验 |
| industry | `32` | 专业版 / 行业版 | `industry_pro` | ✅ 需校验 |

### SKU 套餐对照表
见 §4.3 `rights sku list`。

---

## 六、标准工作流（Agent 编排模板）

### 工作流 A：生成一份报告（**引导式，首选**）

> 信息不完整时**不要直接建报告**，按 §4.2「引导式参数收集流程」先收集齐参数。

```
0. 引导收集（§4.2 Step 0~6）：
   - 判定 品牌报告 / 行业报告
   - 品牌报告：确认 企业/个人 → 品牌名 → **产品类型（企业）/ 职业类型（个人）**
     ⭐ 顺序要求：**必须先拿到品牌名，再据该品牌真实产品线/职业生成 3~4 个强相关候选**
        （拿不准就联网查证），**禁止抛通用产品池**（详见 Step 1A ③）
   - 行业报告：识别行业名（识别不到则给候选让用户选）
   - 【关注问题 ★业务必填】🎯 **Agent 默认生成一批「用户视角 + GEO 场景」问题 → 用户点选**：
     ① 抽 3~6 个卖点/痛点关键词  ② 判定/解析地域  ③ 按四类（scenario_best /
        comparative_alternatives / painpoint_solution / local_ranking）拟 **5~7 条**（行业 **8~10 条**）
     ④ 摆给用户**点选序号**（预置推荐项）  ⑤ 按上限落地：**品牌 ≤3 条、行业 ≤6 条**
     且 **问题不得含被测品牌名/产品名**（否则 GEO 可见度测试失去意义）
   - 【地域解析】用户给的地域若是**模糊词**（"本地""我们这边"）：
     上下文推断 → **IP 归属地联网兜底** → 仍不确定则**问用户**；
     **严禁把模糊词原样写入问题、严禁编造地名**（详见 Step 1A ④「模糊地域解析」）
   - AI 平台：默认 7 个国内平台（可追加海外 4 个）
   - 报告类型：默认极速版
0.5 【前置】CLI 可用性检查：`command -v sougeo || command -v SouGEO`
    未安装 → `npm i -g @yooai/sougeo-cli`（§2.0；安装属环境变更，先告知用户）
1. auth status -o json                        # logged_in==false → 引导 auth login / auth token
2. 白名单预校验（§4.2：-r/-a/-l/-P 取值域；CLI 亦会本地拦截）
   - 额外自查：`-q` 数量（品牌 ≤3 / 行业 ≤6）、非空、不含品牌名、含决策引导词、尽量带卖点词/地域
3. 【权益校验】若 -l 非极速版/免费版：
   rights list -o json → 按类型检查 pro / plus / industry_pro 余额
   - 余额 > 0 → 继续
   - 余额 = 0 → ⛔ 中止，引导开通权益（工作流 C），开通后继续
4. 展示「参数确认单」→ 用户确认（关注问题必须列全且非空）
5. 【创建】sougeo report create ... -o json
   → 从返回体提取 report_id（优先 data.report_id，其次 data.id）
   → 若未回传 id：report list --type brand|industry -p 1 -l 5 -o json 取第一条（最新）的 report_id
6. 【轮询状态 ★必做】sougeo report status <report_id> --watch -i 5 --timeout 600 -o json
   - exit 0 且 data.status=="completed"  → 报告就绪，进入第 7 步
   - exit 0 且 data.status=="opened"     → 报告就绪（已公开终态），进入第 7 步
   - exit 0 且 data.status=="failed"     → ⛔ 中止并提示用户（建议调整参数重试），**不取链接**
   - exit 1（等待超时）                   → ⏳ 告知用户仍在生成中，稍后 `report status <id>` 续查，**不取链接**
   - exit 3（如未授权）                   → 🔒 引导重新登录后再查
7. 【取链接 ★必做】确认 completed/opened 后：
   report read <report_id> --type link | grep -oE 'https://[^ ]+' | head -1
8. 【交付 ★默认形态】把链接交给用户，并给**基础小结**：
   - 品牌报告：品牌指数 / 报告类型 / 覆盖平台 / 测试问题 / 生成时间
     ⛔ **不含"行业归属"**（该字段属行业报告）
   - 行业报告：报告名称 / 行业归属 / 报告类型 / 覆盖平台 / 测试问题 / 生成时间
     （素材优先取 `report status` 的 data，字段不全再查一次 `report list` 定位该条目）
   - 结尾引导"点击链接查看完整报告"
   - ⛔ **默认不执行 `report read --type json`**；仅当用户明确索要完整数据时才调用
```

> ⚠️ **顺序不可颠倒**：`report create` 是异步的，返回后报告**还没生成完**；非终态时 `report read --type link` 会失败或拿到空内容。**必须先轮询到 `completed`/`opened`，再取链接。**

> ⚠️ **`-q` 不可为空**：这是 GEO 测试题，空了整份报告就没有测试意义。CLI 不校验，**Agent 必须自己拦住**。

**命令示例（关注问题体现"四类发散 + 咬合卖点"）：**
```bash
# 企业品牌（默认国内 7 平台）；三问分别覆盖 抉择 / 场景 / 痛点
sougeo report create -r brand -b 小米 -a company -p 智能手机 \
  -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
  -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
  -q "续航久、充电快的安卓手机怎么选？" -l 1 -P 1,3,4,5,6,7,8
# 个人品牌（-p 职业类型，候选须据该人名/身份生成）
sougeo report create -r brand -b 罗永浩 -a personal -p 带货主播 \
  -q "想找靠谱主播带货，选哪个？" -q "中小商家找主播带货怎么避坑？" -l 1 -P 1,3,4,5,6,7,8
# 行业（含地域时补一条 local_ranking）
sougeo report create -r industry -i 新能源汽车 -s "家用纯电" \
  -q "家充方便、通勤代步的新能源车，推荐哪个？" \
  -q "冬天续航虚标的问题，有没有靠谱的应对选择？" \
  -q "成都买新能源车，哪家门店售后口碑最好？" -l 31 -P 1,3,4,5,6,7,8
# 需覆盖海外：在 -P 末尾追加 ,9,10,11,12
```

### 工作流 B：分享报告给客户（**需让用户选有效期 + 拼装分享链接**）
```
1. report list --type brand|industry -o pretty   # 确认要分享的 report_id
2. 询问有效期：3 天（默认 ⭐）/ 7 天 / 永久；是否允许查看 AI 分析师历史
3. 展示确认（报告名 + 有效期 + 是否含历史）
4. report share start <report_id> --time <3days|week|always> [-a]   # 取回 share_code
5. 取 share_code（返回体没有则 report list --type <t> -s -o json，
   取 share.share_code 且 is_current==true）
6. ★ 拼装完整分享链接并交付：
   https://sougeo.com/ReportResults?share_code=<share_code>
   交付时一并告知 ① 有效期 ② **「打开链接的人需登录 SouGEO 账号才能查看」**
   ③ **「分享后可在网站看到谁查看过该报告及查看记录」**
7. （撤回）report share stop <report_id>   # 撤回后链接立即失效
```

### 工作流 C：购买权益 / 升级套餐
```
1. rights list -o json                        # 先确认缺哪个权益（pro/plus/industry_pro）
2. rights sku list -o json                    # 展示对应套餐与实时价格 → 用户确认
3. rights pay --sku-id <SKU_ID> [-g base|pro] --yes -o json
4. 用户支付后：rights check <order_no> -o json
5. rights list -o json                        # 复核余额，通过后回到工作流 A 继续生成
```

### 工作流 D：兜底检索与登录态判断
```
登录态：auth status -o json → json.loads → d["logged_in"]
找报告：report list --type brand|industry|shield -o pretty | 匹配名称 → 取 id
查状态：report status <id> -o json → d["data"]["status"]
        （或 --watch 等待终态）
取链接：状态为 completed/opened 后 → report read <id> --type link
已分享：report list --type brand -s -o json | 过滤 share.share_code != null
分享链接：https://sougeo.com/ReportResults?share_code=<share.share_code>
          （⚠️ 打开后仍需登录 SouGEO 账号才能查看，非免登录公开页）
翻页：以 data.count 与 -l 计算页数，循环 -p 1..N
```

---

## 七、数据结构速览

> 以下为 `report read --type json` 的完整报告结构。**该接口默认不调用**（交付默认只给 `--type link` + 基础小结，见 §4.2 Step 7）；仅当用户明确索要完整数据时参考。

**报告数据（`report read --type json`）结构：**

- **品牌报告**（`--report-type brand`）：`{ advance, base, domain-chart, opinion-analysis }`
  - `base` — 基础信息、品牌指数、竞品数据、平台覆盖
  - `advance` — AI 感知、平台曝光、平台可见度/排名/趋势、口碑
  - `domain-chart` — 域名/流量/热门引用来源
  - `opinion-analysis` — 舆情分析
- **行业报告**（`--report-type industry`）：`{ allPlatform, base, brandSummary, overview, report, suggestions }`
  - `overview` — 行业概览、健康度、关注问题
  - `base.metrics` — 核心指标总览（平均品牌指数、SOV、可见度、情感、引用率…）
  - `allPlatform` — 各 AI 平台 GEO 指数纵览、权威性、EEAT、流量来源
  - `suggestions` — 行业机会 / 问题 / 风险

> **完整字段字典见 `references/report-schema.md`。**

---

## 八、错误处理与注意事项

| 现象 | 退出码 | 原因 | 处理 |
|------|--------|------|------|
| `command not found: sougeo` | 127 | **CLI 未安装**（或全局 bin 不在 PATH） | `npm i -g @yooai/sougeo-cli --registry=https://registry.npmjs.org`（§2.0）；已装则检查 `npm prefix -g` 是否在 PATH。**Linux/macOS 上还可先试大写 `SouGEO`** |
| npm 报 `Unknown cli config "--g@yooai/..."` | — | **`-g` 与包名之间漏了空格** | 改成 `npm install -g @yooai/sougeo-cli`（带空格），否则实际未安装 |
| npm 报 `404 ... is not in this registry` | — | 淘宝镜像（npmmirror）未同步该新包 | 加 `--registry=https://registry.npmjs.org` 用官方源重装 |
| npm 安装时 `postinstall` 报错 | — | 内网/代理无法下载平台二进制 | 改用 §2.1 手动下载对应平台产物，以绝对路径调用 |
| **stderr** 有 `请通过 …`（含 `说明：`/`用法示例：` 段），**stdout 为空** | **0** | **引导性提示**：缺必填参数（`--type`/`--report-type`/`--brand`/`--industry`/`report-id`） | **不是成功**！读 stderr 补齐参数重试 |
| `参数错误：...` | 1 | 未知命令/子命令、未知 flag、非法取值（`-r bogus`/`-l` 不匹配/`-P` 不在表/`-p≤0`/`-o` 非法）、`--config` 缺失 | 修正参数；CLI 本地拦截，**不会创建报告** |
| `参数错误：等待报告生成超时 …` | 1 | `report status --watch` 超时 | 稍后 `report status <id>` 再查，或加大 `--timeout` |
| `请求失败：...` | 2 | **网络错误**（连接失败/HTTP 错误/解析失败） | 检查网络与后端地址；重试 |
| `{"code":10003,"msg":"用户未授权"}` | **3** | **登录凭证无效/已过期** | `auth status -o json` 确认 → `auth login` / `auth token` 重新登录 |
| `code:400` `报告不存在` | 3 | report_id 无效或不属于本账号 | `report list` 重新获取 ID |
| `code:500` `任务失败` | 3 | 报告生成失败/类型不支持 | `report status` 查状态；必要时重新创建 |
| `✗ 订单未支付或不存在` | 3 | 订单未支付/无效 | 完成支付或核对订单号 |
| 「功能暂未开放，正在建设中」 | 0 | `version check` / `version update` 未开放 | 忽略，勿依赖 |
| stderr: `提示: 响应中未找到 status 字段，已停止轮询` | 0 | `report status --watch` 响应无 `status` | 属异常数据，改为单次查询确认 |
| stdout 前有 `[配置] ...` 行 | — | 加了 `-d/--debug` | 去掉 `-d`，或从首个 `{` 截取 |
| stderr: `⚠ 未知子命令 \`x\`` | 0 | `组命令 未知子命令 -h`（如 `rights sku pay -h`） | 用 `--help` 确认可用子命令 |
| 未传 `-q` 却创建成功 | 0 | **CLI 不校验关注问题必填** | 🔥 报告缺少 GEO 测试题、失去意义；**Agent 必须自己拦截**（品牌 ≤3 / 行业 ≤6、非空、不含品牌名、含引导词/卖点词） |
| `report status` 出现 `base_completed` | — | **中间态**（基础数据就绪、其余仍在生成） | ⛔ **非终态**，继续轮询到 `completed`/`opened` 再取链接；终态用白名单 `{completed, opened, failed}` 判定 |
| `report read <id> --type link` 返回空 / 报错 | — | **报告尚未完成**就取链接 | 先 `report status <id> --watch` 到 `completed`/`opened` 再取 |
| `rights list` 对应字段为 `"0"` | — | **不具备该档位权益** | ⛔ 中止生成，引导开通权益（§4.2 Step 4 ④） |
| `rights list` JSON 出现空键 `""` | — | 服务端原始数据 | 忽略或按 0 处理 |
| 品牌报告小结里出现「行业归属」 | — | **字段归属搞混** | ⛔ 删除：`行业归属/industry_name` **仅行业报告有**；品牌报告小结只用 品牌指数/报告类型/覆盖平台/测试问题/生成时间 |
| 关注问题里出现"本地""我们这边"等模糊地名 | — | **地域未解析** | 按「模糊地域解析」（上下文 → IP 归属地 → 用户确认）换成**明确地名**，否则该问题无效 |
| JSON 解析失败 | — | `-d` 模式混入日志行 | 从首个 `{` 截取（兼容写法） |
| 表格列错位 | — | CLI 按字符数而非显示宽度补位 | 用 `-o json` 解析 |

**其他注意事项：**
- **⭐ 使用前先确认 CLI 已安装**：`npm i -g @yooai/sougeo-cli`（命令名 `sougeo`；Linux/macOS 若小写找不到则试 `SouGEO`）。安装/升级属**环境变更**，执行前告知用户（§2.0）。
- `report create` **异步 + 消耗权益**；状态机：`pending` → `processing` → `completed` / `failed`（`opened` 亦为终态）。
- **`-q` 必须由用户点选确认**：Agent **默认生成一批「用户视角 + GEO 场景」的问题摆给用户**（不要只给空模板），按「场景 / 抉择 / 痛点 / 地域」四类拟候选；**上限品牌 ≤3 条、行业 ≤6 条**。**问题里不得出现被测品牌名/产品名**，且**应含决策引导词、植入卖点关键词**（GEO 测试的关键前提，见 Step 1A ④）。
- **`-p` 产品/职业候选必须后置生成**：**先拿到品牌名**，再据该品牌真实产品线/职业身份给出 3~4 个强相关候选（拿不准就联网查证），**不要抛与品牌无关的通用候选池**（见 Step 1A ③）。
- **生成闭环三步不可颠倒**：① `report create` 取 `report_id` → ② `report status <id> --watch` 轮询到终态 → ③ **确认 `completed`/`opened` 后**才 `report read <id> --type link` 取链接。
- **默认交付 = 基础小结 + 链接**：品牌报告给出品牌指数等小结并引导用户点链接；**默认不调用 `report read --type json`**，仅用户明确索要完整数据时才用。
- **⛔ 字段归属别搞混**：品牌报告小结**不含「行业归属」**（`industry_name` 只属行业报告）；**「品牌指数」只属品牌报告**。不要因为 `report list` 返回体里带 `industry_name` 就把它写进品牌报告小结。
- **⛔ 地域必须明确**：关注问题里的地名须是**明确地名**；用户给"本地/我们这边"等模糊词时按「上下文 → IP 归属地联网兜底 → 用户确认」解析，**不得原样使用或编造**；解析结果在确认单里请用户核对。
- **创建后请用 `report status <id> --watch` 等待**，不要手工翻列表；`--timeout` 默认 300s，长报告建议调到 600s 以上；超时（exit 1）只是"还没完"，**可重复执行续查**，不消耗权益。
- **失败（`failed`）或超时（exit 1）时不要取链接**，先告知用户真实状态。
- **非极速版（专业版/动态分析版/行业版）提交前必须先校验权益**，不足则引导开通（§4.2 Step 4）。
- 报告有效期 `agent_end_at`（AI 分析师可用截止时间）。
- **环境由配置决定**（§2.2）：`~/.SouGEO/config.yaml` 存在时其 `api.url` 决定测试/生产。生产环境操作（创建/购买）务必谨慎。
- 凭证文件 `credentials.json` 为**明文**，不要把内容写入日志/报告；`auth logout` 会删除它。
- 不要硬编码价格/套餐，以 `rights sku list` 实时为准。
- **分享链接要拼完整**：拿到 `share_code` 后交付 `https://sougeo.com/ReportResults?share_code=<share_code>`，**不要只给裸 code**；有效期由用户选择（`3days`/`week`/`always`），`always` 慎用；`share stop` 撤回后立即失效。
- **⚠️ 分享链接打开后仍需登录 SouGEO 账号才能查看**（不是免登录公开页）——交付时必须主动告知用户，避免其误以为对方免登录即可打开。
- **⭐ 分享后可在 SouGEO 网站看到「有哪些人查看过该报告及查看记录」**（查看者/时间/次数）——交付时应一并说明这一收益，避免用户误解分享不可控；该记录**只能在网站查看，CLI 无对应命令**，不要编造查看数据。
- 不要擅自执行 `version update`；`rights pay` / `auth logout` / `report share start` 属外部或破坏性动作，需用户确认。

---

## 九、快速参考（复制即用）

```bash
# 0. 安装 / 升级 CLI（首次使用或需要最新版）
npm i -g @yooai/sougeo-cli          # 安装；升级用 @latest
command -v sougeo || command -v SouGEO   # 校验可用（Linux/macOS 若小写找不到，试大写 SouGEO）

CLI="${SOUGEO_CLI:-sougeo}"         # 已全局安装则直接用命令名；也可指向本地二进制绝对路径

# 登录态（JSON，脚本友好）
"$CLI" auth status -o json
# Token 登录
"$CLI" auth token -t <TOKEN> -o json

# 生成「企业品牌」报告（默认国内 7 平台）
# ⚠️ -q 必填：用户从候选清单点选（品牌 ≤3 / 行业 ≤6）；问题里【不含品牌名】、含引导词+卖点词
# ⚠️ -p 产品类型：先拿品牌名，再据其产品线生成候选（智能手机 来自小米产品线）
"$CLI" report create -r brand -b 小米 -a company -p 智能手机 \
  -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
  -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
  -q "续航久、充电快的安卓手机怎么选？" -l 1 -P 1,3,4,5,6,7,8

# 生成「个人品牌」报告（-b 个人品牌名，-p 职业类型，职业候选据人名/身份生成）
"$CLI" report create -r brand -b 罗永浩 -a personal -p 带货主播 \
  -q "想找靠谱主播带货，选哪个？" -q "中小商家找主播带货怎么避坑？" -l 1 -P 1,3,4,5,6,7,8

# 生成「行业」报告（有地域时补一条带地名的 local_ranking）
"$CLI" report create -r industry -i 新能源汽车 -s "家用纯电" \
  -q "家充方便、通勤代步的新能源车，推荐哪个？" \
  -q "冬天续航虚标的问题，有没有靠谱的应对选择？" \
  -q "成都买新能源车，哪家门店售后口碑最好？" -l 31 -P 1,3,4,5,6,7,8

# 需覆盖海外 → 在 -P 末尾追加 ,9,10,11,12（11 个全选）

# ★ 生成闭环：创建 → 轮询状态 → 取报告链接（推荐一次性执行）
RID=$("$CLI" report create -r brand -b 小米 -a company -p 智能手机 \
        -q "除了几个大牌，还有哪些性价比高的手机值得考虑？" \
        -q "适合学生党、预算 3000 以内的手机推荐哪款？" \
        -l 1 -P 1,3,4,5,6,7,8 -o json \
      | python -c "import sys,json;d=json.load(sys.stdin);a=d.get('data') or {};print(a.get('report_id') or a.get('id') or '')")
"$CLI" report status "$RID" --watch -i 5 --timeout 600 -o json   # 轮询至 completed / opened
"$CLI" report read "$RID" --type link | grep -oE 'https://[^ ]+' | head -1   # 状态就绪后取链接 ← 默认交付
# → 交付：基础小结（品牌指数/报告类型/覆盖平台/测试问题/生成时间；⛔品牌报告不含"行业归属"）+ 上述链接

# 列表（分页，含 shield 星盾报告）
"$CLI" report list --type brand -p 1 -l 10 -o json
"$CLI" report list --type shield -o json
"$CLI" report read <REPORT_ID> --type link      # ★ 默认输出：链接字符串（须先确认状态为 completed/opened）
"$CLI" report read <REPORT_ID> --type json      # ⛔ 仅当用户明确索要完整数据时才用（默认不调用）

# 查询生成状态（★ 等待报告完成的标准做法）
"$CLI" report status <REPORT_ID> -o json                       # 单次查询
"$CLI" report status <REPORT_ID> --watch --timeout 600 -o json # 轮询至终态

# 分享（先让用户选有效期：3days ⭐ / week / always）
"$CLI" report share start <REPORT_ID> --time 3days
"$CLI" report list --type brand -s -o json
# ★ 拼装分享链接（取 share.share_code 拼接后交付用户）
#   交付时告知：① 有效期 ② 打开需登录 SouGEO 账号 ③ 分享后可在网站看到查看者及查看记录
SHARE_URL=$("$CLI" report list --type brand -s -o json \
  | python -c "import sys,json;d=json.load(sys.stdin);
for r in (d.get('data') or {}).get('data',[]):
    sc=(r.get('share') or {}).get('share_code')
    if sc: print('https://sougeo.com/ReportResults?share_code='+sc); break")
echo "$SHARE_URL"   # ⚠️ 打开后仍需登录 SouGEO 账号；查看记录需在网站查看（CLI 无此命令）
"$CLI" report share stop  <REPORT_ID>   # 撤回后链接立即失效

# 权益校验与开通
"$CLI" rights list -o json
"$CLI" rights sku list -o json
"$CLI" rights pay --sku-id geo-rights-pro --grade base --yes -o json
"$CLI" rights check <ORDER_NO> -o json

# 版本
"$CLI" version show
```
