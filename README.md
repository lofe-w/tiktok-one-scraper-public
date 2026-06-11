# TikTok One Scraper

[中文 README](https://github.com/lofe-w/tiktok-one-scraper-public/blob/main/README.zh-CN.md)

All In One for TikTok One Top Ads research. Reliably extract structured data from the official TikTok One Top Ads Insight and Top Ads Library workflows, including insight overview metrics, creative approach formulas, selling point analysis, Top Ads material lists, ad library search results, and material details for marketing intelligence, competitive research, and creative analysis.

[Start Now (On Apify)](https://apify.com/doliz/tiktok-one-scraper)

## Key Features

* **Fast API-based extraction**: Calls TikTok One backend APIs directly instead of driving the website UI, saving time and reducing automation overhead.
* **All-in-one Top Ads coverage**: One Actor covers the currently implemented TikTok One Top Ads Insight and Top Ads Library workflows:
    * [Top Ads Insight](https://ads.tiktok.com/creative/inspiration/top-ads/insight)
    * [Top Ads Library](https://ads.tiktok.com/creative/inspiration/top-ads/library)
    * Creative Approach formulas
    * Selling Point Analysis categories
    * Top 20 selling points by category
    * Creative Approach material lists
    * Selling Point Analysis material lists
    * Material detail lookup
* **Official filter alignment**: Uses TikTok One's official industry, country, time range, objective, likes percentile, and sorting fields where the current product exposes them.
* **Structured JSON output**: Returns machine-readable TikTok One API responses ready for dashboards, enrichment pipelines, competitive monitoring, and ad research workflows.
* **Public option files**: Common filter values are published in this repository under [`options/`](options/).

## Best Practices

This Actor works by accessing TikTok One backend APIs with your logged-in TikTok One cookies. TikTok may apply undisclosed authentication checks and rate limits. For stable runs, we recommend the following:

### Use Valid TikTok One Cookies

Use cookies from an account that can open the TikTok One Top Ads pages in a browser. If cookies are missing, expired, invalid, or blocked by TikTok, upstream requests may fail or return authentication errors.

### Rotate Multiple Accounts for Larger Workloads

For frequent or high-volume scraping, rotate multiple account cookies across requests. This distributes request load and reduces the chance that one account is throttled.

### Limit Concurrent Requests

If you run scheduled jobs or call several targets at the same time, too many concurrent upstream requests can trigger rate limiting.

Client-side [rate limiting](https://en.wikipedia.org/wiki/Rate_limiting) is recommended. A common approach is a [token bucket](https://en.wikipedia.org/wiki/Token_bucket), where requests can only start after a token is available.

For production pipelines, use a [message queue](https://en.wikipedia.org/wiki/Message_queue): push scraping tasks into a queue, process them with a controlled number of workers, and retry failed tasks later.

## Input Configuration

### Main

* **Target** `target`: (Required) Select the TikTok One data source. Your choice determines which settings below are used.

  One of `top_ads_insight`, `top_ads_insight_creative_approach`, `top_ads_insight_selling_point_analysis`, `top_ads_insight_top20_selling_points`, `top_ads_insight_formula_material_list`, `top_ads_insight_selling_point_material_list`, `top_ads_insight_material_detail`, `top_ads_library`, `top_ads_library_material_detail`.

* **Cookies** `cookies`: (Required) Your authentication cookies after logging into TikTok One on `ads.tiktok.com`. Way to obtain:

  ![How to get cookies](imgs/get_cookie.png)

---

### Top Ads Insight Settings

These settings are only used when `Target` is set to `top_ads_insight`.

* **Industry label** `insight_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_order_field`: (Required) Ranking metric used by related insight requests. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

The `top_ads_insight` target mirrors the initial Top Ads Insight page load. It returns the overview metrics, Creative Approach formulas, Selling Point Analysis categories, and Top 20 selling points for the first categories returned by TikTok One.

---

### Creative Approach Settings

These settings are only used when `Target` is set to `top_ads_insight_creative_approach`.

* **Industry label** `insight_creative_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_creative_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_creative_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_creative_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

Use `formulaList[].id` from this target as `insight_formula_material_formula_id` when fetching Creative Approach materials.

---

### Selling Point Analysis Settings

These settings are only used when `Target` is set to `top_ads_insight_selling_point_analysis`.

* **Industry label** `insight_selling_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_selling_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_selling_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_selling_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

Use `SellingPoints[].categoryName` from this target as `insight_top20_category` when fetching Top 20 selling points.

---

### Top 20 Selling Points Settings

These settings are only used when `Target` is set to `top_ads_insight_top20_selling_points`.

* **Industry label** `insight_top20_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_top20_country_code`: (Required) Official country or region filter. Leave empty for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_top20_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_top20_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)
* **Category** `insight_top20_category`: (Required) Use `SellingPoints[].categoryName` from `top_ads_insight_selling_point_analysis`.

---

### Creative Approach Materials Settings

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

### Selling Point Analysis Materials Settings

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

### Top Ads Insight Material Detail Settings

These settings are only used when `Target` is set to `top_ads_insight_material_detail`.

* **Insight material ID** `insight_detail_material_id`: (Required) Use `itemList[].materialID` from `top_ads_insight_formula_material_list` or `top_ads_insight_selling_point_material_list`.

---

### Top Ads Library Settings

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

### Top Ads Library Material Detail Settings

These settings are only used when `Target` is set to `top_ads_library_material_detail`.

* **Material ID** `library_material_id`: (Required) Use `itemList[].materialID` from `top_ads_library`.

---

## Targets and Cost

| Target | Description | Cost |
|---|---|---|
| `top_ads_insight` | Fetches the initial Top Ads Insight page data: overview metrics, Creative Approach formulas, Selling Point Analysis categories, and Top 20 selling points for the first categories returned by TikTok One. | 0.002$ / fetched item |
| `top_ads_insight_creative_approach` | Fetches Creative Approach formulas. | 0.002$ / item |
| `top_ads_insight_selling_point_analysis` | Fetches Selling Point Analysis category rows. | 0.002$ / item |
| `top_ads_insight_top20_selling_points` | Fetches selling points under a selected category. | 0.002$ / item |
| `top_ads_insight_formula_material_list` | Fetches Top Ads Insight materials for a selected Creative Approach formula. | 0.002$ / item |
| `top_ads_insight_selling_point_material_list` | Fetches Top Ads Insight materials for a selected selling point. | 0.002$ / item |
| `top_ads_insight_material_detail` | Fetches one Top Ads Insight material detail. | 0.002$ / time |
| `top_ads_library` | Searches Top Ads Library materials with official filters. | 0.002$ / item |
| `top_ads_library_material_detail` | Fetches one Top Ads Library material detail. | 0.002$ / time |

## Output Structure

The Actor returns dataset items. Each item is the raw structured response for the selected `target`, with field names preserved from TikTok One where possible.

**Note**: The following examples are simplified. Actual output can contain more fields depending on TikTok One's current response.

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

### Insight Lists

`top_ads_insight_creative_approach` returns `formulaList`. `top_ads_insight_selling_point_analysis` and `top_ads_insight_top20_selling_points` return `SellingPoints`. The material-list targets return `itemList` and pagination fields.

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

Both `top_ads_insight_material_detail` and `top_ads_library_material_detail` return the detail response for one `materialID`.

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

## Notes

TikTok One runs on `ads.tiktok.com` and requires valid logged-in cookies. Some Top Ads Insight endpoints also require TikTok One browser-side request signatures; the Actor handles those internally, but the account cookie still needs permission to access the official TikTok One pages.

This public repository contains user-facing documentation, images, and option files. The Apify Actor runtime code lives in the private Actor repository.
