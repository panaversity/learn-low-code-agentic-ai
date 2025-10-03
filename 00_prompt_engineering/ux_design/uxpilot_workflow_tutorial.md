# The UX Pilot End-to-End Workflow: From Idea to Production-Ready UI

[Watch: Design An App With AI | UX Pilot Tips & Tricks](https://www.youtube.com/watch?v=mMMRKwGitGw)

This tutorial distills the complete process demonstrated in the provided video into a professional, step-by-step guide. You’ll go from a blank canvas to a documented, high-fidelity mobile app—entirely inside the UX Pilot ecosystem (FigJam/Figma + UX Pilot Web).

---

## What You’ll Achieve

* Generate and refine an app concept with AI.
* Expand ideas into concrete features and user-facing requirements.
* Auto-build a comprehensive flowchart diagram from your features.
* Spin up a ready-to-run UX workshop (agenda, activities, email, and facilitator notes).
* (Optional) Produce wireframes from your diagram.
* Generate high-fidelity, multi-screen UI flows with style controls.
* Edit UI with natural-language prompts (global and section-level).
* Preview, document prompt history, export to Figma, and derive technical requirements—including a foundation for your design system.

---

## Prerequisites

* **Figma account** (FigJam enabled).
* **UX Pilot**:

  * FigJam/Figma plugin installed (`Plugins → Manage Plugins → Search “UX Pilot”`).
  * Access to **UX Pilot Web** (for hi-fi screen generation and editing).

> Tip: Ideation prompts inside the FigJam plugin’s **AI Tools → Custom Prompt** do not consume UX Pilot tokens (as described in the video), making it ideal for early exploration.

---

## Phase 1 — Ideate in FigJam (No Tokens)

1. **Start a FigJam board** → `Plugins → UX Pilot`.
2. **Open AI Tools** (panel end-tab) → **Custom Prompt**.
3. On a sticky note, write a short brief, e.g.:

   * *“Give me ideas for a UI/UX project.”*
4. **Select the sticky** → paste the same prompt into **Custom Prompt** → run.
5. Review the generated list; **choose one concept** to pursue.

   * Example chosen in the video: **Travel Itinerary Builder**.

**Best practices**

* Keep problem statements short.
* Use nouns and verbs that imply the platform early (e.g., “mobile app” vs. “web app”).

---

## Phase 2 — Generate Feature Candidates

1. On a sticky with your chosen concept, run:

   * *“Give me features for this **mobile** app.”*
   * (Include platform type: mobile, web, desktop—this affects the feature set.)
2. Triage the output:

   * **Keep** what aligns with your goals.
   * **Delete** or reword features that don’t.
   * **Group** related features; normalize titles.

**Checklist**

* Feature titles: short, consistent (“Trip Planning,” “Document Vault,” “Invite Collaborators”).
* Scope: note MVP vs. V2 candidates.

---

## Phase 3 — Expand Requirements (User-Facing Detail)

1. **Select all feature stickies**.
2. Run a clarifying prompt:

   * *“Give me details for each feature—how it works from the **user’s** side.”*
3. UX Pilot adds a new sticky detailing each feature.
4. **Refactor**: move details under their feature headings; simplify phrasing.

**Outcome**
A clear, user-centric requirements set that stakeholders can understand quickly.

---

## Phase 4 — Create a Flowchart Diagram

1. **Select all features and notes** you want modeled.
2. Go to the **Diagram** tab in the plugin.
3. Click **Generate Diagram** (the **top** button uses your selection).

   * (The bottom button generates from a prompt—use the top button here.)
4. Inspect the diagram:

   * Confirm each feature appears.
   * Verify sequences (e.g., *Save Travel Documents* lists document types; *Invite Friends* shows the invitation process).

**Common pitfall**
If the diagram feels off, you likely:

* Didn’t select everything, or
* Used the prompt-based diagram button by mistake.

Regenerate after correcting selection.

---

## Phase 5 — Plan & Run a UX Workshop (Team Alignment)

1. **Select the diagram + all relevant stickies**.
2. Open the **Workshops** tab.
3. Choose a template **or** write a short goal prompt, e.g.:

   * *“Goal: Decide which features to keep for MVP.”*
   * Duration: **1 hour**, Participants: **5**.
4. Click **Generate** to get:

   * A 4-activity **agenda**, **facilitator notes**, and a ready-to-send **email template**.
5. **Add to FigJam**.
6. Run the session; capture outcomes directly on the board.

**After the workshop**

* Re-synthesize: repeat Phase 2–3 (features + details) to reflect decisions.

---

## Phase 6 — (Optional) Generate Wireframes from the Diagram

1. **Select the diagram** → **Add as Figma Design**.
2. In Figma, choose a page (e.g., “Home”) and **Generate Wireframe**.
3. Repeat per page as needed.

> Many teams now skip wireframes if they intend to go straight to hi-fi with UX Pilot Web. Use wireframes only if your team requires them.

---

## Phase 7 — Create a Concise Project Brief (for Hi-Fi)

1. Back in FigJam, **select everything** relevant (final features + details).
2. In **AI Tools**, run:

   * *“Give me a summary of the app requirements with feature details.”*
3. Copy the resulting **project brief** (this becomes your single source of truth).

**Tip**
Aim for one well-structured sticky containing all essential requirements and constraints.

---

## Phase 8 — Generate High-Fidelity UI in UX Pilot Web

1. Open **UX Pilot Web** → **Design** tab → **New Design File**.
2. In **Page name / Style settings**, specify:

   * **Theme**: e.g., “Dark theme, bold colors.”
   * **Palette**: name hex values if you have them.
   * **Typography**: family/weight preferences.
3. Click **Create** (new project initializes).
4. **Paste** your **project brief** into the **Context** box.
5. Settings:

   * **Fidelity**: **Hi-fi design**
   * **Platform**: **Mobile**
   * **Deep Design**: **On** (for richer detail)
6. Click **Autoflow**:

   * UX Pilot proposes the screen map (e.g., Home + \~10–13 pages) and displays total cost.
7. Click **Generate** to produce the full set.

**Quality checks**

* Open **Preview** to interact (scrollable content, nav).
* Use **All Design** to scan every page for coverage.
* Compare screens against the brief and flow diagram.

---

## Phase 9 — Document Prompt History (Your “Design Source”)

* For each screen in UX Pilot Web, open **Prompt History**.
* **Export or copy** these prompts into your documentation.
  This is the most reliable way to:

  * Recreate a screen later.
  * Explain design intent during reviews.
  * Maintain a change audit.

**Recommendation**
Create a `Prompts.md` (or a FigJam section) that maps:

* *Screen → Last applied prompt → Rationale/notes*.

---

## Phase 10 — Edit the UI with Natural Language

Two modes:

* **Global Edit** (screen-level):

  * Example: *“Move Participants below the banner; remove the four action buttons.”*
* **Section Edit** (component-level):

  * Example: Select the “Take a photo” floating button → *“Make it a bright color.”*

**Workflow**

1. Choose **Global** or **Section** edit.
2. Apply the instruction.
3. If partially applied, refine:

   * Use **Section Edit** to precisely target areas.
4. Use **Version History** to revert if needed.
5. **Preview** after edits to validate hierarchy, contrast, and tap targets.

---

## Phase 11 — Developer Handoff & Technical Requirements

**Source code per screen**

1. In UX Pilot Web, open a page → **View Source Code** → copy.
2. Add to your docs for reference or scaffolding.

**Export to Figma**

1. On a generated screen (e.g., Home) → **Save to Figma**.
2. In Figma, open **Retrieve in Figma** to import.
3. Organize artboards and apply any manual tweaks.

**Generate technical requirements in FigJam/Figma**

1. Select a **screen** (or element).
2. In **AI Tools**, prompt:

   * *“Give me technical requirements for this screen.”*
3. UX Pilot returns a sticky with **dev-ready requirements/specs**.

**Build a lightweight design system**

* For recurrent components (buttons, cards, tabs, banners):

  * Generate **design specs** and **requirements** per component.
  * Capture tokens (colors, spacing, radii, typography).
  * Normalize names and usage guidelines.

---

## Phase 12 — Governance: Keep Everything Traceable

* **Single source brief**: the summary sticky you pasted into UX Pilot Web.
* **Prompt history**: per screen in a shared doc.
* **Workshop artifacts**: agenda, outcomes, and decisions linked to feature updates.
* **Diagram**: maintained as the high-level truth for navigation and states.
* **Figma import**: mirrors final decisions and is ready for manual polish if needed.

---

## Sample Prompt Library (Copy/Paste)

**Ideation**

* “Give me ideas for a UI/UX project.”

**Feature generation**

* “Give me features for this **mobile** app.”

**Feature elaboration**

* “Give me details for each feature—how it works from the user’s perspective.”

**Project brief**

* “Give me a summary of the app requirements with feature details.”

**Diagramming (selection-based)**

* Use **Diagram → Generate Diagram** (top button) with all relevant stickies selected.

**Wireframe (optional)**

* “Generate a wireframe for the **Home** page from this diagram.”

**UI editing**

* **Global**: “Move Participants below the banner and remove the four action buttons.”
* **Section** (after selecting element): “Make this CTA a bright color.”

**Technical requirements**

* “Give me technical requirements for this screen.”
* “Generate design specs for this component (props, states, responsiveness).”

---

## Troubleshooting & Pro Tips

* **Nothing shows in the diagram**
  Ensure you selected the right stickies and used the **top** Generate button.
* **Features feel generic**
  Always include **platform** and **constraints** (e.g., offline mode, accessibility targets, locales).
* **Hi-fi not matching brand**
  Provide explicit **palette hexes**, **type families**, and a style sentence (e.g., “bold, editorial, high contrast, dark theme”).
* **Edits partially applied**
  Switch from **Global** to **Section** editing for precision.
* **Coverage gaps**
  Revisit **Autoflow** with a more explicit brief; list critical screens (e.g., “Itinerary Detail, Document Scanner, Offline Mode Settings”).
* **Documentation drift**
  Update the brief and re-generate or re-edit screens; paste updated prompt history into your docs.

---

## Lightweight Template (Recommended)

Use this skeleton in FigJam/Figma to keep artifacts aligned:

1. **Product Brief (single sticky)**

   * Problem, Audience, Platforms, Goals, Constraints.
2. **Feature Backlog (grouped stickies)**

   * MVP / Next.
3. **User-Facing Requirements (per feature)**

   * Flows, key states, error/empty cases.
4. **Flow Diagram**

   * Generated from selection.
5. **Workshop Pack**

   * Agenda, activities, email template, outcomes.
6. **Hi-Fi Screens**

   * Links to UX Pilot Web project; Preview notes.
7. **Prompt History Log**

   * Screen → Prompts → Notes.
8. **Technical Requirements**

   * Screen-level + component-level specs.
9. **Design System Seeds**

   * Tokens, components, usage.

---

## Summary

Using **UX Pilot** end-to-end, you can progress from a single sticky-note idea to a documented, developer-ready UI—without leaving FigJam/Figma and UX Pilot Web. The key is to **iterate in layers**: ideate → specify → diagram → align (workshop) → brief → generate hi-fi → refine → document → hand off.
