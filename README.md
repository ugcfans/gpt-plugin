# UGC Fans

Make marketing media from inside your AI assistant. This plugin connects Claude Code, Codex, ChatGPT, Cursor, VS Code and Gemini CLI to the UGC Fans server at https://mcp.ugc.fans/mcp and adds four skills that tell the assistant how to use it: `launch-film`, `ugc-ad`, `finish-clip` and `credits`.

## What it does

- Photographs a website and makes a short launch film from it, in landscape, vertical or square.
- Makes a UGC-style ad for a product from a template, with a script, a person and a voice.
- Makes images and short video clips from a prompt.
- Trims, captions, reformats, brands and transcribes footage you already have in your UGC Fans library, and shares finished files by link.

## What it runs and what it sends

- Nothing runs on your machine. The plugin is Markdown and JSON: one remote server entry and four skills. It has no scripts, hooks, commands or local servers, and it installs no packages.
- Each tool call goes over HTTPS to https://mcp.ugc.fans with the arguments the assistant fills in: the website addresses you ask it to capture, the text for scripts and ads, file paths in your UGC Fans library and the options you choose. UGC Fans fetches and photographs the websites you name.
- Searching the template catalogue and listing models work without an account. Everything else asks you to sign in at ugc.fans through the standard OAuth flow. The plugin holds no key, token or password in any file.

## Credits

Generating images, video and ads spends credits from your UGC Fans account, and each one is quoted before anything is made. Website captures, launch films, clip finishing and transcripts run on UGC Fans' own compute and spend no credits. Plans and credits are described at https://ugc.fans/credits.

## Install

- Claude Code: `claude mcp add --transport http ugcfans https://mcp.ugc.fans/mcp`, or `claude plugin install ugcfans@ugcfans` once the marketplace that lists this folder is added. The plugin bundles the skills as well.
- Codex: `codex mcp add ugcfans --url https://mcp.ugc.fans/mcp`, then `codex mcp login ugcfans`.
- Gemini CLI: `gemini extensions install` with the address of the repository that holds this folder.
- Other clients: add https://mcp.ugc.fans/mcp as a remote Streamable HTTP server.

## License

MIT. See `LICENSE`.
