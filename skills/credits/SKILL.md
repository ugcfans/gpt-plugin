---
name: credits
description: How UGC Fans credits work and what to say when they run out. Use before any paid generation, when the user asks what something costs or what their balance is, and whenever a tool answers needs_credits or needs_quote.
---

# Credits

Some UGC Fans tools spend credits from the user's account and some spend none.

| Spends no credits | Spends credits |
|---|---|
| `ugcfans_capture_website`, `ugcfans_get_capture`, `ugcfans_make_launch_film`, `ugcfans_make_site_sting`, `ugcfans_ad_to_film`, `ugcfans_set_film_words`, `ugcfans_export_film`, `ugcfans_finish_clip`, `ugcfans_transcribe`, the file and job tools, `ugcfans_save_asset`, `ugcfans_list_models`, `ugcfans_estimate` | `ugcfans_make_image`, `ugcfans_make_video` and `ugcfans_make_ad` (each priced first with a quote), and the smaller `ugcfans_write_script`, `ugcfans_make_speech`, `ugcfans_make_performance` and `ugcfans_read_reference` |

## Estimate first

1. Call `ugcfans_get_account` before the first paid call of a conversation. It returns `credits`, `plan` and `admitted`.
2. For an image, a video or an ad, call `ugcfans_estimate` with `kind` and a `body` holding the fields the make call will use, without the quote. It returns `credits` (the most the request can cost), `quote` (good for about ten minutes), `expires_at`, `warnings` and sometimes `prerequisites`. Model ids and prices for images, video, voice and avatars come from `ugcfans_list_models`.
3. Tell the user the cost next to their balance, in credits, as "up to N", and wait for a clear yes to that cost. Ask again when the cost changes.
4. Call the make tool with the same fields and the `quote`. Any change to the fields needs a new estimate, and so does an expired quote. A make call without a matching quote makes nothing and answers `needs_quote` with the cost.
5. A script, speech, a performance or a reference reading has no quote. Tell the user before calling that it spends credits, and do not call it more than once for one request. `ugcfans_make_speech` prices the line and, when it can, makes it in the same call.
6. When the user asks how to spend less, offer a cheaper model from `ugcfans_list_models` or a smaller job. A model marked `locked` is not available on the account's current plan; say so and do not try it.

## When credits run out

A tool answers `status: "needs_credits"` with `have`, `need` and `plans`. Then:

1. Say plainly that this needs more credits than the account has, with the numbers: "This needs 40 credits and the account has 12." When `need` is null, give `have` alone.
2. Say that plans and credits are described at https://ugc.fans/pricing and that credits are bought on ugc.fans. Give that link once.
3. Say what was not made, and offer what still works: the tools that spend no credits, or a smaller job.
4. Stop there. Do not retry the call, do not describe or compare plans, do not recommend buying, and never link to a checkout or payment page.

A tool that refuses a locked model says the model is part of a plan; treat it the same way: one sentence, the link once, nothing more.
