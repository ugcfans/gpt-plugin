---
name: finish-clip
description: Finish the user's own footage with UGC Fans. Use when the user wants a clip trimmed, captioned, reformatted for Reels, TikTok, Shorts or a square feed, branded with a logo, given an end card or music, or transcribed.
---

# Finish a clip

Trim, reformat, captions, logo overlay, end card and music run on footage already in the user's UGC Fans library. They run on UGC Fans' own compute and spend no credits.

There is no upload tool. Footage goes in at https://ugc.fans/library first; when the clip is not there yet, ask the user to upload it and tell you when it is done.

## The operations

Each goes in the `operations` list of `ugcfans_finish_clip`; one call takes up to six, each run on the file the one before made.

- `trim`: `end` in seconds, and `start` in seconds (default 0).
- `reformat`: `aspect_ratio` of `9:16` for Reels, TikTok and Shorts, `1:1` for a square feed, `4:5` for a portrait feed, `16:9` for landscape. `mode` is `crop` by default, `pad` keeps the whole picture, `follow` follows the subject.
- `mix`: `music`, the `out/` address of a music file in the library. Ask the user to confirm they have the right to use it; the tool takes the request as that confirmation.
- `captions`: burns in words from `transcript`, the id `ugcfans_transcribe` returned, or from `cues`, lines with `start`, `end` and `text`. A transcript is timed to the exact file it was made from, so it must come from the file the captions run on.
- `overlay`: puts a saved brand's logo in the top-right corner; `brand` is the id of a saved brand and is required. Skip it when the user has no saved brand.
- `end_card`: closes the clip with a card; `headline` is required, `subline` and `brand` are optional.

## Steps

1. Call `ugcfans_list_files` (optional `prefix` and `limit`) and let the user pick the file. Use its `path` exactly as returned; it starts with `out/`.
2. Agree the list of operations with the user and run only what was asked.
3. Run the picture and sound operations first, in this order: `trim`, `reformat`, `mix`. Call `ugcfans_finish_clip` with `source` and those operations.
4. When captions are wanted, transcribe the file they will run on: the original when nothing ran before, otherwise the finished output of step 3. Call `ugcfans_transcribe` with it as `source` and wait with `ugcfans_wait_job`; the finished result carries `transcript` (the id), `text`, `language` and `duration`. Then call `ugcfans_finish_clip` on that same file with `captions`, then `overlay` and `end_card` when asked.
5. When the user only wants to know what is said, to find cut points or to write a summary or a post, stop after the transcript and report `text`.
6. A call that runs past four minutes returns the job it is on, `operations_done` and `remaining_operations`. Call `ugcfans_wait_job` on that job until it is done, then call `ugcfans_finish_clip` again with `remaining_operations` as the operations and the finished file's `path` as `source`.
7. The finished result lists the file in `result.files` with a `url` and `expires_in` in seconds; give the user the link and say how long it lasts. For a later link, or a public page, call `ugcfans_share_file` with the `path`, and `kind: "showcase"` for the page.

## Rules

- Words in captions and transcripts come from the clip. Do not rewrite what the speaker said.
- A failed job carries `failure.message`; pass it on in one sentence. A `failure.code` of `uncertain` means it stopped before it finished and is not sent again automatically; ask before making it again.
- To stop a job the user no longer wants, call `ugcfans_cancel_job` with the job id.
