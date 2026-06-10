# TikTok One Scraper

[中文 README](https://github.com/lofe-w/tiktok-one-scraper-public/blob/main/README.zh-CN.md)

Scrape TikTok One inspiration and insight data through TikTok One backend APIs. This Actor currently focuses on the official TikTok One Top Ads Insight and Top Ads Library workflows, returning structured JSON for ad research, competitive monitoring, creative analysis, and marketing intelligence.

[Start Now (On Apify)](https://apify.com/doliz/tiktok-one-scraper)

## Key Features

* **Fast API-based extraction**: Calls TikTok One backend APIs directly instead of driving the website UI.
* **Top Ads Insight support**: Fetch overview metrics, creative approaches, selling point analysis, material lists, and material details.
* **Top Ads Library support**: Search the Top Ads Library with official filters and fetch material details.
* **Structured JSON output**: Returns raw TikTok One API responses in a machine-readable format.
* **Public option files**: Common filter values are published in this repository under [`options/`](options/).

## Input Settings

### Common Settings

* **Target** `target`: (Required) Select the TikTok One data source.

  One of `top_ads_insight`, `top_ads_insight_creative_approach`, `top_ads_insight_selling_point_analysis`, `top_ads_insight_material_list`, `top_ads_insight_material_detail`, `top_ads_library`, `top_ads_library_material_detail`.

* **Cookies** `cookies`: (Required) Your TikTok One authentication cookies after logging into TikTok One. Way to obtain:

  ![How to get cookies](imgs/get_cookie.png)

### Top Ads Insight Settings

These settings are used when the `Target` starts with `top_ads_insight`.

* **Industry label** `insight_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_country_code`: (Required) Official country or region filter. Use an empty value for `All regions`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

### Top Ads Insight - Selling Point Analysis

These settings are used when `Target` is `top_ads_insight_selling_point_analysis`.

* **Category** `insight_category`: (Optional) Category returned by `top_ads_insight_selling_point_analysis`. Leave empty to fetch top-level categories.

### Top Ads Insight - Material List

These settings are used when `Target` is `top_ads_insight_material_list`.

* **Formula ID** `insight_formula_id`: (Optional) Formula ID returned by `top_ads_insight_creative_approach`.
* **Selling point** `insight_selling_point`: (Optional) Selling point returned by `top_ads_insight_selling_point_analysis`.
* **Page** `insight_page`: (Required) Page number. Page size is fixed internally.
* **Exclude KA ads** `insight_exclude_ka`: (Optional) Exclude KA ads from the material list.

### Top Ads Insight - Material Detail

These settings are used when `Target` is `top_ads_insight_material_detail`.

* **Insight material ID** `insight_material_id`: (Required) Material ID returned by `top_ads_insight_material_list`.

### Top Ads Library Settings

These settings are used when `Target` is `top_ads_library`.

* **Industry labels** `library_industry_label_list`: (Optional) Industry label IDs. Leave empty for all industries.
* **Country codes** `library_country_code_list`: (Optional) Country or region codes. Leave empty for all countries. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_country.json)
* **Time range** `library_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_time_range.json)
* **Search word** `library_search_word`: (Optional) Search by brand, product, or creative keyword.
* **Objectives** `library_objective_list`: (Optional) Campaign objectives. Leave empty for all objectives. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_objective.json)
* **Ad format** `library_ad_format`: (Optional) Ad format filter. Leave empty for all formats. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_ad_format.json)
* **Likes percentile** `library_like_cnt_filter`: (Optional) Like count percentile filters. Leave empty for all percentiles. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_like_cnt_filter.json)
* **Selling points** `library_selling_point_list`: (Optional) Selling point IDs or names. Leave empty for all selling points.
* **Sort by** `library_order_field`: (Required) Metric used to sort results. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_order_field.json)
* **Page** `library_page`: (Required) Page number. Page size is fixed internally.

### Top Ads Library - Material Detail

These settings are used when `Target` is `top_ads_library_material_detail`.

* **Material ID** `library_material_id`: (Required) Material ID returned by `top_ads_library`.

## Targets

| Target | Description | Cost |
| --- | --- | --- |
| `top_ads_insight` | Fetches the initial Top Ads Insight page data: overview, creative approaches, categories, and selling point rows for the first categories. | 0.002$ / fetched item |
| `top_ads_insight_creative_approach` | Fetches creative approach formulas. | 0.002$ / item |
| `top_ads_insight_selling_point_analysis` | Fetches selling point categories or selling points under a selected category. | 0.002$ / item |
| `top_ads_insight_material_list` | Fetches Top Ads Insight materials for a formula or selling point. | 0.002$ / item |
| `top_ads_insight_material_detail` | Fetches one Top Ads Insight material detail. | 0.002$ / time |
| `top_ads_library` | Searches Top Ads Library materials with official filters. | 0.002$ / item |
| `top_ads_library_material_detail` | Fetches one Top Ads Library material detail. | 0.002$ / time |

## Notes

TikTok One requests require valid logged-in cookies from `ads.tiktok.com`. If cookies are missing, invalid, expired, or rate-limited by TikTok, upstream requests may fail or return authentication errors.

This public repository contains user-facing documentation, images, and option files. The Apify Actor runtime code lives in the private Actor repository.
