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

  One of `top_ads_insight`, `top_ads_insight_creative_approach`, `top_ads_insight_selling_point_analysis`, `top_ads_insight_top20_selling_points`, `top_ads_insight_formula_material_list`, `top_ads_insight_selling_point_material_list`, `top_ads_insight_material_detail`, `top_ads_library`, `top_ads_library_material_detail`.

* **Cookies** `cookies`: (Required) Your TikTok One authentication cookies after logging into TikTok One. Way to obtain:

  ![How to get cookies](imgs/get_cookie.png)

### Top Ads Insight Settings

These settings are used when `Target` is `top_ads_insight`.

* **Industry label** `insight_industry_label`: (Required) Official industry label. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_industry.json)
* **Country code** `insight_country_code`: (Required) Official country or region filter. Use an empty value for `All`. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_country.json)
* **Time range** `insight_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_time_range.json)
* **Sort by** `insight_order_field`: (Required) Ranking metric. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_insight_order_field.json)

### Top Ads Insight Single Targets

Each Top Ads Insight single target has its own industry, country, time range, and sort fields in the Actor input.

* `top_ads_insight_creative_approach`: fetches creative approach formulas.
* `top_ads_insight_selling_point_analysis`: fetches selling point categories.
* `top_ads_insight_top20_selling_points`: requires `insight_top20_category` and fetches selling points under that category.
* `top_ads_insight_formula_material_list`: requires `insight_formula_material_formula_id`, plus `insight_formula_material_page` and `insight_formula_material_limit`.
* `top_ads_insight_selling_point_material_list`: requires `insight_selling_material_selling_point`, plus `insight_selling_material_page` and `insight_selling_material_limit`.

### Top Ads Insight - Material Detail

These settings are used when `Target` is `top_ads_insight_material_detail`.

* **Insight material ID** `insight_detail_material_id`: (Required) Material ID returned by a Top Ads Insight materials target.

### Top Ads Library Settings

These settings are used when `Target` is `top_ads_library`.

* **Industry label** `library_industry_label`: (Optional) Industry label ID. Leave empty for all industries. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_industry.json)
* **Country code** `library_country_code`: (Optional) Country or region code. Leave empty for all countries. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_country.json)
* **Time range** `library_time_range`: (Required) Publication time range. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_time_range.json)
* **Search word** `library_search_word`: (Optional) Search by brand, product, or creative keyword.
* **Objective** `library_objective`: (Optional) Campaign objective. Leave empty for all objectives. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_objective.json)
* **Likes percentile** `library_like_cnt_filter`: (Optional) Like count percentile filter. Leave empty for all percentiles. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_like_cnt_filter.json)
* **Sort by** `library_order_field`: (Required) Metric used to sort results. [Options](https://raw.githubusercontent.com/lofe-w/tiktok-one-scraper-public/refs/heads/main/options/top_ads_library_order_field.json)
* **Page** `library_page`: (Required) Page number.
* **Limit** `library_limit`: (Required) Number of records per page.

### Top Ads Library - Material Detail

These settings are used when `Target` is `top_ads_library_material_detail`.

* **Material ID** `library_material_id`: (Required) Material ID returned by `top_ads_library`.

## Targets

| Target | Description | Cost |
| --- | --- | --- |
| `top_ads_insight` | Fetches the initial Top Ads Insight page data: overview, creative approaches, categories, and selling point rows for the first categories. | 0.002$ / fetched item |
| `top_ads_insight_creative_approach` | Fetches creative approach formulas. | 0.002$ / item |
| `top_ads_insight_selling_point_analysis` | Fetches selling point categories. | 0.002$ / item |
| `top_ads_insight_top20_selling_points` | Fetches selling points under a selected category. | 0.002$ / item |
| `top_ads_insight_formula_material_list` | Fetches Top Ads Insight materials for a formula. | 0.002$ / item |
| `top_ads_insight_selling_point_material_list` | Fetches Top Ads Insight materials for a selling point. | 0.002$ / item |
| `top_ads_insight_material_detail` | Fetches one Top Ads Insight material detail. | 0.002$ / time |
| `top_ads_library` | Searches Top Ads Library materials with official filters. | 0.002$ / item |
| `top_ads_library_material_detail` | Fetches one Top Ads Library material detail. | 0.002$ / time |

## Notes

TikTok One requests require valid logged-in cookies from `ads.tiktok.com`. If cookies are missing, invalid, expired, or rate-limited by TikTok, upstream requests may fail or return authentication errors.

This public repository contains user-facing documentation, images, and option files. The Apify Actor runtime code lives in the private Actor repository.
