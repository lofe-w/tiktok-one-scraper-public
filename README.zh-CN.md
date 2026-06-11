# TikTok One Scraper

[English README](https://github.com/lofe-w/tiktok-one-scraper-public/blob/main/README.md)

All In One！这是面向 TikTok One 当前官方 Top Ads 页面的专用采集器。你可以稳定获取 Top Ads Insight 指标、Creative Approach 创意方法公式、Selling Point Analysis 卖点分析、Top Ads 素材列表、Top Ads Library 搜索结果和素材详情等结构化数据，用于营销洞察、竞品研究和创意分析。

[Start Now (On Apify)](https://apify.com/doliz/tiktok-one-scraper)

## ✨ 核心功能

* **⚡️ 快速高效**：直接调用 TikTok One 后端接口，避开缓慢的页面自动化操作，节省时间和平台成本。
* **🎯 All-in-one Top Ads 采集**：一个 Actor 覆盖当前已实现的 TikTok One 官方 Top Ads 工作流，包括：
    * [Top Ads Insight](https://ads.tiktok.com/creative/inspiration/top-ads/insight)
    * [Top Ads Library](https://ads.tiktok.com/creative/inspiration/top-ads/library)
    * Creative Approach 创意方法公式
    * Selling Point Analysis 卖点分析分类
    * 按分类获取 Top 20 selling points
    * Creative Approach 素材列表
    * Selling Point Analysis 素材列表
    * 素材详情查询
* **🔎 强大的筛选能力**：使用 TikTok One 当前产品暴露的官方行业、国家/地区、时间范围、广告目标、点赞分位和排序字段。
* **📦 结构化 JSON 输出**：返回适合看板、数据清洗、竞品监控和广告研究流程使用的机器可读数据。
* **🧭 与官方当前范围对齐**：本 Actor 跟随 TikTok One 当前 Top Ads 产品范围，只暴露已经实现且可运行的 target。

## 💡 最佳实践

本 Actor 通过访问 TikTok One 后端接口运行。TikTok 可能存在未公开的认证检查和限流规则。为了稳定运行并减少被限流的概率，建议参考以下做法：

### 💡 使用有效的 TikTok One Cookie

请使用能够在浏览器中正常打开 TikTok One Top Ads 页面的账号 Cookie。如果 Cookie 缺失、过期、无效，或账号被 TikTok 限制，上游请求可能失败或返回认证错误。

### 💡 使用多个账号 Cookie

如果你需要频繁或大批量采集，建议提供一组账号 Cookie，并在不同请求之间轮换使用。这样可以分摊请求压力，降低单个账号被限流的风险。

### 💡 控制并发请求数量

当你需要定时或批量调用多个接口时，如果同时发起大量请求，可能会触发上游限流。

建议在客户端实现 [Rate Limiting](https://en.wikipedia.org/wiki/Rate_limiting) 控制请求速度。例如使用 [Token Bucket Algorithm](https://en.wikipedia.org/wiki/Token_bucket)：按固定速率发放 token，请求前先获取 token，没有 token 时等待后重试。

更稳健的方式是使用 [Message Queuing](https://en.wikipedia.org/wiki/Message_queue)。先把采集任务推入队列，由专门的消费者进程取出任务并请求接口；你可以通过限制消费者数量或增加限流逻辑来控制请求速度，失败任务也可以重新入队并延迟重试。

## ⚙️ 输入配置

### ⚙️ Main

* **Target** `target`：（必填）选择 TikTok One 数据源。不同 target 会使用下方不同的配置项。

  可选值：`top_ads_insight`、`top_ads_insight_creative_approach`、`top_ads_insight_selling_point_analysis`、`top_ads_insight_top20_selling_points`、`top_ads_insight_formula_material_list`、`top_ads_insight_selling_point_material_list`、`top_ads_insight_material_detail`、`top_ads_library`、`top_ads_library_material_detail`。

* **Cookies** `cookies`：（必填）登录 `ads.tiktok.com` 上的 TikTok One 后获取的认证 Cookie。获取方式：

  ![](https://github.com/lofe-w/tiktok-one-scraper-public/raw/main/imgs/get_cookie.png)

---

### ⚙️ Top Ads Insight 设置

当 `Target` 设置为 `top_ads_insight` 时使用这些参数。

* **Industry label** `insight_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_order_field`：（必填）相关 insight 请求使用的排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

`top_ads_insight` 会模拟 Top Ads Insight 页面首次加载的数据请求，返回概览指标、Creative Approach 公式、Selling Point Analysis 分类，以及 TikTok One 返回的前几个分类下的 Top 20 selling points。

---

### ⚙️ Creative Approach 设置

当 `Target` 设置为 `top_ads_insight_creative_approach` 时使用这些参数。

* **Industry label** `insight_creative_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_creative_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_creative_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_creative_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

使用该 target 返回的 `formulaList[].id` 作为 `insight_formula_material_formula_id`，即可继续采集 Creative Approach 素材。

---

### ⚙️ Selling Point Analysis 设置

当 `Target` 设置为 `top_ads_insight_selling_point_analysis` 时使用这些参数。

* **Industry label** `insight_selling_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_selling_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_selling_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_selling_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

使用该 target 返回的 `SellingPoints[].categoryName` 作为 `insight_top20_category`，即可继续采集该分类下的 Top 20 selling points。

---

### ⚙️ Top 20 Selling Points 设置

当 `Target` 设置为 `top_ads_insight_top20_selling_points` 时使用这些参数。

* **Industry label** `insight_top20_industry_label`：（必填）官方行业标签。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_top20_country_code`：（必填）官方国家或地区筛选。留空表示 `All`。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_top20_time_range`：（必填）发布时间范围。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_top20_order_field`：（必填）排序指标。[Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Category** `insight_top20_category`：（必填）使用 `top_ads_insight_selling_point_analysis` 返回的 `SellingPoints[].categoryName`。

---

### ⚙️ Creative Approach Materials 设置

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

### ⚙️ Selling Point Analysis Materials 设置

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

### ⚙️ Top Ads Insight Material Detail 设置

当 `Target` 设置为 `top_ads_insight_material_detail` 时使用这些参数。

* **Insight material ID** `insight_detail_material_id`：（必填）使用 `top_ads_insight_formula_material_list` 或 `top_ads_insight_selling_point_material_list` 返回的 `itemList[].materialID`。

---

### ⚙️ Top Ads Library 设置

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

### ⚙️ Top Ads Library Material Detail 设置

当 `Target` 设置为 `top_ads_library_material_detail` 时使用这些参数。

* **Material ID** `library_material_id`：（必填）使用 `top_ads_library` 返回的 `itemList[].materialID`。

---

## 📊 输出结构

Actor 会返回 dataset items。每个 item 的结构取决于你选择的 `target`。

**说明**：下面是来自 TikTok One API 响应的真实示例结构。实际输出可能包含更多字段，请以实际运行结果为准。

---

### 📊 Top Ads Insight

```json
{
    "dataInsight": {
        "BaseResp": {
            "StatusCode": 0,
            "StatusMessage": ""
        },
        "clickRate": 0.01861966174097709,
        "engagementRate": 0.004667982359031695,
        "videoView": "2923411862",
        "videoView6sRate": 0.13300873819879164
    },
    "creativeApproach": {
        "BaseResp": {
            "StatusCode": 0,
            "StatusMessage": ""
        },
        "formulaList": [
            {
                "clickRate": 0.01532804205489521,
                "content": [
                    "Function/Attribute",
                    "Physical product/Commodity",
                    "Applicable Scenarios",
                    "Target Audience"
                ],
                "engagementRate": 0.003626492592307972,
                "id": "[Function/Attribute, Physical product/Commodity, Applicable Scenarios, Target Audience]",
                "postNumPct": 0.10424275523091102,
                "videoView": "725058423",
                "videoView6sRate": 0.1357802376705856
            },
            ... /* omit */
        ]
    },
    "sellingPointAnalysis": {
        "category": {
            "BaseResp": {
                "StatusCode": 0,
                "StatusMessage": ""
            },
            "SellingPoints": [
                {
                    "categoryName": "Target Audience",
                    "clickRate": 0.015131855660077015,
                    "clickRateWow": -0.021820344372670414,
                    "engagementRate": 0.006421364107410872,
                    "engagementRateWow": 0.037459146764485536,
                    "postNumPct": 0.8560120182585081,
                    "sellingPointName": "",
                    "videoView": "7085131950",
                    "videoView6sRate": 0.12949835648438418,
                    "videoView6sRateWow": -0.013653260524311877,
                    "videoViewWow": 0.47982782334909313
                },
                ... /* omit */
            ]
        },
        "top20SellingPoints": [
            {
                "BaseResp": {
                    "StatusCode": 0,
                    "StatusMessage": ""
                },
                "SellingPoints": [
                    {
                        "categoryName": "Target Audience",
                        "clickRate": 0.013211661428313998,
                        "clickRateWow": -0.04245137306068203,
                        "engagementRate": 0.0031889771656263264,
                        "engagementRateWow": -0.06709534547129498,
                        "postNumPct": 0.10068180504997976,
                        "sellingPointName": "Homeowners and Maintenance Enthusiasts",
                        "videoView": "836561023",
                        "videoView6sRate": 0.14181924418919528,
                        "videoView6sRateWow": -0.03342331175429736,
                        "videoViewWow": 0.5779036729199662
                    },
                    ... /* omit */
                ]
            },
            ... /* omit */
        ]
    }
}
```

---

### 📊 Top Ads Insight - Material List

用于 `top_ads_insight_formula_material_list` 和 `top_ads_insight_selling_point_material_list`。

```json
{
    "BaseResp": {
        "StatusCode": 0,
        "StatusMessage": ""
    },
    "itemList": [
        {
            "brandName": "",
            "clickRate": 0,
            "commentCnt": "0",
            "countryCodeList": [
                "SA"
            ],
            "ctrRank": 0.66,
            "engagementRate": 0.00036005527335007106,
            "industry": "18000000000",
            "landingPage": "",
            "likeCnt": "0",
            "materialID": "7637823224530026516",
            "shareCnt": "0",
            "title": "احمي اثاثك مع بخاخ نانو فور لايف",
            "videoInfo": {
                "cover": "https://p19-common-sign.tiktokcdn.com/tos-alisg-p-0051c001-sg/oYDfXgPVQCNf9IE6JfbAGX7DJCJwwA7ULDAgFT~tplv-photomode-zoomcover-tcm:720:720.avif?dr=16670&refresh_token=c241a2c4&x-expires=1781740800&x-signature=jSOpVOxf%2FV4pFCjlNMt%2FS%2BmJ5x4%3D&t=e8257cf9&ps=933b5bde&shp=c8b38fb9&shcp=c8b38fb9&idc=my2",
                "duration": 21.316,
                "height": "1280",
                "vid": "v10033g50000d7vg15fog65tgqjqdts0",
                "video_url": {
                    "720p": "https://v16m-default.tiktokcdn.com/a910ff2b45523eb66f31e225e9c6a627/6a2ab814/video/tos/alisg/tos-alisg-ve-0051c001-sg/oQ9eM839ZQTAief79APAWK67AHtfAgahfMwlkS/?a=0&bti=NTU4QDM1NGA%3D&&bt=1801&ft=cApXJCz7ThWHzMr9LGZmo0P&mime_type=video_mp4&rc=aGY7Z2k4ODlmODNoPDU8NUBpM3d0dHc5cjhqOjMzODYzNEA0NV4tMTJeNS4xMzYxMWNiYSNwYWdkMmRjLnNhLS1kMC1zcw%3D%3D&vvpl=1&l=202606111528313D38198328B44E682ADF&btag=e000b8000"
                },
                "width": "720"
            },
            "videoView": "411048",
            "videoView6sRank": 0.73,
            "videoView6sRate": 0
        },
        ... /* omit */
    ],
    "pagination": {
        "hasMore": false,
        "page": "1",
        "size": "2",
        "total": "2"
    }
}
```

---

### 📊 Top Ads Library

```json
{
    "BaseResp": {
        "StatusCode": 0,
        "StatusMessage": ""
    },
    "itemList": [
        {
            "adv_id": [
                "7372413843525517313"
            ],
            "brandName": "",
            "clickRate": 0,
            "commentCnt": "0",
            "countryCodeList": [
                "PH"
            ],
            "ctrRank": 0.04,
            "engagementRate": 0.0010963407354446612,
            "industry": "23125000000",
            "landingPage": "",
            "likeCnt": "0",
            "materialID": "7644457820796878856",
            "objective": [
                5
            ],
            "shareCnt": "0",
            "title": "Pabangisin ang bonding ng tropa with Foodvencha. #TikmanAngAdvencha P0173P052526M",
            "videoInfo": {
                "cover": "https://p16-common-sign.tiktokcdn.com/tos-alisg-p-0037/o01V469EWVvE5h6iWsuNnpYmBGIaAibaLB9AI~tplv-photomode-zoomcover-tcm:720:720.avif?dr=16670&refresh_token=a327d82d&x-expires=1781748000&x-signature=g%2BKhapyPyF7CntIv%2FXghBzDZMwY%3D&t=e8257cf9&ps=933b5bde&shp=c8b38fb9&shcp=c8b38fb9&idc=my",
                "duration": 15.022,
                "height": "1280",
                "vid": "v14044g50000d8aplpnog65tjr3c996g",
                "video_url": {
                    "720p": "https://v16m-default.tiktokcdn.com/a9b53b615ecfa7f5c90c4fdf794e5f4a/6a2ab825/video/tos/alisg/tos-alisg-pve-0037c001/okSgbQEhYEAuasAWBkWVIEpNFLvkp5ihn9Bia/?a=0&bti=NTU4QDM1NGA%3D&&bt=2884&ft=cApXJCz7ThWHeMr9LGZmo0P&mime_type=video_mp4&rc=aTszNjg5OWVpPGZoZGRpZ0BpajxmdXc5cnNzOzMzODczNEAzMi9iMGFhNTMxNi5hMWFjYSMzNjBnMmRraV5hLS1kMTFzcw%3D%3D&vvpl=1&l=202606111528544E4EA3BB5EA48867FD7A&btag=e000b8000"
                },
                "width": "720"
            },
            "videoView": "82856540",
            "videoView6sRank": 0.08,
            "videoView6sRate": 0.1343032426572047
        },
        ... /* omit */
    ],
    "pagination": {
        "hasMore": true,
        "page": "1",
        "size": "2",
        "total": "500"
    }
}
```

---

### 📊 Material Detail

用于 `top_ads_insight_material_detail` 和 `top_ads_library_material_detail`。

```json
{
    "BaseResp": {
        "StatusCode": 0,
        "StatusMessage": ""
    },
    "itemInfo": {
        "brandName": "",
        "clickRate": 0,
        "commentCnt": "675",
        "countryCodeList": [
            "PH"
        ],
        "ctrRank": 0.04,
        "engagementRate": 0.0010963407354446612,
        "industry": "23000000000",
        "keyFrameInfo": [
            {
                "click_cnt": 0.7028168762184107,
                "convert_cnt": 0,
                "play_retain_cnt": 1,
                "retain_ctr": 0.4027917084587095,
                "retain_cvr": 0,
                "second": "0"
            },
            ... /* omit */
        ],
        "landingPage": "https://www.klook.com/en-PH/tetris/promo/dewfoodvenchatour/",
        "likeCnt": "89051",
        "materialID": "7644457820796878856",
        "objective": [
            5
        ],
        "shareCnt": "1113",
        "title": "Pabangisin ang bonding ng tropa with Foodvencha. #TikmanAngAdvencha P0173P052526M",
        "videoInfo": {
            "cover": "https://p16-common-sign.tiktokcdn.com/tos-alisg-p-0037/o01V469EWVvE5h6iWsuNnpYmBGIaAibaLB9AI~tplv-noop.image?dr=18692&refresh_token=bff93274&x-expires=1781184550&x-signature=Qsoq1AIxg7tDb01r%2F9pqZy%2BRizI%3D&t=9276707c&ps=14f1eb3e&shp=9e36835a&shcp=317596d8&idc=my2&VideoID=v14044g50000d8aplpnog65tjr3c996g",
            "duration": 15.022,
            "height": "1280",
            "vid": "v14044g50000d8aplpnog65tjr3c996g",
            "video_url": {
                "720p": "https://v16m-default.tiktokcdn.com/d6221c39c77ff604a181e6e9df87799a/6a2ab826/video/tos/alisg/tos-alisg-pve-0037c001/okSgbQEhYEAuasAWBkWVIEpNFLvkp5ihn9Bia/?a=0&bti=NTU4QDM1NGA%3D&&bt=2884&ft=cApXJCz7ThWHsMr9LGZmo0P&mime_type=video_mp4&rc=aTszNjg5OWVpPGZoZGRpZ0BpajxmdXc5cnNzOzMzODczNEAzMi9iMGFhNTMxNi5hMWFjYSMzNjBnMmRraV5hLS1kMTFzcw%3D%3D&vvpl=1&l=202606111528552512DCABE9F7496A60E3&btag=e000b8000"
            },
            "width": "720"
        },
        "videoView": "82856540",
        "videoView6sRank": 0.08,
        "videoView6sRate": 0
    }
}
```

---

## 💰 使用成本与计费

Pricing model: [Pay per event](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event)

事件触发逻辑为返回结果中的 item 数量。

| Target | 计费 |
|---|---|
| [Top Ads Insight](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / fetched item |
| [Top Ads Insight - Creative Approach](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / item |
| [Top Ads Insight - Selling Point Analysis](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / item |
| [Top Ads Insight - Top 20 Selling Points](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / item |
| [Top Ads Insight - Creative Approach Materials](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / item |
| [Top Ads Insight - Selling Point Analysis Materials](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / item |
| [Top Ads Insight - Material Detail](https://ads.tiktok.com/creative/inspiration/top-ads/insight) | 0.002$ / time |
| [Top Ads Library](https://ads.tiktok.com/creative/inspiration/top-ads/library) | 0.002$ / item |
| [Top Ads Library - Material Detail](https://ads.tiktok.com/creative/inspiration/top-ads/library) | 0.002$ / time |

未知或未支持的 target 会在发起上游请求和计费前被拒绝。

## 📞 Support

如果你遇到问题或有功能建议，请在 Apify Console 的 Actor 页面使用 **Issues** 标签。请尽量提供详细的问题描述和 Run ID。

## ⚠️ 限制与免责声明

* 本 Actor 不是 TikTok 官方产品，也不隶属于 TikTok, Inc. 或获得其背书。
* TikTok One 网站结构及其内部 API 可能随时变化。
* TikTok One 请求需要来自 `ads.tiktok.com` 的有效登录 Cookie；过期、无效、被限流或无权限的 Cookie 可能导致上游请求失败。
* 部分 Top Ads Insight 接口需要浏览器侧请求签名；Actor 会在内部处理这些签名，但是否成功访问仍取决于 TikTok One 是否接受账号会话。
* 请负责任地使用本 Actor，并遵守 Apify Terms of Service。
