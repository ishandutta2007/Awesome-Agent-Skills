---
name: curlshot
description: Captures web pages as PNG, JPEG or WebP screenshots, PDFs, or scrolling videos through the CurlShot screenshot API, and renders raw HTML or Markdown to an image or PDF. Use when a task needs a screenshot, full-page capture, mobile or dark-mode view, PDF, or video of a public URL, or an image of generated HTML. Needs a CurlShot access key in CURLSHOT_API_KEY.
---

# CurlShot Screenshots

## Overview

CurlShot renders a page in a real browser and returns the result as an image (PNG, JPEG, WebP), a PDF, or a video of the page scrolling (MP4, WebM, GIF). One HTTP request does it: `GET https://curlshot.com/api/v1/screenshot?url=…` for a public URL, or a `POST` with a JSON body for raw HTML or Markdown. Agents with MCP support can use the hosted server `https://curlshot.com/api/mcp` (tools `take_screenshot`, `list_devices`) instead of curl.

## When to Use

- Use when the user wants a screenshot or full-page capture of a public URL.
- Use when the user wants a page as it looks on a phone or tablet preset, at a given window size, or in dark mode.
- Use when the user wants a page saved as a PDF or as a scrolling video.
- Use when generated HTML or Markdown (a social card, a report, a local page) must be rendered to an image or PDF.
- Do not use for localhost or private network URLs by address: the API refuses them. Send the page's HTML instead.

## Process

1. Check the key without printing it: `test -n "$CURLSHOT_API_KEY"`. If it is missing, ask the user for one. A free key (100 screenshots a month, no card) comes from https://curlshot.com/register.
2. Capture a URL. Send the key in the `X-Access-Key` header, never in a link you show the user:

   ```bash
   curl -sS -G "https://curlshot.com/api/v1/screenshot" \
     -H "X-Access-Key: $CURLSHOT_API_KEY" \
     --data-urlencode "url=https://example.com" \
     -d format=png -d full_page=true -d block_cookie_banners=true \
     -o shot.png -w '%{http_code}\n'
   ```

3. Add options as extra `-d name=value` pairs: `viewport_device=<preset>` (list: https://curlshot.com/docs/devices.md), `viewport_width`/`viewport_height`, `dark_mode=true`, `selector=<css>`, `format=jpeg|webp|pdf|mp4|webm|gif`, `block_ads=true`, `wait_until=networkidle`, `delay=<seconds>`, `cache=true`. Every option: https://curlshot.com/docs/options.md
4. Render raw HTML or Markdown with a POST:

   ```bash
   curl -sS -X POST "https://curlshot.com/api/v1/screenshot" \
     -H "X-Access-Key: $CURLSHOT_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"html": "<h1>Hello</h1>", "format": "png", "viewport_width": 1200, "viewport_height": 630}' \
     -o card.png
   ```

   Use `"markdown": "..."` instead of `html` for Markdown, and `"format": "pdf"` for a PDF.
5. Check the result. HTTP `200` with an `image/*`, `video/*` or `application/pdf` content type means success. Any status of `400` or above returns a JSON error body. Branch on its `error_code`:
   - `400`/`413`/`422` (`invalid_options`, `invalid_url`, `selector_not_found`, ...): do not retry. Fix the request using the `errors` field.
   - `401`: the key is missing or invalid. `402 quota_exceeded`: tell the user and show `upgrade_url`. `403 host_not_allowed`: send HTML instead of a private URL.
   - `429`: wait `Retry-After` seconds, then retry once. `5xx`: retry once. For `timeout`, drop `wait_until=networkidle` or lower `delay`.
   Every code: https://curlshot.com/docs/errors.md

## Common Rationalizations

| Rationalization | Reality |
| --- | --- |
| "curl exited 0, so the file is an image." | A failed render writes a JSON error into the output file. Check the HTTP status and content type first. |
| "Putting the key in the URL is simpler." | Links get logged and shared. Use the `X-Access-Key` header. |
| "Retrying will fix a 400." | `4xx` errors are deterministic. Read `errors` and fix the request. |
| "`localhost` will work if I try again." | Private addresses are always refused. POST the HTML instead. |
| "A misspelled option will just be ignored." | Unknown options are rejected with `invalid_options` and a hint. Use the suggested name. |

## Red Flags

- The access key appears in output, a committed file, or a URL shown to the user.
- The saved `.png` or `.pdf` is a few hundred bytes of JSON.
- A loop retries on `4xx` errors or ignores `Retry-After`.
- The capture targets a private address (localhost, 192.168.x.x) by URL instead of sending the page's HTML.

## Verification

- [ ] `CURLSHOT_API_KEY` was checked without printing its value.
- [ ] The HTTP status was `200` and the content type matched the requested format.
- [ ] The output file opens as the expected format and was saved where the user asked.
- [ ] Any error was handled according to its `error_code`, with at most one retry for `429` or `5xx`.
