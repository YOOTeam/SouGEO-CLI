# 搜极星报告数据结构字典

> **前置**：CLI 需先安装 → `npm i -g @yooai/sougeo-cli`（命令名 `sougeo`；详见 SKILL.md §2.0）。
>
> 本文档描述 `sougeo report read <report-id> --type json` 返回的完整报告数据结构。
> 顶层统一为 `{ "code": 200, "msg": "success", "data": <报告对象> }`（读取失败时 `code` 为 400/500 且 `data` 为 null；未登录为 `{"code":10003,"msg":"用户未授权"}`）。
> 品牌报告与行业报告结构**完全不同**，按创建时的 `--report-type brand|industry` 区分。
>
> ⚠️ **调用时机**：`--type json` 是**按需**接口（数据量大）。**默认交付形态是 `--type link` + 一段基础小结**（见 SKILL.md §4.2 Step 7 ③）；仅当用户明确索要完整数据/明细指标时才读取本结构。

---

## 一、品牌报告结构（`--report-type brand`）

顶层四个区块：`advance`、`base`、`domain-chart`、`opinion-analysis`。

> ⛔ **字段归属**：品牌报告**不含"行业归属"**（`industry_name` 仅行业报告有），其核心指标是 **`base.brand_index`（品牌指数）**。
> 品牌报告的基础小结只展示：品牌指数 / 报告类型 / 覆盖平台 / 测试问题 / 生成时间（见 SKILL.md §4.2 Step 7 ③）。

```
{
  "advance":          AI 深度分析（AI 感知 / 平台曝光 / 平台指标 / 品牌指数 / 竞品 / 口碑）
  "base":             基础信息与图表数据（含品牌指数、竞品、平台覆盖）
  "domain-chart":     域名与流量图表（热门引用来源、行业流量）
  "opinion-analysis": 舆情分析
}
```

### 1.1 `base` — 基础信息与图表

| 路径 | 类型 | 说明 |
|------|------|------|
| `base.open_agent` | bool | 是否开通 AI 分析师 |
| `base.problems` | string[] | 本次分析关注的**问题列表** |
| `base.has_platform[]` | object[] | 覆盖的 AI 平台：`{have_ai, icon, is_overseas, platform_id, platform_name}` |
| `base.ai_platform_char[]` | object[] | 各平台图表：`{name, value}` |
| `base.base` | object | **报告基础信息**（见下） |
| `base.brand_index` | object | **品牌指数**（见下） |
| `base.competitor.competitor_data[]` | object[] | **竞品数据**（见下） |
| `base.competitors_bubble_char[]` | object[] | 竞品气泡图：`{name, data:[visibility, reference_ratio, name]}` |
| `base.ai_platforms.platform_data[]` | object[] | 各平台指标（见下） |
| `base.ai_platforms.ai_platforms_visibility[]` | object[] | 平台可见度开关：`{icon, is_overseas, platform_id, platform_name, visibility}` |

**`base.base`（报告元信息）：**

| 字段 | 类型 | 示例 | 说明 |
|------|------|------|------|
| `anchor` | string | `"基础信息"` | 区块标题 |
| `report_name` | string | `"ChatPPT AIPPT Platform Analysis Report"` | 报告名 |
| `brand_name` | string | `"chatppt"` | 品牌名（个人品牌报告为个人品牌名） |
| `brand_type` | string | `"company"` | 品牌类型 `company`（企业）/ `personal`（个人） |
| `product_name` | string | `"AIPPT"` | 企业品牌为产品名/产品类型；个人品牌为**职业类型** |
| `report_type_id` | int | `2` | 报告类型 ID |
| `type_name` | string | `"专业版"` | 类型中文名 |
| `status` | string | `"completed"` | 状态 |
| `progress` | int | `100` | 进度 |
| `banner_url` | string | `"https://..."` | 封面图 |
| `completed_at` | string | `"2026-09-16 18:44:46"` | 完成时间 |
| `close_at` | string/null | `null` | 关闭时间 |
| `authenticity` / `auth_reason` | null | — | 真实性标记（预留） |

**`base.brand_index`（品牌指数）：**

| 字段 | 说明 |
|------|------|
| `brand_index` | 品牌指数总分（字符串数字，如 `"0.00"`） |
| `ranking` | 排名分 |
| `reference` | 引用分 |
| `visibility` | 可见度分 |
| `brand_keywords[]` | 品牌关键词数组 |

**`base.competitor.competitor_data[]`（竞品）：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `name` | string | 竞品品牌名 |
| `self` | bool | 是否为本品牌 |
| `brand_index` | float | 竞品品牌指数 |
| `avg_rank` | float | 平均排名 |
| `reference_ratio` | float | 引用率 |
| `visibility` | float | 可见度 |
| `keyword[]` | string[] | 竞品命中的关键词 |

**`base.ai_platforms.platform_data[]`（各平台指标）：**

| 字段 | 类型 | 说明 |
|------|------|------|
| `platform_id` / `platform_name` | int / string | 平台 ID / 名称 |
| `icon` / `is_overseas` | string / bool | 图标 / 是否海外平台 |
| `brand_index` | float | 该平台品牌指数 |
| `visibility` | float | 可见度 |
| `rank` / `avg_rank` | float | 排名 / 平均排名 |
| `reference_ratio` | float | 引用率 |
| `reference_ids[]` | string[] | 引用来源 ID |
| `total` | int | 采样总数 |

### 1.2 `advance` — AI 深度分析

| 路径 | 类型 | 说明 |
|------|------|------|
| `advance.status` | string | 分析状态 |
| `advance.ai_perception.anchor` | string | `"AI感知"` |
| `advance.ai_perception.data[]` | string[] | AI 对品牌的**感知标签**（如 `time-saving`、`professional`） |
| `advance.ai_platform_exposure[]` | object[] | 平台曝光量：`{platform, value:[{name, value}]}` |
| `advance.ai_platform_exposure_analyse` | object | 曝光分析（见下方通用形态） |
| `advance.ai_platforms` | object | 平台综合：`anchor` + 4 个分析子块（见下） |
| `advance.brand_index.brand_index_review` | object | 品牌指数复盘分析 |
| `advance.competitor.competitor_analysis` | object | 竞品对比分析 |
| `advance.user_reputation` | object | 用户口碑（见下） |

**`advance.ai_platforms` 子块：**
`platform_perception`（平台感知）、`platform_rank`（平台排名）、`platform_trend`（平台趋势）、`platform_visibility`（平台可见度），每个都是通用分析形态。

**通用分析形态（`*_analyse` / `*_analysis` / `*_review` 等）：**
```jsonc
{
  "conclusions": [ { "keyDimension": "核心维度", "keyPoint": "结论要点" } ],
  "keyMessage":  [ "关键信息1", "关键信息2" ],
  "resultText":  "完整分析正文（长文本）"
}
```

**`advance.user_reputation`（用户口碑）：**
| 字段 | 类型 | 说明 |
|------|------|------|
| `anchor` | string | `"用户评价"` |
| `goods[]` | string[] | 正面评价点 |
| `bad[]` | string[] | 负面评价点 |
| `positive_knowledge_ratio` | string | 正面知识占比 |
| `evaluate` | object | 评价分析（通用分析形态） |

### 1.3 `domain-chart` — 域名与流量图表

| 路径 | 类型 | 说明 |
|------|------|------|
| `domain_char[]` | array | 域名图表（可能为空） |
| `hot_reference_char[]` | object[] | **热门引用来源**：`{has_brand_name, platform, site_name, url, value}` |
| `industry_traffic[]` | object[] | 行业流量分布：`{name, value}` |
| `industry_traffic_analyse` | object | 行业流量分析（通用分析形态） |
| `traffic_distribution_analyse` | object | 流量分布分析（通用分析形态） |

### 1.4 `opinion-analysis` — 舆情分析

| 字段 | 类型 | 说明 |
|------|------|------|
| `status` | string | 状态 |
| `data` | object | 舆情数据（见下） |

**`opinion-analysis.data`：**

| 字段 | 说明 |
|------|------|
| `analysisDate` | 分析日期 |
| `brand` | 品牌名 |
| `brandCurrentStatus` | **品牌现状**：`competitiveLandscape`（竞争格局）、`introduction`（品牌介绍）、`marketPosition`（市场地位）、`searchTrend`（搜索趋势）、`technology`（技术） |
| `brandOperationActionGuide` | **品牌行动指南**（运营建议） |
| （可能含更多舆情子块） | |

---

## 二、行业报告结构（`--report-type industry`）

顶层六个区块：`allPlatform`、`base`、`brandSummary`、`overview`、`report`、`suggestions`。

```
{
  "overview":     行业概览
  "base":         核心指标 + 关键发现
  "allPlatform":  各 AI 平台 GEO 指数与信源分析（最丰富）
  "brandSummary": 行业内品牌概览
  "suggestions":  行业机会 / 问题 / 风险
  "report":       报告元信息与平台列表
}
```

### 2.1 `overview` — 行业概览

| 字段 | 示例 | 说明 |
|------|------|------|
| `name` | `"新能源汽车品牌认知分析报告"` | 报告名 |
| `industry_name` | `"新能源汽车"` | 行业名 |
| `focusPoint` | `"新能源汽车"` | 焦点主题 |
| `date` | `"2026-09-01T22:42:48Z"` | 日期 |
| `banner` | `"https://..."` | 封面图 |
| `goodBrands` | `"19/54"` | 良好品牌数 / 总数 |
| `industryFocusRate` | `"2.28"` | 行业关注度 |
| `industryHealth` | `"27.98"` | 行业健康度 |
| `problems[]` | string[] | 行业关注问题列表 |

### 2.2 `base` — 核心指标与关键发现

- `base.keyFind` = `{ anchor:"关键发现", data:[ "发现1", "发现2", ... ] }`
- `base.metrics` = `{ anchor:"核心指标总览", data:{...} }`

**`base.metrics.data`：**

| 字段 | 示例 | 说明 |
|------|------|------|
| `aiIndustrySummary` | `"行业处于'高可见、低引用'..."` | AI 行业总述 |
| `avgBrandIndex` | `"35.91"` | 平均品牌指数 |
| `avgVisibility` | `"20.37"` | 平均可见度 |
| `avgSov` | `"4.33"` | 平均声量份额 SOV |
| `avgSentiment` | `"56.05"` | 平均情感值 |
| `avgReferenceRate` | `"7.71"` | 平均引用率 |
| `authorityPreferenceIndex` | `"37.54"` | 权威偏好指数 |
| `citationDiversityIndex` | `"1.19"` | 引用多样性指数 |
| `top3SOV` | `"57.00"` | 前三品牌 SOV 合计 |
| `top5Brand[]` | object[] | Top5 品牌：`{brand_index, brand_name, icon}` |

### 2.3 `allPlatform` — 各 AI 平台分析（16 个子块）

每个子块统一为 `{ anchor, data }` 形态；`data` 多为数组或对象。

| 键 | data 类型 | 关键字段 | 含义 |
|----|-----------|----------|------|
| `geoIndex` | object[] | `{platform_name, data:{avgRank, brand_index, citationRatio, sentiment, sov, visibility}}` | **各平台 GEO 指数纵览** |
| `platformsAvgIndex` | object | `{citationDiversityIndex, freshnessRate, reference_ratio}` | 平台平均指标 |
| `platformsBrand` | object[] | `{avgRank, avgResponseWords, brandNum, brand_index, citationCountPerAnswer, citationDiversityIndex, citationRatio, freshnessRate, longTailBrandCount, platform_name, ...}` | 各平台品牌表现 |
| `platformsPlatform` | object[] | `{BrandCount, brandNum, longTailBrandCount, platform_name, top3Brand:[{brand_index,name}]}` | 各平台竞争集中度 |
| `platformsStyle` | object[] | `{data:{avgResponseWords, structurePreference:[], ...}, platform_name}` | 各平台内容风格偏好 |
| `platformsAnalyse` | string | 竞争格局定性长文本（CR3/CR10 等） | 平台综合分析 |
| `authority` | object[] | `{authoritative_conclusion, avg_authority, high_ratio, low_ratio, medium_ratio, ...}` | 信源权威性分布 |
| `eeat` | object[] | `{platform_name, linkContent:{EEAT:{authorityScore, factualAccuracyScore, objectivityScore, structuredScore, verifiabilityScore}, index}}` | EEAT 评估 |
| `freshnessDistribution` | object[] | `{platform_name, citationRatio, freshnessRate, sov}` | 内容新鲜度分布 |
| `headSite` | object[] | `{data:[{name, ratio, url}], platform_name}` | 头部信源站点 |
| `highFeature` | object[] | `{data:[{avgWordCount, containsDataChart, keyEntitiesIncluded:[], name, structure, url}]}` | 高特征内容 |
| `referBrandDirtribution` | object[] | `{platform_name, ratio:[{name, ratio}]}` | 品牌引用分布 |
| `referenceStyle` | object[] | `{allRatio:[{name,ratio}], firstRatio, conclusion}` | 引用内容形态偏好 |
| `topBrand` | object | `{avg:{referenceRatio, sov, visibility}, index:[{brandPreference:{品牌:{icon,referenceRatio,sov,visibility}}}]}` | 头部品牌指标 |
| `trafficAnalyse` | string | 流量格局分析长文本 | 流量综合分析 |
| `trafficSource` | object[] | `{platform_name, referBrandDirtribution:[{domain, name, ratio}]}` | 流量来源渠道 |

### 2.4 `brandSummary` — 行业品牌概览

| 键 | 结构 | 说明 |
|----|------|------|
| `industryBrand` | `{anchor, data}` | 行业品牌列表 |
| `importantIndustryBrand` | `{anchor, data}` | 重要行业品牌 |
| `picture` | `{anchor, data}` | 品牌图谱/图片数据 |

### 2.5 `suggestions` — 建议

| 键 | 结构 | 说明 |
|----|------|------|
| `industry_opportunity` | `{anchor, data[]}` | **行业机会** |
| `issue` | `{anchor, data[]}` | **问题** |
| `risk` | `{anchor, data[]}` | **风险** |

### 2.6 `report` — 报告元信息

| 字段 | 说明 |
|------|------|
| `status` | 状态 |
| `progress` | 进度 |
| `report_type_id` | 报告类型 ID |
| `open_agent` | 是否开通 AI 分析师 |
| `ai_platform[]` | 平台列表：`{hasAi, icon, is_overseas, platform_id, platform_name}` |

---

## 三、通用术语与指标

| 术语 | 英文/字段 | 含义 |
|------|-----------|------|
| 品牌指数 | `brand_index` | 品牌在 AI 生态中的综合表现分 |
| 可见度 | `visibility` | 品牌被 AI 回答提及的程度 |
| 引用率 | `reference_ratio` / `citationRatio` | AI 回答中引用该品牌的比率 |
| 排名 | `rank` / `avg_rank` | 品牌在回答中的平均排序 |
| 声量份额 | `sov` | Share of Voice，品牌提及占比 |
| 情感值 | `sentiment` | 舆情的正面/中性/负面倾向分值 |
| 新鲜度 | `freshnessRate` | 引用内容的时效性 |
| 引用多样性 | `citationDiversityIndex` | 引用来源的分散程度 |
| EEAT | `eeat` | Experience/Expertise/Authoritativeness/Trust 信源质量 |
| 长尾品牌 | `longTailBrandCount` | 非头部品牌占比 |
| SOV | `sov` | 声量份额 |

## 四、解析提示

- 所有数值多为**字符串化数字**（如 `"35.91"`、`"0.00"`），聚合计算前需 `float()` 转换。
- 空数据分析块的 `conclusions[].keyPoint` 会写明"无数据/无法分析"（如 `traffic_distribution_analyse`）。
- 部分字段在特定报告下为 `null`（如竞品为空时 `competitor_data` 为空数组）。
- 长文本分析统一放在 `resultText` / 直接字符串（如 `platformsAnalyse`、`trafficAnalyse`）。

---

## 五、非报告接口的返回结构

以下接口的返回结构不属于「报告数据」本体，单独列出以免混淆。

### 5.1 `report list` — 报告列表

```jsonc
{
  "code": 200, "msg": "success",
  "data": {
    "count": 175,         // 总数（用于翻页边界判断）
    "data": [ /* 报告条目数组，字段见 SKILL.md §4.2 */ ]
  }
}
```
> `--type` 支持 `brand`（品牌报告）/ `industry`（行业报告）/ `shield`（星盾报告）。
> 报告状态机：`pending` → `processing` → `completed` / `failed`（`opened` 亦为终态）。

### 5.2 `report status` — 生成状态（`report status <id>`）

```jsonc
{
  "code": 200,
  "msg": "success",
  "data": {
    "status": "processing"      // pending / processing / completed / failed / opened
    // 其余字段以实际返回为准
  }
}
```

**状态取值与终态判定：**

| `data.status` | 中文 | 终态 |
|---------------|------|------|
| `pending` | 待处理 | 否 |
| `processing` | 处理中 | 否 |
| `completed` | 已完成 | ✅ |
| `failed` | 失败 | ✅ |
| `opened` | 已公开 | ✅ |

> 状态字段固定为 `data.status`。`--watch` 时轮询至终态；**超时返回 exit 1**、缺少 `status` 字段则提示后 exit 0、`code != 200` 立即 exit 3。

### 5.3 `report read --type link` — 报告在线链接

**返回的是纯文本链接字符串**（不是 `{code,msg,data}` 包裹），形如：

```text
https://<环境域名>/ReportResults/?id=<report_id>&reportTypeId=<n>
```

> - 提取方式：`grep -oE 'https://[^ ]+' | head -1`。
> - **必须先确认 `report status` 为 `completed` / `opened` 再调用**；非终态时可能为空或报错。
> - 登录失效时返回 `{"code":10003,"msg":"用户未授权"}`（此时才是 JSON）。
> - 该链接为**账号内查看**链接，打开后需登录 SouGEO 账号。

### 5.3b 分享链接（由 `share_code` 拼装，非 CLI 直接返回）

```text
https://sougeo.com/ReportResults?share_code=<share_code>
```

> - 由 `report share start` 生成 `share_code` 后**自行拼装**（CLI 不直接回该 URL）。
> - 有效期由 `--time`（`3days`/`week`/`always`）决定，`share stop` 后立即失效。
> - ⚠️ **打开后仍需登录 SouGEO 账号才能查看，不是免登录的公开页**——交付时须向用户说明，避免误解。
> - 👀 **分享后可在 SouGEO 网站上查看该报告的查看情况**（有哪些人查看过、查看时间/次数等）。该能力为**网站侧功能**，`report share` 仅有 `start`/`stop`，**CLI 无对应命令**，不要尝试用 CLI 拉取或编造查看数据。

### 5.4 `auth status` — 登录态（`-o json`）

```jsonc
// 已登录
{ "logged_in": true, "user": { "id": "246301", "mobile": "178****5971", "nickname": "..." } }
// 未登录
{ "logged_in": false }
```
> 手机号始终脱敏；`-v/--verbose` 仅影响 pretty 输出，JSON 模式忽略。

### 5.5 `rights list` — 权益余额（`-o json`）

```jsonc
{
  "code": 200,
  "data": { "":"0", "base":"0", "pro":"0", "plus":"0",
            "competitor_base":"0", "agent":"0", "industry_pro":"0" },
  "msg": "success",
  "request_id": "yoo-api..."
}
```
| 字段 | 含义 | 对应报告类型 |
|------|------|--------------|
| `base` | 极速版余额 | 品牌报告 `--report-level 1` |
| `pro` | 专业版余额 | 品牌报告 `--report-level 2` |
| `plus` | 动态分析版余额 | 品牌报告 `--report-level 3` |
| `competitor_base` | 竞品分析基础版 | — |
| `agent` | AI 分析师次数 | — |
| `industry_pro` | 行业版余额 | 行业报告 |
| `""`（空键） | 未分类余额（服务端原始数据，忽略或按 0 处理） | — |

### 5.6 `rights sku list` — 可购买套餐

```jsonc
{
  "code": 200, "msg": "success",
  "data": [
    {
      "id": "geo-rights-pro", "sku_id": "geo-rights-pro",
      "type": 12, "use_type": "geo",
      "name": "专业版", "title": "专业版", "product_name": "专业版", "unit": "份",
      "origin_price": 199, "now_price": 199, "first_price": 199,
      "order": "0", "status": "1",
      "extend": {
        "desc": "基于API+RPA的深度全网数据挖掘",
        "icon": "layers", "time": "5~20分钟获取结果：极速/专业",
        "_type": "pro",                       // base | pro | plus | industry_pro
        "_grade": { "base": 179.1 },          // 各等级的升级价
        "features": ["23项洞察指标", "13项AI分析+舆情+GEO建议", "单日深度数据"],
        "is_event": false, "event_text": "", "highlight": false
      }
    }
  ]
}
```

### 5.7 订单相关（`rights pay` / `rights check`）
以 `-o json` 调用时返回含订单号、支付链接/状态的对象；`rights check` 未支付时退出码为 **3**。字段以实际返回为准（价格与订单结构随业务迭代，**勿硬编码**）。

