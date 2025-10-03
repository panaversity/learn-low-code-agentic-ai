# UI Development by Prompting

[Watch: Lovable FULL Tutorial - For COMPLETE Beginners (No Experience Needed)](https://www.youtube.com/watch?v=YLjopoEnPi8)

Watch this Video and Tutorial based on this tutorial is also included below.

[Build Ai-powered Prompt Engineering Projects with AI](https://lovable.dev/how-to/ai-and-machine-learning/ai-powered-prompt-engineering)

[The Lovable Prompting Bible](https://lovable.dev/blog/2025-01-16-lovable-prompting-handbook)

## Lovable AI — A Complete, Professional, Step-by-Step Tutorial

This tutorial distills the full workflow shown in the provided [video](https://www.youtube.com/watch?v=YLjopoEnPi8) into a crisp, repeatable guide—from your first prompt to a deployed full-stack app with authentication, data storage, version control, and UI polish.

---

## 1) What Lovable Is (and why it’s useful)

* **AI app builder** that can generate full-stack web apps from natural-language prompts.
* **No coding required**, but you can edit/export code if you want (the in-editor code browser is a **paid** feature).
* Works best when you **build in small steps** and iterate.
* Supports **importing Figma**, **attaching images** as design references, and **remixing** community projects.

> Credits: It’s free to start with daily credits. Each action consumes credits; bigger builds cost more than small edits.

---

## 2) Get Set Up

1. Go to **lovable.dev** and create/sign in to your account.
2. Confirm your **credit balance**; you’ll accrue more daily.
3. (Optional) Upgrade if you need private projects or the built-in **code browser**.

---

## 3) Plan First, Then Prompt

Lovable rewards clarity. Before you start, outline:

* **What you’re building** (MVP scope).
* **Pages & features** (list them).
* **Visual style** (colors, tone).
* **Future steps** (add later, not all at once).

**Example MVP** (from the video): a **Macro Calculator** (calories/protein/fat targets), later expanded into a fitness tracker.

**Initial prompt (sample):**

> “Create a simple macro-tracking app. Users enter gender, age, height, weight, goal (lose/maintain/gain), and activity level. Recommend daily calories, protein, fat, etc. Use a clean UI with an **orange + black** theme. Keep it simple; we’ll add features later.”

Press **Enter** to generate.

---

## 4) Understand the Interface

* **Left panel**: your conversation and controls with Lovable.

  * **Default mode**: Lovable acts and modifies the project.
  * **Chat mode**: Lovable only **answers questions**—it doesn’t change code.
  * **Attach**: Add images for visual guidance.
  * **(Later) Knowledge**: Persistent rules & docs (see §9).
* **Right side**: **Live Preview** of your app.

  * Top bar: **Refresh**, **Pop-out**, **Route picker** (choose pages), **Device sizes** (desktop/tablet/phone).

**Tip:** After a build, **test the UI** immediately to catch issues early.

---

## 5) Make Targeted Visual Edits

Click **Edit** → select an element in the preview → describe a change.

Example:

> “Underline this title in orange.”

Lovable applies the change precisely to the selected component.

**If you don’t like a change:**

* Use **History** (top bar) to **Restore** a prior state.
* Use **Code diff** (for paid code view) to see exactly what changed.

---

## 6) Add a Landing Page (Second Feature)

Prompt:

> “Create a **stylish landing page** named **‘Tim’s Macro App’** with a clear call-to-action that routes to the calculator.”

Result: You’ll now see two routes, e.g. `/` (landing) and `/calculator`.

---

## 7) Meta-Prompting with Chat Mode (Strongly Recommended)

When you **know the outcome** but **not the best instructions**, use **Chat mode** to have Lovable *write the prompt for Lovable*.

Example (in **Chat mode**):

> “I want a **dark/light mode toggle** site-wide. Help me craft a clear, safe prompt for the model to implement this without breaking the project.”

Copy Lovable’s proposed prompt, **exit Chat mode**, paste it, and run.

**Prompting tip:** The **first and last lines** of your prompt carry extra weight—place key constraints there.

---

## 8) Add Advanced UI Components (Animations)

Lovable works well with high-quality, pre-authored component prompts (e.g., from **21st.dev**, a curated collection the speaker recommends).
Workflow:

1. Copy the component’s detailed prompt (it often includes usage and code fragments).
2. Paste into Lovable and **add a final line** like:

   > “Integrate this on the **home/landing page** only.”
3. Run and verify visually.

**Note:** Complex animations can work out-of-the-box, but be ready to iterate.

---

## 9) Project Knowledge (Persistent Rules & Context)

Open **Knowledge** (left panel → **+** → Knowledge). Add rules Lovable should always follow, e.g.:

* “Every component must support **light and dark** themes and integrate with the existing theme toggle.”
* “Prefer **simple, clean UI**; avoid unnecessary complexity.”

Use bullet points; keep each rule crisp. Update as the project grows.

---

## 10) Version Control: Git & GitHub

1. Click **Sync to GitHub** (top bar).
2. Connect your GitHub account/org and create the repo.
3. Lovable pushes the code; **two-way sync** lets you commit via GitHub and pull back into Lovable.

**If preview breaks after linking GitHub** (e.g., “blank app”):
Prompt Lovable:

> “After connecting GitHub, the app preview is blank. **Fix the preview** and ensure the app builds and runs.”

---

## 11) Add a Backend with Supabase (Auth + Data)

You’ll need a backend for **accounts**, **private data**, and **protected pages**.

**A. Connect Supabase**

1. Click the **green Supabase button** (top right in Lovable).
2. Sign in (GitHub is convenient) and **create a new project** (choose region, set a strong password).
3. Back in Lovable, click **Connect** to link the project.

**B. Ask for Auth + Protected Route(s)**
Prompt:

> “Enable **user sign-up/sign-in**. **Protect `/calculator`** so only authenticated users can access it. When a user clicks ‘Calculate’, **save the calculation** to the database under their account.”

Lovable will generate database changes (**migrations**) and access rules (RLS policies).

* When asked about storing extra profile fields, you can keep it minimal (name, email) to start.
* **Review migrations** before applying; then click **Apply changes**.

**C. Test Authentication**

* Use **Sign Up / Sign In** from the UI.
* Confirm the email if required (Supabase often sends confirmation emails by default).
* Verify that visiting `/calculator` requires sign-in.

**D. Persist and View Data (Calculation Log)**
If logs aren’t visible yet, ask:

> “Add a **Calculation History** section (or a new page) that lists the user’s past calculations.”

Test: Perform a calculation, then verify it appears in **History**.

---

## 12) Deployment

* Click **Publish** to build and deploy.
* Review **Security** warnings.
* Use the generated URL to access the live app.
* (Optional) Configure **Custom Domains** and **SEO** from project settings.

---

## 13) Debugging & Problem-Solving Tactics

* **Iterate small.** Add one feature at a time; verify before moving on.
* **Switch to Chat mode** to ask “what/why/how” questions about migrations, policies, or errors.
* **Be explicit**: paste error messages, attach screenshots of misbehavior, and explain what you expected vs. what happened.
* **Use History** to roll back to a known good state.
* **State constraints** at the **start/end** of prompts to keep the model on track.
* **Supabase gotchas**:

  * Confirm emails if sign-in “succeeds” but you remain unauthenticated.
  * Ensure **RLS policies** allow the operations you need (read/write for the owner).
* **GitHub sync issues**: If sync causes build hiccups, ask Lovable to **refresh/rebuild** the preview and re-link if needed.

---

## 14) Reference Prompts (Copy-Ready)

**A. Landing page**

<div style="width: 200px; border: 1px solid black; padding: 10px;">
Create a stylish landing page titled “Tim’s Macro App” that promotes the calculator with a single primary CTA button routing to /calculator. Use the existing orange + black theme and keep the layout clean and modern.
</div>

**B. Dark/Light toggle (site-wide)**

<div style="width: 200px; border: 1px solid black; padding: 10px;">
Add a persistent dark/light mode toggle. Persist the user’s choice. Ensure all components support both themes and integrate with the existing theme system. Do not regress accessibility or contrast. Test across all pages.
</div>

**C. Auth + Protected Routes + Logging**

<div style="width: 200px; border: 1px solid black; padding: 10px;">
Integrate Supabase auth (email/password). Add sign-up and sign-in flows. Protect /calculator so only authenticated users can access it. On each calculation, save the inputs and computed outputs to a user-owned table with RLS. Add a History section that lists the signed-in user’s past calculations.
</div>

**D. Animated hero on home page**

<div style="width: 200px; border: 1px solid black; padding: 10px;">
Integrate this animation component into the home/landing page only. Keep performance acceptable on mobile and preserve accessibility (focus order, reduced motion preference).
</div>

---

## 15) Checklists

**Quick Start**

* [ ] Sign in to Lovable
* [ ] Draft MVP scope (1–2 features)
* [ ] Run the first build prompt
* [ ] Test the UI

**Prompting**

* [ ] One feature per prompt
* [ ] Key constraints at start/end
* [ ] Use **Chat mode** to refine prompts
* [ ] Attach images if styling matters

**Backend**

* [ ] Connect Supabase
* [ ] Enable auth; protect routes
* [ ] Apply migrations after review
* [ ] Verify data writes + reads with a real user

**Versioning & Release**

* [ ] Sync to GitHub (two-way)
* [ ] Use History for rollbacks
* [ ] Publish and test the live URL
* [ ] (Optional) Custom domain + SEO

---

## 16) Where to Go Next

* Add **profiles** (usernames/avatars).
* Create **meal plans** or **progress dashboards**.
* Expand RLS policies for team/shared views.
* Instrument **analytics** once published.
* Document **Knowledge** rules as your design system hardens.

---

### Final Thought

Lovable shines when you **stay incremental, precise, and test after every change**. Use Chat mode to think with the tool, Knowledge to encode standards, Supabase for real data, and GitHub for reliability. You’ll move faster—and with far fewer surprises.
