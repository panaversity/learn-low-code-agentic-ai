# n8n Tutorial for Beginners: EASIEST WAY to build your first AI AGENT in 20 Minutes

[The Tutorial is based on this Video: EASIEST WAY to build your first AI AGENT in 20 Minutes (build with me)](https://www.youtube.com/watch?v=EW93sbQUafE)

Here’s a beginner-friendly n8n tutorial that recreates (and expands on) the scheduling agent from the video—filling in all the “but how do I actually wire that?” gaps. Yes, we’ll keep it simple; and no, you don’t need a PhD in flowcharts—just vibes and a few clicks.&#x20;

[Contacts CSV Template](./Contacts_CSV_template.csv)

# What you’ll build (in \~30–45 min)

A chat-driven scheduling agent: you type “book a 30-min call with Laura next Tuesday,” it looks up Laura’s email in a Google Sheet, checks Google Calendar for conflicts, proposes slots if needed, and creates the event when confirmed—no code, just nodes. 

---

# 0) Prereqs you actually need (the video glosses over these)

* **n8n**: Desktop, Cloud, or Docker—any is fine; pick the one you can spell.
* **Google**: A project with **Calendar API** enabled + OAuth credentials; add your account as a test user if your app is in “testing.”
* **Google Sheets**: A simple “Contacts” sheet with columns: `Name, Email, Phone, Notes` (we’ll use this as a lightweight CRM).
* **LLM provider**: OpenAI / Anthropic / Gemini API key. 

> Tip: Share the Contacts sheet with the same Google account you’ll connect in n8n; otherwise your agent will play hide-and-seek with emails.

---

# 1) Create the workflow (the “agent’s house”)

1. **New Workflow →** name it **Appointment Agent** (because “Calendar Wizard 9000” scares clients). Important: In workflow settings, change time zone to Asia/Karachi as by default it is America/New York, otherwise the schedule time of events will be wrong.
2. **Add “Chat Trigger”** (n8n Chat) so you can talk to your agent from the built-in chat panel.
3. **Add “Respond to Chat”** (we’ll connect it later) to stream replies like a friendly barista.

---

# 2) Add the brain (AI Agent) + memory (so it remembers the last thing you said)

1. **Add “AI Agent”** node and connect **Chat Trigger → AI Agent**.
2. **Model**: pick something cheap-and-smart (e.g., GPT-4o-mini / Claude Haiku / Gemini Flash). Your wallet will thank you more than your GPU.
3. **Memory**: enable a short window (e.g., 5 messages). Think goldfish—but one that can schedule meetings.
4. **System prompt**: paste this ready-to-use prompt (or download it):

   * [Download: system prompt](sandbox:/mnt/data/n8n_appointment_agent_system_prompt.txt)
     It includes **role**, **tools**, **planning**, **constraints**, and **output style**—like checklists for a very organized assistant.

> Guardrails matter: the prompt enforces business hours, a 15-min buffer, and “ask before booking” if details are fuzzy—because confident wrong is still wrong.

---

# 3) Wire the tools (this is where n8n shines)

You’ll give the AI Agent three tools it can call by name:

### A) Lookup contacts in Google Sheets

* **Node**: *Google Sheets* → operation **Lookup** (range: `Contacts!A:D`).
* Purpose: when the user says “Laura,” the agent gets `laura@example.com`.
* **Credential**: connect your Google account; select the spreadsheet.

> Pro tip: normalize names/emails—two “Lauras” become chaos faster than a group chat.

### B) List events in Google Calendar (conflict checks)

* **Node**: *Google Calendar* → operation **Get All** (`timeMin`, `timeMax`).
* Purpose: confirm the time window is free (or suggest alternatives).

> Add a little “buffer math” in your agent prompt to avoid back-to-backs—your future neck thanks you.

### C) Create event in Google Calendar

* **Node**: *Google Calendar* → operation **Create** (map `start`, `end`, `summary`, `attendees`).
* Purpose: actually book the meeting and return the event link.

> Add you + invitee(s) as attendees so everyone gets a calendar email; robots still send nice invites.

### D) Tell the AI Agent about these tools

In **AI Agent → Tools**, add:

* `get_contact` → **Google Sheets: Lookup Contact**
* `list_events` → **Google Calendar: List Events**
* `create_event` → **Google Calendar: Create Event**
  …and include short descriptions (“lookup a person’s email by name,” etc.). Yes, names matter—like spices in biryani.

---

# 4) Close the loop (reply nicely)

* Connect **AI Agent → Respond to Chat** so confirmations, questions, or proposed slots appear in the chat panel.
* Keep responses short and specific; no one needs a sonnet to say “2pm works.” Unless they do—then we applaud.

---

# 5) Test drive (scripts you can copy)

Try these in the chat:

* “**Book a 30-min call with Laura next Tuesday afternoon.**”
* “**Find time with Alex Chen this Friday morning; 45 minutes.**”
* “**Propose three slots next week with Zia (Pakistan time).**”
  If the agent lacks `Email` for a person, it should ask—like a bouncer for your calendar.

---

# 6) Files you can use right now (no guessing)

* **Starter workflow (JSON)** – import into n8n, then attach your credentials & select your sheet/calendar:
  [Download: n8n\_appointment\_agent\_starter.json](sandbox:/mnt/data/n8n_appointment_agent_starter.json)
* **Contacts CSV template** – open in Sheets and share with your Google account used in n8n:
  [Download: contacts\_template.csv](sandbox:/mnt/data/contacts_template.csv)
* **System prompt (txt)** – copy/paste into the AI Agent “System Prompt”:
  [Download: n8n\_appointment\_agent\_system\_prompt.txt](sandbox:/mnt/data/n8n_appointment_agent_system_prompt.txt)

> After importing the workflow, you still need to pick your **Google** and **LLM** credentials in each node—otherwise the bot will meditate instead of schedule.

---

# 7) “Gaps” the video didn’t detail (so you don’t get stuck)

**A. Google OAuth that actually works**

* In Google Cloud Console: enable **Calendar API** and **Sheets API** → create **OAuth Client ID** (Desktop if you use n8n Desktop; Web if self-hosted with proper redirect).
* In n8n, add Google credentials and complete the OAuth dance; if in “Testing,” add your email as a test user.
* If you see `invalid_grant`, your clock/timezone may be off—scheduling irony at its finest.

**B. Time zones & fuzzy dates**

* Let the agent convert “next Tuesday” to ISO8601 using **your** timezone (set n8n instance TZ correctly).
* In prompt, force explicit confirmation: “Book 14:00–14:30 PKT on Tue, Sep 2?”—humans + timezones = sitcom.

**C. Buffers, business hours, and duration defaults**

* Encode in the **System Prompt**: “work 09:00–17:00, add 15-min buffer, default 30-min duration unless told.”
* Your future self won’t miss lunch again; neither should your agent.

**D. Safe-ops & fallbacks**

* Add an **IF** node after `list_events`: if conflicts > 0, branch to a message proposing 2–3 alternatives, else create the event.
* Use a **Try/Catch** (Error Workflow) for provider hiccups—because APIs nap sometimes.

**E. Data hygiene**

* Validate `Email` from Contacts; if blank or malformed, ask before booking.
* Optional: enrich contacts via a CRM API using HTTP Request—because you’re fancy now.

**F. Logging for humans**

* Write key steps to **Execute Workflow → Last Node → JSON** or a Data Store. You’ll thank the logs when the CEO asks “what happened?”

---

# 8) Going further (a few shiny add-ons)

* **Multi-channel trigger**: add a **Webhook** or **Slack** trigger so leads can DM your agent; fewer tabs, more naps.
* **Email follow-ups**: add **Gmail** node to send a summary/ICS after booking; it’s like a tiny victory parade.
* **Business rules**: block holidays, limit meeting types, or round to :00/:30—because precision is just organized laziness.

---

# 9) Quick troubleshooting (copy-paste saves lives)

* **Agent keeps asking instead of booking** → missing `Email` or `Duration`; add defaults in prompt.
* **No events returned** → wrong calendar picked (“primary” vs specific) or time window off by timezone.
* **Permission errors** → re-auth Google; ensure the sheet is shared with the same account.
* **Spooky duplicates** → check if your CRM also creates events; add a dedupe key in the summary like `[#lead-123]`.

---

# 10) Your first run checklist (print-worthy, unlike most memes)

* Can the agent **find the contact**?
* Does it **see conflicts** within the requested window + 15-min buffer?
* Does it **confirm** the exact ISO8601 start/end with timezone before booking?
* Does it **return the event link** after creating it?
  If all yes: high-five your screen (gently—warranties are fragile).

---

