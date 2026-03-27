---
name: client-demo
description: Build polished, self-playing or interactive client demos and sales prototypes. Use this skill when the user wants to create a demo, prototype, proof-of-concept, or sales mockup for a client — including dashboards, SaaS tools, AI agent showcases, or product walkthroughs. Prompts the user for client details, demo format, and UI design style before building. Outputs a single deployable HTML file ready for GitHub Pages.
---

# Client Demo Builder

Build impressive, deployable client demos and sales prototypes. This skill handles the full flow: gathering requirements, choosing a design direction, building the demo, and preparing it for deployment.

## Workflow

### Step 1: Gather Client Context

Before writing any code, use AskUserQuestion to collect the following. Ask all questions in a single call to minimize back-and-forth:

**Question 1 — Client & Industry**
Ask: "Who is the client and what's their industry?"
- This is a free-text "Other" question (provide example options like "SaaS / B2B", "Retail / E-commerce", "Auto / Dealership", "Manufacturing", "Professional Services")

**Question 2 — Demo Purpose**
Ask: "What should the demo showcase?"
Options:
- "Dashboard / Analytics" — KPIs, charts, data views
- "AI Agent / Automation" — chatbot, workflow automation, auto-responses
- "Operations Tool" — inventory, production, scheduling, CRM
- "Product Landing Page" — marketing, feature showcase, conversion

**Question 3 — Demo Format**
Ask: "How should the demo be presented?"
Options:
- "Self-playing walkthrough (Recommended)" — auto-animated scenes that play like a video, with playback controls. Best for sending a link to a client who just watches.
- "Interactive prototype" — fully clickable UI. Best for live meetings or screen recordings.
- "Both" — interactive with a built-in "Play Demo" button that auto-walks through key flows.

**Question 4 — UI Design Style**
Ask: "What visual style fits this client?"
Options with previews:

- **"Dark Professional"**
  Preview:
  ```
  ┌─────────────────────────────┐
  │ ██ Dark navy/charcoal bg    │
  │ ── Subtle borders           │
  │ 🔵 Blue/purple accents      │
  │ ── Glowing highlights       │
  │    Monospace data, clean    │
  │    Sans-serif headings      │
  │                             │
  │ Best for: Tech, SaaS, AI,  │
  │ fintech, developer tools    │
  └─────────────────────────────┘
  ```

- **"Light & Clean"**
  Preview:
  ```
  ┌─────────────────────────────┐
  │ ░░ White/cream background   │
  │ ── Soft gray borders        │
  │ 🟢 Teal/green accents       │
  │ ── Airy spacing, cards      │
  │    Rounded corners, soft    │
  │    shadows, friendly type   │
  │                             │
  │ Best for: Healthcare, edu,  │
  │ retail, consumer products   │
  └─────────────────────────────┘
  ```

- **"Bold & Editorial"**
  Preview:
  ```
  ┌─────────────────────────────┐
  │ ▓▓ High contrast, B&W base  │
  │ ── Sharp type hierarchy     │
  │ 🔴 Single vivid accent      │
  │ ── Dramatic whitespace      │
  │    Large display fonts,     │
  │    magazine-style layout    │
  │                             │
  │ Best for: Luxury, fashion,  │
  │ creative agencies, branding │
  └─────────────────────────────┘
  ```

- **"Warm Industrial"**
  Preview:
  ```
  ┌─────────────────────────────┐
  │ ▒▒ Warm grays, tan, earth   │
  │ ── Sturdy borders           │
  │ 🟠 Amber/orange accents     │
  │ ── Dense, utilitarian       │
  │    Compact tables, mono     │
  │    data, functional feel    │
  │                             │
  │ Best for: Manufacturing,    │
  │ logistics, construction,    │
  │ hardware, auto              │
  └─────────────────────────────┘
  ```

### Step 2: Plan the Demo Structure

Based on the answers, design the demo architecture:

**For self-playing demos:**
- Plan 4-6 scenes, each 4-8 seconds
- Scene 1 is always a dashboard/overview establishing the product
- Middle scenes show key workflows with typing animations, slide-ins, and data reveals
- Final scene is a branded end screen with a CTA and summary stats
- Include a playback bar with progress, play/pause, restart, and scene-skip labels

**For interactive demos:**
- Plan 3-5 tabbed views accessible via sidebar or top nav
- Each view is fully populated with realistic data
- Include hover states, click interactions, and smooth transitions
- Add a "Powered by [product]" footer

**For all demos:**
- Use realistic, industry-appropriate sample data (real-sounding names, plausible numbers)
- Every element should feel like a real product, not a wireframe
- Single `index.html` file — all HTML, CSS, and JS inline
- No external dependencies except Google Fonts (one distinctive display + one body font)
- CSS variables for the full color system
- Deploy-ready for GitHub Pages

### Step 3: Apply the Design Style

Execute the chosen style with full commitment:

**Dark Professional:**
- Background: `#0b0e14` to `#111520` range
- Cards: `#161b28` with `1px solid #252d3f` borders
- Accent: electric blue `#3b82f6` or violet `#8b5cf6`
- Font: pair a geometric sans (e.g., DM Sans, Plus Jakarta Sans) with JetBrains Mono for data
- Subtle gradient accents, soft glows on active elements, pulse animations on status indicators

**Light & Clean:**
- Background: `#fafbfc` or warm `#fdf6ee`
- Cards: white with soft `box-shadow: 0 1px 3px rgba(0,0,0,.08)`
- Accent: teal `#0d9488` or soft blue `#3b82f6`
- Font: pair a friendly sans (e.g., Nunito, Source Sans 3) with a clean mono for numbers
- Rounded corners (`12-16px`), generous padding, pastel status badges

**Bold & Editorial:**
- Background: pure white `#ffffff` or near-black `#0a0a0a`
- One accent color only — red `#e11d48`, blue `#2563eb`, or emerald `#059669`
- Font: pair a dramatic serif or display face (e.g., Playfair Display, Fraunces) with a crisp sans
- Oversized headings, dramatic whitespace, sharp horizontal rules, minimal decoration

**Warm Industrial:**
- Background: `#f5f0eb` or `#1a1814`
- Cards: `#ebe5dd` (light) or `#252019` (dark) with solid borders
- Accent: amber `#d97706` or rust `#c2410c`
- Font: pair a workhorse sans (e.g., IBM Plex Sans) with IBM Plex Mono for data
- Dense tables, compact spacing, utilitarian icons, no rounded corners (2-4px max)

### Step 4: Build

Write the full demo to a single `index.html` in a project directory named after the client (kebab-case, e.g., `henry-car-guy-demo/`).

Key implementation patterns:
- Scene engine: `setTimeout` chains with cleanup functions between scenes
- Typing animations: character-by-character with `setInterval` at 25-40ms
- Slide-in panels: `transform: translateX(100%)` → `translateX(0)` with `cubic-bezier(.22,1,.36,1)`
- Charts: CSS-only (flexbox bars with transition on height/width) — never use Chart.js or external charting libs
- Toast notifications: fixed position, `transform` slide-in, auto-dismiss
- WhatsApp-style chat (if messaging is involved): green outgoing bubbles `#d9fdd3`, white incoming, doodle background pattern, double-check delivery marks
- Email compose (if email is involved): field-by-field typing, paragraph fade-ins, signature block
- Progress bar: track elapsed time vs total duration, show `MM:SS / MM:SS`

### Step 5: Verify

After building:
1. Start a preview server (`preview_start`)
2. Screenshot each scene/view to verify rendering
3. For self-playing: confirm all scenes play through without stalling or overlapping
4. For interactive: test all click targets and transitions
5. Check that fonts load, animations are smooth, and text is readable

### Step 6: Deploy (if requested)

If the user wants a shareable link:
1. `git init` in the project directory
2. `gh repo create [name] --public --source=. --push`
3. Enable GitHub Pages: `gh api repos/{owner}/{repo}/pages -X POST -f "build_type=workflow"` then `gh api repos/{owner}/{repo}/pages -X PUT -f "source[branch]=main" -f "source[path]=/" -f "build_type=legacy"`
4. Trigger build: `gh api repos/{owner}/{repo}/pages/builds -X POST`
5. Return the `{owner}.github.io/{repo}` link

## Reference Demos

These existing demos in the user's repo serve as quality benchmarks:
- `/Users/admin/claude/henry-car-guy-demo/` — Self-playing AI lead agent demo (dark professional, 6 scenes, WhatsApp chat, email compose, analytics)
- `/Users/admin/claude/bespoke-suiting-demo/` — Interactive fabric/inventory dashboard (warm industrial)
- `/Users/admin/claude/bishop-bindings-demo/` — Manufacturing cost forecasting dashboard

When building new demos, match or exceed the quality of these references.
