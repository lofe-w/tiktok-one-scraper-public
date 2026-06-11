# TikTok One Scraper

[English README](https://github.com/lofe-w/tiktok-one-scraper-public/blob/main/README.md)

All In One，这是面向 TikTok One Top Ads 研究场景的一体化采集器。你可以稳定获取 TikTok One 官方 Top Ads Insight 和 Top Ads Library 工作流中的结构化数据，包括洞察概览指标、创意方法公式、卖点分析、Top Ads 素材列表、广告库搜索结果和素材详情，用于营销洞察、竞品研究和创意分析。

[Start Now (On Apify)](https://apify.com/doliz/tiktok-one-scraper)

## 核心功能

* **快速 API 采集**：直接调用 TikTok One 后端接口，避开缓慢的网页 UI 自动化，节省时间并减少自动化开销。
* **All-in-one Top Ads 覆盖**：一个 Actor 覆盖当前已实现的 TikTok One Top Ads Insight 和 Top Ads Library 工作流，包括：
    * [Top Ads Insight](https://ads.tiktok.com/creative/inspiration/top-ads/insight)
    * [Top Ads Library](https://ads.tiktok.com/creative/inspiration/top-ads/library)
    * Creative Approach 创意方法公式
    * Selling Point Analysis 卖点分析分类
    * 按分类获取 Top 20 selling points
    * Creative Approach 素材列表
    * Selling Point Analysis 素材列表
    * 素材详情查询
* **对齐官方筛选项**：使用 TikTok One 当前产品暴露的官方行业、国家/地区、时间范围、广告目标、点赞分位和排序字段。
* **结构化 JSON 输出**：返回适合看板、数据清洗、竞品监控和广告研究流程使用的机器可读 TikTok One API 数据。
* **公开选项文件**：常用筛选项发布在本仓库的 [`options/`](options/) 目录下。

## 最佳实践

本 Actor 使用你的 TikTok One 登录 Cookie 访问 TikTok One 后端接口。TikTok 可能存在未公开的认证检查和限流规则。为了稳定运行，建议参考以下做法：

### 使用有效的 TikTok One Cookie

请使用能够在浏览器中正常打开 TikTok One Top Ads 页面的账号 Cookie。如果 Cookie 缺失、过期、无效，或账号被 TikTok 限制，上游请求可能失败或返回认证错误。

### 大批量任务轮换多个账号

如果你需要频繁或高流量采集，建议在不同请求之间轮换多个账号 Cookie。这样可以分摊请求压力，降低单个账号被限流的概率。

### 控制并发请求数量

当你需要定时任务或同时调用多个 target 时，过高并发可能触发上游限流。

建议在客户端实现 [Rate Limiting](https://en.wikipedia.org/wiki/Rate_limiting) 控制请求速度。常见方式是 [Token Bucket](https://en.wikipedia.org/wiki/Token_bucket)：只有拿到 token 后才发起请求。

生产流水线中更稳健的方式是使用 [Message Queue](https://en.wikipedia.org/wiki/Message_queue)：先把采集任务放入队列，再用受控数量的 worker 消费任务，并对失败任务延迟重试。

## 输入配置

### Main

* **Target** `target`：（必填）选择 TikTok One 数据源。不同 target 会使用下方不同的配置项。

  可选值：`top_ads_insight`、`top_ads_insight_creative_approach`、`top_ads_insight_selling_point_analysis`、`top_ads_insight_top20_selling_points`、`top_ads_insight_formula_material_list`、`top_ads_insight_selling_point_material_list`、`top_ads_insight_material_detail`、`top_ads_library`、`top_ads_library_material_detail`。

* **Cookies** `cookies`：（必填）登录 `ads.tiktok.com` 上的 TikTok One 后获取的认证 Cookie。获取方式：

  ![How to get cookies](imgs/get_cookie.png)

---

### Top Ads Insight 设置

当 `Target` 设置为 `top_ads_insight` 时使用这些参数。

* **Industry label** `insight_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_order_field`：（必填）相关 insight 请求使用的排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

`top_ads_insight` 会模拟 Top Ads Insight 页面首次加载的数据请求，返回概览指标、Creative Approach 公式、Selling Point Analysis 分类，以及 TikTok One 返回的前几个分类下的 Top 20 selling points。

---

### Creative Approach 设置

当 `Target` 设置为 `top_ads_insight_creative_approach` 时使用这些参数。

* **Industry label** `insight_creative_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_creative_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_creative_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_creative_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

使用该 target 返回的 `formulaList[].id` 作为 `insight_formula_material_formula_id`，即可继续采集 Creative Approach 素材。

---

### Selling Point Analysis 设置

当 `Target` 设置为 `top_ads_insight_selling_point_analysis` 时使用这些参数。

* **Industry label** `insight_selling_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_selling_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_selling_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_selling_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

使用该 target 返回的 `SellingPoints[].categoryName` 作为 `insight_top20_category`，即可继续采集该分类下的 Top 20 selling points。

---

### Top 20 Selling Points 设置

当 `Target` 设置为 `top_ads_insight_top20_selling_points` 时使用这些参数。

* **Industry label** `insight_top20_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_top20_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_top20_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_top20_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Category** `insight_top20_category`：（必填）使用 `top_ads_insight_selling_point_analysis` 返回的 `SellingPoints[].categoryName`。

---

### Creative Approach Materials 设置

当 `Target` 设置为 `top_ads_insight_formula_material_list` 时使用这些参数。

* **Industry label** `insight_formula_material_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_formula_material_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_formula_material_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_formula_material_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Formula ID** `insight_formula_material_formula_id`：（必填）使用 `top_ads_insight_creative_approach` 返回的 `formulaList[].id`。
* **Page** `insight_formula_material_page`：（必填）页码。
* **Limit** `insight_formula_material_limit`：（必填）每页返回数量。

使用该 target 返回的 `itemList[].materialID` 作为 `insight_detail_material_id`，即可继续采集 Top Ads Insight 素材详情。

---

### Selling Point Analysis Materials 设置

当 `Target` 设置为 `top_ads_insight_selling_point_material_list` 时使用这些参数。

* **Industry label** `insight_selling_material_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_selling_material_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_selling_material_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_selling_material_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Selling point** `insight_selling_material_selling_point`：（必填）使用 `top_ads_insight_top20_selling_points` 返回的 `SellingPoints[].sellingPointName`。
* **Page** `insight_selling_material_page`：（必填）页码。
* **Limit** `insight_selling_material_limit`：（必填）每页返回数量。

使用该 target 返回的 `itemList[].materialID` 作为 `insight_detail_material_id`，即可继续采集 Top Ads Insight 素材详情。

---

### Top Ads Insight Material Detail 设置

当 `Target` 设置为 `top_ads_insight_material_detail` 时使用这些参数。

* **Insight material ID** `insight_detail_material_id`：（必填）使用 `top_ads_insight_formula_material_list` 或 `top_ads_insight_selling_point_material_list` 返回的 `itemList[].materialID`。

---

### Top Ads Library 设置

当 `Target` 设置为 `top_ads_library` 时使用这些参数。

* **Industry label** `library_industry_label`：（可选）行业标签 ID。留空表示全部行业。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_industry.json)
* **Country code** `library_country_code`：（可选）国家或地区代码。留空表示全部国家/地区。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_country.json)
* **Time range** `library_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_time_range.json)
* **Search word** `library_search_word`：（可选）按品牌、产品或创意关键词搜索。
* **Objective** `library_objective`：（可选）广告目标。留空表示全部目标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_objective.json)
* **Likes percentile** `library_like_cnt_filter`：（可选）点赞数分位筛选。留空表示全部分位。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_like_cnt_filter.json)
* **Sort by** `library_order_field`：（必填）结果排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_order_field.json)
* **Page** `library_page`：（必填）页码。
* **Limit** `library_limit`：（必填）每页返回数量。

使用该 target 返回的 `itemList[].materialID` 作为 `library_material_id`，即可继续采集 Top Ads Library 素材详情。

---

### Top Ads Library Material Detail 设置

当 `Target` 设置为 `top_ads_library_material_detail` 时使用这些参数。

* **Material ID** `library_material_id`：（必填）使用 `top_ads_library` 返回的 `itemList[].materialID`。

---

## Targets 和计费

| Target | 说明 | 计费 |
|---|---|---|
| `top_ads_insight` | 采集 Top Ads Insight 初始页面数据：概览指标、Creative Approach 公式、Selling Point Analysis 分类，以及 TikTok One 返回的前几个分类下的 Top 20 selling points。 | 0.002$ / fetched item |
| `top_ads_insight_creative_approach` | 采集 Creative Approach 公式。 | 0.002$ / item |
| `top_ads_insight_selling_point_analysis` | 采集 Selling Point Analysis 分类。 | 0.002$ / item |
| `top_ads_insight_top20_selling_points` | 采集指定分类下的 selling points。 | 0.002$ / item |
| `top_ads_insight_formula_material_list` | 按 Creative Approach 公式采集 Top Ads Insight 素材。 | 0.002$ / item |
| `top_ads_insight_selling_point_material_list` | 按 selling point 采集 Top Ads Insight 素材。 | 0.002$ / item |
| `top_ads_insight_material_detail` | 采集单个 Top Ads Insight 素材详情。 | 0.002$ / time |
| `top_ads_library` | 使用官方筛选项搜索 Top Ads Library 素材。 | 0.002$ / item |
| `top_ads_library_material_detail` | 采集单个 Top Ads Library 素材详情。 | 0.002$ / time |

## 输出结构

Actor 会返回 dataset items。每个 item 是所选 `target` 对应的结构化原始响应，并尽量保留 TikTok One 的字段名。

**说明**：下面是简化后的结构示例。实际输出可能随着 TikTok One 当前响应包含更多字段。

### Top Ads Insight

```json
{
    "dataInsight": {
        "BaseResp": {
            "StatusCode": 0,
            "StatusMessage": "success"
        },
        "videoView": 123456,
        "clickRate": 0.82,
        "engagementRate": 1.54,
        "videoView6sRate": 22.31
    },
    "creativeApproach": {
        "BaseResp": {
            "StatusCode": 0,
            "StatusMessage": "success"
        },
        "formulaList": [
            {
                "id": "example_formula_id",
                "content": "Example creative approach",
                "videoView": 12345
            }
        ]
    },
    "sellingPointAnalysis": {
        "category": {
            "SellingPoints": [
                {
                    "categoryName": "Example category"
                }
            ]
        },
        "top20SellingPoints": [
            {
                "SellingPoints": [
                    {
                        "sellingPointName": "Example selling point"
                    }
                ]
            }
        ]
    }
}
```

### Insight 列表

`top_ads_insight_creative_approach` 返回 `formulaList`。`top_ads_insight_selling_point_analysis` 和 `top_ads_insight_top20_selling_points` 返回 `SellingPoints`。素材列表 target 返回 `itemList` 和分页字段。

```json
{
    "BaseResp": {
        "StatusCode": 0,
        "StatusMessage": "success"
    },
    "itemList": [
        {
            "materialID": "1234567890",
            "videoView": 12345,
            "clickRate": 0.82,
            "engagementRate": 1.54
        }
    ],
    "pagination": {
        "page": 1,
        "size": 20,
        "total": 100,
        "hasMore": true
    }
}
```

### Top Ads Library

```json
{
    "BaseResp": {
        "StatusCode": 0,
        "StatusMessage": "success"
    },
    "itemList": [
        {
            "materialID": "1234567890",
            "brandName": "Example brand",
            "videoView": 12345,
            "clickRate": 0.82,
            "engagementRate": 1.54
        }
    ],
    "pagination": {
        "page": 1,
        "size": 20,
        "total": 100,
        "hasMore": true
    }
}
```

### Material Detail

`top_ads_insight_material_detail` 和 `top_ads_library_material_detail` 都会返回单个 `materialID` 的详情响应。

```json
{
    "BaseResp": {
        "StatusCode": 0,
        "StatusMessage": "success"
    },
    "materialID": "1234567890",
    "videoView": 12345,
    "clickRate": 0.82,
    "engagementRate": 1.54,
    "videoInfo": {
        "cover": "https://...",
        "videoUrl": "https://..."
    }
}
```

## 说明

TikTok One 运行在 `ads.tiktok.com`，需要有效登录 Cookie。部分 Top Ads Insight 接口还需要 TikTok One 浏览器侧请求签名；Actor 会在内部处理这些签名，但账号 Cookie 仍然需要有权限访问官方 TikTok One 页面。

本公开仓库只包含面向用户的文档、图片和选项文件。Apify Actor 运行时代码保存在私有 Actor 仓库中。
