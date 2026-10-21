# Claude Code on API credits

Run the **native Claude Code TUI** on your **Anthropic API credits** — promo credits,
free-trial credits, or any Console API-key balance — instead of a paid subscription.

Out of the box, Anthropic's API credits only cover the **Agent SDK**. Open the normal
interactive `claude` and it bills the `cli` entrypoint, which fails with
`400 credit balance too low`. This tiny wrapper sets `CLAUDE_CODE_ENTRYPOINT=sdk-cli`,
so the **same TUI you already use** bills your credits instead of your card.

## The whole trick

```sh
CLAUDE_CODE_ENTRYPOINT=sdk-cli claude
```

That's it. This repo wraps that line in a launcher that also cleans your environment so
a subscription token or a proxy's model-routing variables can't leak in and break billing.

## What you get

- Full **native Claude Code**: tools, agents, skills, MCP, `CLAUDE.md`, slash commands.
- Your **API / promo credits** are billed instead of a subscription.
- Every `claude` flag still works: `-p`, `--continue`, `--resume`, `--model`, …
- Permission prompts off by default (`--dangerously-skip-permissions`).

## Install

```sh
git clone https://github.com/Chumbayoumba/claude-code-api-credits.git
cd claude-code-api-credits
install -m 0755 claude-api ~/.local/bin/claude-api
```

(Or just copy the `claude-api` file anywhere on your `$PATH`.)

## Setup — one API key

```sh
export ANTHROPIC_API_KEY="sk-ant-..."                              # option 1: env var
echo "sk-ant-..." > ~/.anthropic_api_key && chmod 600 ~/.anthropic_api_key   # option 2: file
```

Override the file location with `CLAUDE_API_KEY_FILE`.

## Usage

```sh
claude-api                # interactive TUI
claude-api -p "hi"        # one-shot print mode
claude-api --continue     # continue the last session
claude-api --resume <id>  # resume a session by id
```

## How it works

1. Scrubs inherited `ANTHROPIC_*`, `CLAUDE_CODE_*`, and OAuth env vars.
2. Exports `ANTHROPIC_API_KEY` and `CLAUDE_CODE_ENTRYPOINT=sdk-cli`.
3. `exec`s the native `claude` with your arguments.

The `sdk-cli` entrypoint routes billing to your API-credit balance. Prompt caching still
works the way it always does in Claude Code — the system prompt, tools, and history are
cached automatically, which is where most of the input-cost savings come from.

## License

[MIT](LICENSE)
