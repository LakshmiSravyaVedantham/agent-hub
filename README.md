# agent-hub

A Claude Code skill that makes Claude the orchestrator for a fleet of free-tier AI providers. Every task is automatically classified, routed to the best available provider, and tracked against free tier limits.

## Providers

| Provider | Best For | Free Tier | Fallback |
|----------|----------|-----------|----------|
| Groq | Fast Q&A, summaries, general | 14,400 req/day | Gemini |
| Codex (gpt-4o-mini) | Code generation, debugging | 500 req/month | Groq |
| Gemini | Research, long context | 1M tokens/day | MiniMax |
| MiniMax | Creative writing, dialogue | 1M tokens/month | Gemini |

## Install

```bash
SKILL_DIR="$HOME/.claude/plugins/cache/claude-plugins-official/superpowers/5.0.5/skills/agent-hub"
mkdir -p "$SKILL_DIR/tests"
curl -o "$SKILL_DIR/router.py" https://raw.githubusercontent.com/LakshmiSravyaVedantham/agent-hub/main/router.py
curl -o "$SKILL_DIR/SKILL.md" https://raw.githubusercontent.com/LakshmiSravyaVedantham/agent-hub/main/SKILL.md
```

## Setup

```bash
ROUTER="$HOME/.claude/plugins/cache/claude-plugins-official/superpowers/5.0.5/skills/agent-hub/router.py"
python3 $ROUTER set-key groq gsk_...
python3 $ROUTER set-key codex sk-...
python3 $ROUTER set-key gemini AIza...
python3 $ROUTER set-key minimax ...
python3 $ROUTER set-key minimax-group-id ...
```

## Usage

In any Claude Code session:
```
superpowers:agent-hub
```

Or call `router.py` directly:
```bash
python3 $ROUTER route "explain how attention works" --type research
python3 $ROUTER status
python3 $ROUTER reset groq
```

## Status Bar

Displayed before every response:
```
[GROQ ●] Groq: 1,240/14,400 · Codex: 77/500 · Gemini: 108K/1M · MiniMax: 220K/1M
```

Fallback event:
```
[GROQ ●] ⚠ Codex at limit — routing code task to Groq · Groq: 11,200/14,400
```

## Tests

```bash
python3 -m pytest tests/ -v
```

## License

MIT
