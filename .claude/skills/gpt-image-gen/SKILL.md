---
name: gpt-image-gen
description: Generate an image via the OpenAI Images API (model gpt-image-2). Takes a prompt and an output path, writes a PNG file. Reads OPENAI_API_KEY from .env at the project root. Use when an agent (typically yuval) needs to produce an image from a text description.
---

# gpt-image-gen — OpenAI Images API wrapper

Generate a single PNG image from a text prompt using OpenAI's Images API.

## Model

**`gpt-image-2`** — released by OpenAI on 2026-04-21.

> [!warning] Do NOT substitute the model name
> If a call fails, the problem is almost always the API key or the JSON parameters — **not** the model. Do not swap `gpt-image-2` for `dall-e-3`, `gpt-image-1`, or any other variant.

## Required environment variable

`OPENAI_API_KEY` — defined in `.env` at the project root. Load it before calling:

```bash
set -a; source .env; set +a
```

## Primary call — bash with `jq`

```bash
PROMPT="<the prompt text in English>"
OUT="yuval/outputs/<YYYY-MM-DD>-<slug>.png"

RESP=$(curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n --arg p "$PROMPT" '{
    model: "gpt-image-2",
    prompt: $p,
    size: "1024x1024",
    quality: "medium",
    output_format: "png"
  }')")

# Surface API errors before trying to decode
if echo "$RESP" | jq -e '.error' >/dev/null 2>&1; then
  echo "API error:"; echo "$RESP" | jq '.error'; exit 1
fi

echo "$RESP" | jq -r '.data[0].b64_json' | base64 --decode > "$OUT"

# Verify
[ -s "$OUT" ] && echo "ok: $OUT ($(wc -c < "$OUT") bytes)" || { echo "FAILED: empty file"; exit 1; }
```

## Fallback — Python (when `jq` is unavailable, e.g. Git Bash on Windows)

```bash
PROMPT="<the prompt text in English>"
OUT="yuval/outputs/<YYYY-MM-DD>-<slug>.png"

PROMPT="$PROMPT" OUT="$OUT" python3 <<'PY'
import os, json, base64, sys, urllib.request, urllib.error

body = json.dumps({
    "model": "gpt-image-2",
    "prompt": os.environ["PROMPT"],
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png",
}).encode()

req = urllib.request.Request(
    "https://api.openai.com/v1/images/generations",
    method="POST",
    headers={
        "Authorization": f"Bearer {os.environ['OPENAI_API_KEY']}",
        "Content-Type": "application/json",
    },
    data=body,
)

try:
    with urllib.request.urlopen(req) as r:
        resp = json.loads(r.read())
except urllib.error.HTTPError as e:
    print("API error:", e.read().decode()); sys.exit(1)

with open(os.environ["OUT"], "wb") as f:
    f.write(base64.b64decode(resp["data"][0]["b64_json"]))

print(f"ok: {os.environ['OUT']} ({os.path.getsize(os.environ['OUT'])} bytes)")
PY
```

## Parameters

| Field | Required | Notes |
|---|---|---|
| `model` | yes | `gpt-image-2`. Do not change. |
| `prompt` | yes | English text. Describe subject, action, style, composition, lighting. |
| `size` | yes | `1024x1024` (default), `1024x1792` (portrait), `1792x1024` (landscape) |
| `quality` | yes | `low` / `medium` / `high` — cost scales with quality |
| `output_format` | yes | `png` (we use png for transparency support) |

## Error handling

Common HTTP statuses returned by the API:

| Status | Cause | Fix |
|---|---|---|
| 401 | Missing/bad `OPENAI_API_KEY` | `echo "${OPENAI_API_KEY:0:8}..."` — confirm it's set and starts with `sk-` |
| 400 | Malformed JSON or invalid `size`/`quality` | Re-check the JSON payload, especially shell escaping in `-d` |
| 429 | Rate limit | Wait, then retry once |
| 5xx | OpenAI outage | Wait and retry |

If you see a "model not found" error: **inspect the request body first** (a typo or wrong field is the usual cause). The model is correct.

## Conventions

- Output path is the caller's responsibility (use `yuval/outputs/<date>-<slug>.png` by convention).
- The caller should also write a sibling `.txt` with the exact prompt used, for iteration and traceability.
- One image per call (the API supports `n` > 1 but we stick to one for cost predictability).
