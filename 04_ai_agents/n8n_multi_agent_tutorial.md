# n8n Multi‑Agent Tutorial: Multi‑Agent Newsletter Automation (Beginner‑Friendly)

[The Tutorial is based on this Video: Watch Me Build a Multi-Agent Newsletter System in n8n (step-by-step)](https://www.youtube.com/watch?v=pxzo2lXhWJE)

> Build a 3‑agent workflow in n8n that researches, drafts, and formats a newsletter, then creates a Gmail draft for human review. We’ll follow the video’s flow **and** fill in the missing pieces (prompts, schemas, expressions, error‑handling, hosting, and security). Yes, like adding fries to a perfectly good burger 🍟.

---

## What You’ll Build (at a glance)
A weekly workflow that:
1) **Researches** recent news for a niche → 2) **Plans** the edition title + topics (Agent 1) → 3) **Deep‑researches** each topic → 4) **Writes** three sections (Agent 2) → 5) **Aggregates** sections → 6) **Edits & styles** into responsive HTML (Agent 3) → 7) **Creates a Gmail draft**. It’s linear, like a conga line—only with fewer awkward hips. 💃

```
Schedule Trigger
  → Tavily Search (initial research)
    → Agent 1: Planning (title + 3 topics, structured output)
      → Split Out (topics → 3 items)
        → Tavily Search (per topic, include raw content)
          → Agent 2: Section Writer (writes 3 sections)
            → Aggregate (3 → 1)
              → Agent 3: Editor (HTML + subject, structured output)
                → Gmail: Create Draft (HTML)
```

---

## Prerequisites (the stuff the video zooms past)
- **n8n**: Cloud or self‑host.
  - **Enable Community Nodes**: *Settings → Community Nodes → Allow installation from community*. No community, no party 🎉.
- **Credentials** you’ll need:
  - **Tavily** API key (free tier available). Used for web search. Like a librarian who doesn’t shush you.
  - **GEMINI** API key (or your preferred model provider). Used by all AI agents.
  - **Gmail** (OAuth) to create a **Draft**. It’s like sending an email… but with commitment issues.
- **Optional**: A test inbox, e.g., a secondary Gmail account.

**Self‑hosting tip (Docker)**
```bash
docker run -it --rm \
  -p 5678:5678 \
  -e N8N_ENCRYPTION_KEY="change_me" \
  -e N8N_SECURE_COOKIE=true \
  -e N8N_PORT=5678 \
  -e WEBHOOK_URL="https://your-domain" \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n:latest
```
Set credentials inside n8n. Avoid hard‑coding keys—secrets hate attention. 🤫

---

## Step‑by‑Step Build


### 1. Schedule Trigger

- **Node:** `Schedule Trigger`
- **Purpose:** Starts the workflow every week.
- **Every**: 1 week; **On**: Sunday; **Time**: 00:00 (adjust to your timezone)
- **Notes:** Leave **Active** off while building. Like removing the car keys before opening the hood. 🧰

---

### 2. Initial Research (Tavily)
- **Node**: *Tavily → Search*
- **Query**: your niche (e.g., `AI adoption for small businesses`)
- **Options**:
  - *topic*: `news`
  - *time_range*: `week`
  - *max_results*: `3`
  - *include_raw_content*: `false` (we’ll enable raw later)
- **Rename**: `Initial Research`
- **Tip**: *Pin Data* after a good run to speed up testing. Your CPU will send a thank‑you note. 💌
---

### 3. Agent 1 — Planning Agent (Title + Topics)

- **Node:** `AI Agent` (rename to `Planning Agent`)
- **Purpose:** Generates the newsletter edition **title** and exactly **3 topics**.
- **Chat Model:** `models/gemini-2.5-pro` (or your provider/model)
- **User Prompt:**

```text
Here are recent articles to consider:

{{ $json.results.map(item => JSON.stringify(item, null, 2)).join("\n\n") }}
```

- **System Prompt:**

```text
You are an expert newsletter planner. You receive 3–5 short article digests (title, URL, published date, content) from the past week.

Task: 
propose a catchy edition title (≤ 80 chars) and exactly 3 concise topics (each 3–5 words) that reflect distinct angles for our audience of small-business leaders.

Constraints: unique topics; no duplicates; no clickbait; be informative.
Output only via the required schema (no extra text).
```

- **Require specific output format**: *ON* → **Structured Output Parser** → *Define using JSON Schema*:

```json
{
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "title": { "type": "string", "minLength": 10 },
    "topics": {
      "type": "array",
      "minItems": 3,
      "maxItems": 3,
      "items": { "type": "string", "minLength": 3, "maxLength": 50 }
    }
  },
  "required": ["title", "topics"]
}
```

- Pin Data after you like the output. Like bookmarking a great pizza place. 🍕

---

### 4. Split Out (Topics)

- **Node**: *Item Lists → Split Out Items*
- **Purpose:** Turn the 3 topics into three separate items so each can be researched/written independently.
- **Source/Field to split:** `{{$node["Planning Agent"].json["topics"]}}` (or `output.topics` depending on your agent node output)
- **Result:** 3 items, one per topic.

---

### 5. Deeper Research per Topic (Tavily Search)

- **Node:** `Tavily → Search` (rename to `Topic Research`)
- **Purpose:** Run deeper searches for each topic item to gather sources and raw content.
- **Query:** set to the current item (e.g., `={{ $json }}` or `={{ $json.query }}` depending on the split node's output)
- **Options:**
  - `time_range`: `"month"`
  - `max_results`: `5`
  - `include_raw_content`: `true`
- Pin Data after a healthy run. It’s your workflow’s multivitamin. 💊

---

### 6. Agent 2 — Section Writer (3 passes)

- **Node:** `AI Agent` (rename to `Section Writer`)
- **Purpose:** Using the topic-specific research, write a single, standalone newsletter section (one per topic).
- **Chat Model:** `models/gemini-2.5-pro` (use a cost-efficient model here)
- **User Prompt:**

```text
Topic: {{ $json.query }}

Use these sources to write one standalone section:

{{ $json.results.map(item => JSON.stringify(item,null,2)).join("\n\n") }}
```

- **System Prompt:**

```text
You are a professional newsletter section writer. Write ONE self-contained section (350–500 words) for small-business leaders.

Requirements:
- Start with an H2 heading matching the Topic (e.g., “## <topic>”).
- Synthesize facts from the provided sources (don’t invent).
- Inline-cite claims with superscript markers like [1], [2].
- After the prose, add a short “Sources” list: [n] Title — Domain (linked).
- Tone: clear, expert, engaging. No overall intro or conclusion; just this section.
- Output plain Markdown (the editor will convert to HTML).
```

- **Notes:** This node will run once per topic (3 executions total) and should return a Markdown string per item.

---

### 7. Aggregate Sections (3 → 1)

- **Node**: *Aggregate*
- **Combine**: field `output` (or the agent’s text field) into an array
- **Result**: one item with `sections: [sec1, sec2, sec3]`
- **Rename**: `Sections`
- Your sections are now roommates. Hopefully tidy ones. 🧹

---

### 8. Agent 3 — Editor (HTML + Subject via structured output)

- **Node:** `AI Agent` (rename to `Editor`)
- **Purpose:** Convert the collected sections into final HTML and produce an email subject and body.
- **Chat Model:** a stronger model such as `models/gemini-2.5-pro` or a higher-quality provider model for layout/styling
- **User Prompt:**

```text
Ttile: {{ $('Planning Agent').item.json.output.title }}

Sections: 
{{ $json.output.join("\n\n\n\n") }}
```

- **System Prompt:**

```text
You are the newsletter editor and layout stylist.

**Input:** one title candidate and 3 Markdown sections with [n] footnotes.

**Output:**
1. **subject** — an email subject (≤ 80 chars, no emojis).
2. **content** — a VALID, responsive HTML **body only** (no `<!DOCTYPE>`, `<html>`, or `<head>`).

**HTML Requirements:**
* Structure: include a header with the title + current date {{ $now.format("DDD") }}, a short intro paragraph, the 3 sections (convert Markdown to HTML), a “Key Sources” section that deduplicates and numbers links, and a short friendly sign-off.
* Typography: use `<h1>/<h2>`, `<p>`, `<ul>/<ol>`, `<a>`. Apply inline CSS for readability (max-width container, readable font size and line-height).
* Links: anchors must include `rel="noopener noreferrer"` and `target="_blank"`.
* Accessibility: semantic headings; include `alt` text if images appear (don’t invent images).
* Restrictions: no external CSS/JS, no tracking pixels.
```

- **Require specific output format**: *ON* → **Structured Output Parser** → *Define using JSON Schema*:

```json
{
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "subject": {
      "type": "string",
      "description": "the email subject"
    },
    "content": {
      "type": "string",
      "description": "the newsletter content"
    }
  },
  "required": ["subject", "content"]
}
```

- **Notes:** The Editor should return a single item with `output.subject` and `output.content` (HTML body). Pin the output while fine-tuning.

---

### 9. Create a Draft (Gmail)

- **Node:** `Gmail → Create Draft` (rename to `Create Draft`)
- **Purpose:** Save the final HTML newsletter to Gmail as a draft for human review.
- **Config:**
  - **To:** your testing inbox (or leave blank for drafts)
  - **Subject:** `={{ $json.output.subject }}` (map from Editor node)
  - **Email type:** `html`
  - **Message:** `={{ $json.output.content }}`

---

## Production Hardening (filling the gaps)

### A) Error Handling & Fallbacks (because the internet sometimes naps)
- **IF (Function)**: After each Tavily call, check `results.length`.
  - If `0`, branch to a fallback provider (e.g., SERP API, Perplexity, Bing Web Search) via another node; merge back.
- **Error Trigger**: Add an *Error Trigger* workflow that logs to Google Sheets and pings Slack/Email.
- **Wait & Retry**: Wrap flaky nodes with *Wait* → *Move On* → *IF retried < N* → *Run again*. Like Groundhog Day, but with APIs. ⏳

### B) Cost Control & Guardrails
- Use a cheaper model for Agents 1 & 2; keep the stronger model only for the Editor.
- Lower temperature (e.g., 0.3–0.5) for consistency; set max tokens.
- Limit Tavily results to 3–5; keep raw content only for the topic research step.
- Schedule during off‑hours to avoid rate caps; staggering is your friend.

### C) Logging & Human‑in‑the‑Loop (HITL)
- **Google Sheets** (or DB): Log `run_id`, `title`, `topics[]`, section headings, token counts, and status.
- **Approval**: Keep “Create Draft” only (no auto‑send). For approval, add a simple **Webhook** that, when called, flips a Google Sheets cell from `PENDING` → `APPROVED` and triggers a separate “Send Mail” workflow. Humans keep the crown 👑.

### D) Prompt & Schema Management
- Store long prompts in a single *Code* or *Set* node and reference via expressions.
- Version prompts (v1, v2) with a sheet or env var (`PROMPT_VERSION`).
- Unit test prompts by feeding fixed inputs (use pinned data). Test like you mean it. 🧪

### E) Security & Secrets
- Use **Credentials** for API keys. Do NOT paste keys into nodes.
- Rotate keys; least‑privilege OAuth scopes for Gmail.
- If self‑hosting, set `N8N_ENCRYPTION_KEY` and use HTTPS. Secret agents love encryption. 🕵️‍♀️

---

## Troubleshooting Quickies
- **Agent output in one blob?** Ensure Structured Output Parser is ON where you need separate fields.
- **Weird `[object Object]` in messages?** Pre‑format arrays using a Code node and pass a string.
- **HTML shows `<p>` tags as text?** Set Gmail node **Email Type** to **HTML**.
- **Timeouts?** Reduce Tavily results, shorten raw excerpts, choose faster models, or pin while iterating.

---

## Optional Extensions
- Swap Tavily with Perplexity/SerpAPI/Bing and **merge** results for diversity.
- Add a *Policy/Lint Agent* that checks for claims without sources before the Editor. It’s your newsroom’s grumpy copy desk. 📝
- Post the HTML to your CMS or Mailchimp; attach a plaintext version too.

---

## Copy‑Paste Assets (ready to use)

### JSON Schema — Planning Agent
```json
{
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "title": { "type": "string", "minLength": 10 },
    "topics": {
      "type": "array",
      "minItems": 3,
      "maxItems": 3,
      "items": { "type": "string", "minLength": 3, "maxLength": 50 }
    }
  },
  "required": ["title", "topics"]
}
```

### JSON Schema — Editor Agent
```json
{
  "type": "object",
  "additionalProperties": false,
  "properties": {
    "subject": {
      "type": "string",
      "description": "the email subject"
    },
    "content": {
      "type": "string",
      "description": "the newsletter content"
    }
  },
  "required": ["subject", "content"]
}
```

### System Prompts (Planning / Section / Editor)
**Planning**
```
You are an expert newsletter planner... (see Agent 1 above)
```
**Section Writer**
```
You are a professional newsletter section writer... (see Agent 2 above)
```
**Editor**
```
You are the newsletter editor and layout stylist... (see Agent 3 above)
```

### Expressions / Joins
- Join sections for the Editor:
```
{{ $json.output.join("\n\n\n\n") }}   (Map from Sections node)
```
- Pull title from Planning:
```
{{$node["Planning Agent"].json["title"]}}
```

---

## Next Steps
1) Duplicate the workflow for other niches.
2) Add approval & send flow.
3) Log runs + feedback to evolve prompts.

Congrats—you just built a practical multi‑agent system in n8n. High‑five your future self for the hours you’ll save ✋.
