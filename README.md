# TikTok One Scraper

[中文 README](https://github.com/lofe-w/tiktok-one-scraper-public/blob/main/README.zh-CN.md)

All In One! A focused scraper for the official TikTok One Top Ads surfaces available today. Reliably extract structured Top Ads Insight metrics, creative approach formulas, selling point analysis, Top Ads material lists, Top Ads Library search results, and material details for marketing intelligence, competitive research, and creative analysis.

[Start Now (On Apify)](https://apify.com/doliz/tiktok-one-scraper)

## ✨ Key Features

* **⚡️ Fast & Efficient**: Bypasses slow UI interactions by calling TikTok One backend APIs directly, saving time and platform costs.
* **🎯 All-in-one Top Ads Scraping**: One Actor covering the currently implemented official TikTok One Top Ads workflows, including:
    * [Top Ads Insight](https://ads.tiktok.com/creative/inspiration/top-ads/insight)
    * [Top Ads Library](https://ads.tiktok.com/creative/inspiration/top-ads/library)
    * Creative Approach formulas
    * Selling Point Analysis categories
    * Top 20 selling points by category
    * Creative Approach material lists
    * Selling Point Analysis material lists
    * Material detail lookup
* **🔎 Powerful Filtering**: Use TikTok One's official industry, country, time range, objective, likes percentile, and sorting fields where the current product exposes them.
* **📦 Structured JSON Output**: Get clean, machine-readable data ready for dashboards, enrichment pipelines, competitive monitoring, and ad research workflows.
* **🧭 Officially Aligned Scope**: This Actor follows the current TikTok One Top Ads product scope and only exposes implemented, runnable targets.

## 💡 Best practices

This Actor operates by directly accessing TikTok One backend APIs. TikTok may apply undisclosed authentication checks and rate limits. To keep runs stable and avoid throttling, we strongly recommend the following best practices:

### 💡 Use valid TikTok One cookies

Use cookies from an account that can open the TikTok One Top Ads pages in a browser. If cookies are missing, expired, invalid, or blocked by TikTok, upstream requests may fail or return authentication errors.

### 💡 Utilize multiple accounts (cookies)

For frequent or high-volume scraping, provide a pool of account cookies and rotate through them for different requests. This distributes request load and reduces the risk of any single account being rate-limited.

### 💡 Limit the number of concurrent requests

For scheduled or bulk workflows, too many concurrent upstream requests may be throttled.

You can implement [Rate Limiting](https://en.wikipedia.org/wiki/Rate_limiting) on the client side. A common approach is the [Token Bucket Algorithm](https://en.wikipedia.org/wiki/Token_bucket): tokens are issued according to your rate limit rule, and each request must obtain a token before it starts.

A more robust implementation is to use [Message Queuing](https://en.wikipedia.org/wiki/Message_queue). Push scraping tasks to a queue, let dedicated consumers process them, limit the number of concurrent consumers, and retry failed tasks later.

## ⚙️ Input Configuration

### ⚙️ Main

* **Target** `target`: (Required) Select the TikTok One data source. Your choice determines which settings below are used.

  One of `top_ads_insight`, `top_ads_insight_creative_approach`, `top_ads_insight_selling_point_analysis`, `top_ads_insight_top20_selling_points`, `top_ads_insight_formula_material_list`, `top_ads_insight_selling_point_material_list`, `top_ads_insight_material_detail`, `top_ads_library`, `top_ads_library_material_detail`.

* **Cookies** `cookies`: (Required) Your authentication cookies after logging into TikTok One on `ads.tiktok.com`. Way to obtain:

  ![](https://github.com/lofe-w/tiktok-one-scraper-public/raw/main/imgs/get_cookie.png)

---

### ⚙️ Top Ads Insight Settings

These settings are only used when `Target` is set to `top_ads_insight`.

* **Industry label** `insight_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_order_field`: (Required) Ranking metric used by related insight requests. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

The `top_ads_insight` target mirrors the initial Top Ads Insight page load. It returns overview metrics, Creative Approach formulas, Selling Point Analysis categories, and Top 20 selling points for the first categories returned by TikTok One.

---

### ⚙️ Creative Approach Settings

These settings are only used when `Target` is set to `top_ads_insight_creative_approach`.

* **Industry label** `insight_creative_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_creative_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_creative_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_creative_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

Use `formulaList[].id` from this target as `insight_formula_material_formula_id` when fetching Creative Approach materials.

---

### ⚙️ Selling Point Analysis Settings

These settings are only used when `Target` is set to `top_ads_insight_selling_point_analysis`.

* **Industry label** `insight_selling_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_selling_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_selling_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_selling_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

Use `SellingPoints[].categoryName` from this target as `insight_top20_category` when fetching Top 20 selling points.

---

### ⚙️ Top 20 Selling Points Settings

These settings are only used when `Target` is set to `top_ads_insight_top20_selling_points`.

* **Industry label** `insight_top20_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_top20_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_top20_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_top20_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Category** `insight_top20_category`: (Required) Use `SellingPoints[].categoryName` from `top_ads_insight_selling_point_analysis`.

---

### ⚙️ Creative Approach Materials Settings

These settings are only used when `Target` is set to `top_ads_insight_formula_material_list`.

* **Industry label** `insight_formula_material_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_formula_material_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_formula_material_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_formula_material_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Formula ID** `insight_formula_material_formula_id`: (Required) Use `formulaList[].id` from `top_ads_insight_creative_approach`.
* **Page** `insight_formula_material_page`: (Required) Page number.
* **Limit** `insight_formula_material_limit`: (Required) Number of records per page.

Use `itemList[].materialID` from this target as `insight_detail_material_id` when fetching a Top Ads Insight material detail.

---

### ⚙️ Selling Point Analysis Materials Settings

These settings are only used when `Target` is set to `top_ads_insight_selling_point_material_list`.

* **Industry label** `insight_selling_material_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_selling_material_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_selling_material_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_selling_material_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Selling point** `insight_selling_material_selling_point`: (Required) Use `SellingPoints[].sellingPointName` from `top_ads_insight_top20_selling_points`.
* **Page** `insight_selling_material_page`: (Required) Page number.
* **Limit** `insight_selling_material_limit`: (Required) Number of records per page.

Use `itemList[].materialID` from this target as `insight_detail_material_id` when fetching a Top Ads Insight material detail.

---

### ⚙️ Top Ads Insight Material Detail Settings

These settings are only used when `Target` is set to `top_ads_insight_material_detail`.

* **Insight material ID** `insight_detail_material_id`: (Required) Use `itemList[].materialID` from `top_ads_insight_formula_material_list` or `top_ads_insight_selling_point_material_list`.

---

### ⚙️ Top Ads Library Settings

These settings are only used when `Target` is set to `top_ads_library`.

* **Industry label** `library_industry_label`: (Optional) Industry label ID. Leave empty for all industries. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_industry.json)
* **Country code** `library_country_code`: (Optional) Country or region code. Leave empty for all countries and regions. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_country.json)
* **Time range** `library_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_time_range.json)
* **Search word** `library_search_word`: (Optional) Search by brand, product, or creative keyword.
* **Objective** `library_objective`: (Optional) Campaign objective. Leave empty for all objectives. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_objective.json)
* **Likes percentile** `library_like_cnt_filter`: (Optional) Like count percentile filter. Leave empty for all percentiles. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_like_cnt_filter.json)
* **Sort by** `library_order_field`: (Required) Metric used to sort results. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_order_field.json)
* **Page** `library_page`: (Required) Page number.
* **Limit** `library_limit`: (Required) Number of records per page.

Use `itemList[].materialID` from this target as `library_material_id` when fetching a Top Ads Library material detail.

---

### ⚙️ Top Ads Library Material Detail Settings

These settings are only used when `Target` is set to `top_ads_library_material_detail`.

* **Material ID** `library_material_id`: (Required) Use `itemList[].materialID` from `top_ads_library`.

---

## 📊 Output Structure

The Actor returns a dataset of items. The structure of each item depends on the `target` you selected.

**NOTE**: The following are real sample structures from TikTok One API responses. The actual output may contain more fields. Please refer to the output of a sample run for the exact schema.

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

Used by `top_ads_insight_formula_material_list` and `top_ads_insight_selling_point_material_list`.

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

Used by `top_ads_insight_material_detail` and `top_ads_library_material_detail`.

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

## 💰 Cost of Use & Pricing

Pricing model: [Pay per event](https://docs.apify.com/platform/actors/publishing/monetize/pay-per-event)

The trigger logic of the event is the number of items that return the result.

| Target | Cost |
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

Unknown or unsupported targets are rejected before any upstream request or charge.

## 📞 Support

If you encounter any issues or have feature requests, please use the **Issues** tab on the Actor's page in the Apify Console. Please provide a detailed description of the problem and the Run ID.

## ⚠️ Limitations and Disclaimers

* This Actor is not an official TikTok product and is not affiliated with or endorsed by TikTok, Inc.
* The structure of the TikTok One website and its internal APIs may change at any time.
* TikTok One requests require valid logged-in cookies from `ads.tiktok.com`; expired, invalid, rate-limited, or unauthorized cookies can cause upstream failures.
* Some Top Ads Insight endpoints require browser-side request signatures. The Actor handles these internally, but successful access still depends on TikTok One accepting the account session.
* Please use this Actor responsibly and in accordance with the Apify Terms of Service.
