# claude-glm-usage-check

CLI tools to check subscription usage for Claude Code and Z.ai GLM.

**ccusage** — Anthropic Claude Code (Pro/Max) session and weekly quota
**glmusage** — Z.ai GLM Coding Plan (Lite/Pro/Max) usage

No dependencies. Python 3.6+.

## Install

```bash
# Clone and symlink into your PATH
git clone https://github.com/eyehol3/claude-glm-usage-check.git
ln -s "$(pwd)/claude-glm-usage-check/ccusage" ~/.local/bin/ccusage
ln -s "$(pwd)/claude-glm-usage-check/glmusage" ~/.local/bin/glmusage
```

## ccusage

Reads your Claude Code OAuth token from the macOS Keychain or `~/.claude/.credentials.json`.

```
$ ccusage
  Claude Code Usage  [PRO]
  ──────────────────────────────────────────────────────────

  Current session
  [██████████████████████████████████████████████████] 100% used
  Resets 2h 12m

  Current week (all models)
  [███████████████████████████░░░░░░░░░░░░░░░░░░░░░░░] 55% used
  Resets 5d 0h

  Extra usage
  Not enabled
```

```
$ ccusage -r
45
```

## glmusage

Requires a Z.ai API key. Create one at [z.ai/manage-apikey](https://z.ai/manage-apikey), then save it:

```bash
mkdir -p ~/.config
cat > ~/.config/model-proxy.env << 'EOF'
Z_AI_API_KEY=your-key-here
EOF
```

Or export it: `export Z_AI_API_KEY=your-key-here`

```
$ glmusage
  Z.ai GLM Usage  [LITE]
  ────────────────────────────────────────

  Monthly Web Search / Reader / Zread Quota
  [░░░░░░░░░░░░░░░░░░░░] 1%
  resets in 29d 20h

  5 Hours Quota
  [█████████░░░░░░░░░░░] 45%
  resets in 1h 36m

  Weekly Quota
  [█░░░░░░░░░░░░░░░░░░░] 9%
  resets in 6d 20h
```

```
$ glmusage -r
55
```

## Pipeline usage

Use `-r` / `--remaining` to get a single number (0-100) for scripts:

```bash
# Only run if there's at least 20% session quota left
if [ "$(ccusage -r)" -gt 20 ]; then
  claude -p "do the thing"
fi

# Same for Z.ai
if [ "$(glmusage -r)" -gt 30 ]; then
  echo "enough quota, launching task..."
fi
```

Exit code is 0 on success, 1 on auth/API error, 2 if quota data is missing.

## License

MIT
