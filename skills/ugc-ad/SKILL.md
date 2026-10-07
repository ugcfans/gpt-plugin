---
name: ugc-ad
description: Make a UGC-style ad for a product with UGC Fans, with the cost quoted before anything is made. Use when the user wants a UGC ad, testimonial ad, product video ad or ad creative for a product or a product page.
---

# UGC ad from a product

An ad spends credits from the user's UGC Fans account. Nothing is made until the user has seen the cost and said yes.

## Steps

1. Call `ugcfans_get_account` for the balance. If `admitted` is false, say the account is not enabled yet and stop.
2. Collect what the ad needs: the product, as a `page_url` or as `product` (a sentence, or an object with its `name`, `description` and `images`, which are `out/` addresses of library pictures), and the kind of ad.
3. Call `ugcfans_search_templates` with a short `query` to find the `feature` slug and the `template` slug that fit, and offer the user two or three. Read each entry's `requires` for what it needs.
4. When the ad has a presenter or a voice, `person` and `voice` are ids of saved ones. A presenter needs a portrait and, for a real person, recorded consent, and both are done at https://ugc.fans/library, never through a tool. `ugcfans_save_asset` saves a generated presenter, a stock voice, a brand or a product when the user wants one.
5. Optional. When the user wants the words written first, tell them it spends a few credits, then call `ugcfans_write_script`, wait with `ugcfans_wait_job`, show the script and pass its id to the ad as `script`. The ad takes a script by id.
6. Call `ugcfans_estimate` with `kind: "ad"` and a `body` holding exactly the fields the ad will use, without the quote. Tell the user the most it can cost in `credits` next to their balance, repeat any `warnings`, and handle any `prerequisites` first (a presenter whose portrait is not drawn yet). Then wait for a clear yes to that cost.
7. Call `ugcfans_make_ad` with the same fields and the `quote` from the estimate. Change nothing between the two calls; any change needs a new estimate, and a quote is good for about ten minutes.
   - `status: "needs_quote"` means no quote came or the request changed; show the new cost and ask again.
   - `status: "needs_credits"` follows the credits skill.
8. The call returns a job of kind `ad:<id>`. Call `ugcfans_wait_job` until it finishes. Ads take minutes, so tell the user it is still being made each time you wait, and do not start a second ad meanwhile.
9. On `done`, the result holds `ad` with its `outputs`, each with a `url` and `expires_in` in seconds. Give the user the links and say how long they last. `ugcfans_get_job` reads the same result later.
10. To polish it, use the finish-clip skill on an output's `path` (captions, a reformat, an end card). To edit it as a film, call `ugcfans_ad_to_film` with the ad id. For a public page, call `ugcfans_share_file` with the `path` and `kind: "showcase"`.

## Rules

- A testimonial's `testimonial_basis` is `verbatim` or `edited` only when the user supplies real customer reviews in `reviews`; `composed` means the words are written for the ad, and the user should be told so. Never invent reviews, ratings, results or endorsements.
- Never start the same ad twice to retry. A `failure.code` of `uncertain` means the job stopped before it finished and is not sent again automatically; ask the user before making it again.
- A failed job carries `failure.message`; pass it on in one sentence and say what was not made.
- To stop a job the user no longer wants, call `ugcfans_cancel_job` with the job id. A cancelled job is not charged, and a job already with a provider cannot be stopped.
