# Build Your Own AI Interview Agent
### CareerBytes Workshop — June 27, 2026

Welcome! This guide has everything you need to build your own AI-powered interview coach today. No coding experience needed.

---

## Before You Start

1. Create a free account at [bolt.new](https://bolt.new)
2. Create a free account at [claude.ai](https://claude.ai)
3. Create a free account at [console.anthropic.com](https://console.anthropic.com) to get your API key

> You will be given a shared Anthropic API key during the workshop. Swap it for your own free key after the event.

---

## How This Works

You're going to use **two AI tools together**:

- **Claude** — to help you think through your app and write the Bolt prompt
- **Bolt** — to actually build the app from that prompt

The reason we use Claude first is simple: a vague prompt gives you a broken app. Claude helps you think through every detail before Bolt touches a single line of code.

---

## The Full Workflow

```
Your idea (one sentence)
        ↓
Claude asks you 4 questions
        ↓
Claude writes a detailed Bolt prompt
        ↓
You paste it into Bolt
        ↓
Working app 
```

---

## Step 1 — Use Claude to Plan Your App

Open [claude.ai](https://claude.ai) and paste this entire message:

---

> I want to build a web app. I'll describe my idea in one sentence. Ask me exactly these 4 questions one at a time and help me think through each answer in detail:
>
> 1. What does each screen look like and feel like?
> 2. What does the user do on each screen, step by step?
> 3. How does the AI fit in — what does it receive, what does it return, when does it get called?
> 4. What are the edge cases — what happens if the user refreshes, uploads a bad file, or the API fails?
>
> After I've answered all 4, write everything as a structured Bolt.new prompt under these headings: UI Design, User Flow, Backend & AI Logic, Edge Cases.
>
> Start the prompt with "Build a [app name]".
>
> Then add these two blocks at the end, word for word:
>
> ---
> **Design Defaults — use these if I haven't specified them:**
> - Card max-width: 680px, centered, overlapping the header by 48px using negative margin
> - Card style: white background, subtle shadow, 16px border radius, 32px padding
> - Font: Inter from Google Fonts
> - Buttons: solid fill, white text, full width, rounded, never grey
> - If I mentioned a color scheme, use those exact hex codes throughout
> ---
> **Technical Requirements — always follow these exactly, never deviate:**
> - Model: use `claude-haiku-4-5` for all Anthropic API calls — not claude-3, not claude-sonnet, not any other model
> - API key: read from `.env` file as `ANTHROPIC_API_KEY` — never hardcode it, never build a settings UI, never store it in a database
> - API calls: always call Anthropic server-side through an Express backend on port 3001 — never call Anthropic from the browser, this causes CORS errors
> - Dev setup: run Vite frontend and Express backend together using `concurrently` — one dev script starts both
> - Server: Express must call `dotenv.config()` at the very top of the server file before anything else
> - Session persistence: save all state to localStorage on every change, restore on page load, never reset on tab switch or refresh
> - AI text rendering: strip all markdown characters before displaying any AI text in the UI — no asterisks, pound signs, bullet symbols, or underscores
> ---
>
> My app idea: [REPLACE THIS WITH YOUR ONE SENTENCE IDEA]

---

Claude will ask you the 4 questions **one at a time**. Answer each one naturally, like you're describing your idea to a friend. Don't overthink it.

Once you've answered all 4, Claude will generate a complete, detailed Bolt prompt.

---

## Why These 4 Questions Matter

Most Bolt apps break because the prompt was too vague. These 4 questions force you to think through every detail before Bolt starts building:

| Question | What it catches |
|----------|----------------|
| What does each screen look like? | Vague UI = generic ugly app |
| What does the user do step by step? | Missing this = broken flow |
| How does the AI fit in? | Wrong AI setup = CORS errors, broken calls |
| What are the edge cases? | Skipping this = app resets on tab switch, crashes on bad input |

---

## Why Bolt Breaks Without Specific Instructions

There are 4 things Bolt always gets wrong if you don't explicitly tell it what to do:

**1. The API key**
Bolt will build a settings UI, hardcode the key, or store it somewhere weird. The fix: always specify `.env` file, server-side only, nothing else.

**2. The app resets when you switch tabs**
Bolt uses React state by default — it dies on tab switch or refresh. The fix: always specify localStorage persistence.

**3. Raw markdown symbols appear in the UI**
Claude returns text with `**bold**` and `## headers`. Bolt renders them as-is. The fix: always say strip markdown before rendering.

**4. CORS errors on API calls**
Bolt puts API calls directly in React components. Browsers block this. The fix: always use an Express backend on port 3001.

The Claude prompt above automatically handles all 4 of these. That's why it works.

---

## Step 2 — Paste Claude's Output Into Bolt

1. Copy the entire Bolt prompt Claude generated
2. Go to [bolt.new](https://bolt.new)
3. Paste it into the chat and hit enter
4. Wait — this takes 5-10 minutes for a full app (~100K tokens)

> Don't interrupt Bolt while it's building. If it stops mid-way, type: "Continue building. What files are left?"

---

## Step 3 — Add Your API Key

Once Bolt finishes building:

1. Go to [console.anthropic.com](https://console.anthropic.com)
2. Sign in → API Keys → Create new key
3. In Bolt's file tree, look for the `.env` file
4. Replace `your-key-here` with your real API key

> If you can't see the `.env` file, type in Bolt chat: "Show me the .env file and let me edit it"

---

## Step 4 — Test Your App

Click the Preview button in Bolt. Your app should be fully working.

Try it end to end:
- Enter a job role
- Answer all 5 questions
- See your score and feedback
- Switch tabs mid-interview and come back — your progress should be saved

---

## Step 5 — Iterate and Customize

Once your base app is working, ask Bolt to add features:

- "Add a difficulty selector — Junior, Mid, or Senior level"
- "Add an interview type selector — Behavioral, Technical, or Mixed"
- "Make the results page show which questions I struggled with most"
- "Add a motivational message based on the score"
- "Add a dark mode toggle"
- "Show a study cheat sheet at the end with 3 topics to review before the real interview"

---

## The Example App

Here's the prompt used to build the example shown at the workshop. This is what the 4-question Claude conversation produced:

```
Build a AI Interview Coach web app using React (Vite), Express backend on port 3001, and the Anthropic API.

UI Design
- Light grey background (#F8FAFC), white cards with subtle shadow, indigo accents (#4F46E5)
- Global header: navy (#0F172A) to indigo (#4F46E5) gradient, app name centered, 3 feature pills below it ("Tailored Questions", "AI Feedback", "Improvement Tips")
- All screens share the same header — it never changes
- Cards are rounded (16px), padded (32px), white background, shadow — no harsh borders
- Typography: Inter from Google Fonts, generous line height, nothing cramped
- Buttons: solid indigo fill, white text, full width, rounded — disabled state is visibly muted
- No markdown symbols ever rendered in the UI — strip all *, #, _, • from AI output before displaying

Home Screen
A single centered card (max-width 680px, overlapping header by 48px) containing:
- Text input labeled "Job Role" — required, placeholder: "e.g. Product Manager at a fintech startup"
- A toggle link labeled "Add Job Description (optional)" — clicking reveals a textarea; clicking again hides it
- A PDF upload zone labeled "Upload Resume (optional)" — on upload show filename and a " Remove" button
- A "Start Interview" button — disabled until Job Role has at least one character
- PDF accepts .pdf only; on failed extraction show: "Could not read this PDF, try another file" — continue without resume context

Interview Screen
- Progress bar showing "Question X of 5" — updates after each submission
- A card with the current question in large readable text
- A textarea for the answer — no placeholder, clean empty space
- Submit button: "Submit Answer" for questions 1–4, "Submit & See Results" for question 5
- Submit disabled until textarea has at least one character
- On submit: fade out current question, fade in next question — no page reload
- One-way flow only — no back button

Results Screen
- Loading state after final answer: spinner + "Analysing your answers…"
- Once results arrive, show all at once:
  - Score: large bold indigo number (e.g. "7 / 10")
  - Overall feedback paragraph
  - Three numbered improvement points
  - "Practice Again" button — wipes all localStorage, returns to fresh home screen

User Flow
1. User fills in job role (required), optionally adds JD and/or uploads resume PDF
2. Clicks "Start Interview" → API Call 1 fires immediately → loading spinner on button
3. On success: navigate to interview screen, question 1 displayed
4. User answers each question, clicks Submit → answer saved to localStorage → next question fades in
5. After question 5 submit → API Call 2 fires → "Analysing your answers…" loading screen
6. On success: results screen with score, feedback, 3 improvement points
7. "Practice Again" → clear all localStorage → fully reset home screen

Backend & AI Logic

API Call 1 — Question Generation
- Triggered when user clicks "Start Interview"
- Frontend sends to POST /api/generate-questions: jobRole, jobDescription (optional), resumeText (optional, extracted from PDF client-side using PDF.js)
- Claude generates exactly 5 behavioral interview questions in STAR format, tailored to the role, JD, and resume. Each one asks for a real past experience ("Tell me about a time when…").
- Returns a plain JSON array of 5 question strings
- Frontend saves array to localStorage immediately on receipt

API Call 2 — Evaluation
- Triggered after user submits answer to question 5
- Frontend sends to POST /api/evaluate-answers: jobRole, questions (array of 5), answers (array of 5)
- Claude scores each answer against the STAR framework — Situation, Task, Action, Result — noting which components the candidate hit and which they missed
- Claude returns: score (1–10), feedback (one paragraph that names the STAR elements they nailed and the ones they skipped), improvements (array of 3 strings, each tied to a weak or missing STAR element)
- Strip all markdown from feedback and improvements before storing in state

Edge Cases
- Refresh on interview screen: restore exact question from localStorage — no AI call needed
- Refresh on results screen: restore results from localStorage — no AI call needed
- API Call 1 failure: stay on home screen, show error + Retry button
- API Call 2 failure: stay on loading screen, show error + Retry button — answers preserved in localStorage
- Empty answer: Submit button disabled — no minimum length beyond that
- Bad PDF: show inline error, clear upload, continue without resume context
- Tab switching: localStorage saves on every state change — no data loss

Technical Requirements — always follow these exactly:
- Model: claude-haiku-4-5 for all Anthropic API calls
- API key: read from .env file as ANTHROPIC_API_KEY — never hardcode it, never build a settings UI
- API calls: server-side only through Express on port 3001 — never call Anthropic from the browser
- Dev setup: run Vite + Express together using concurrently — one dev script starts both
- Server: call dotenv.config() at the very top of the server file before anything else
- Session persistence: save all state to localStorage on every change, restore on page load
- AI text rendering: strip all markdown characters before displaying any AI text in the UI
```

---

## Useful Links

- [Bolt.new](https://bolt.new) — Build your app
- [Claude.ai](https://claude.ai) — Plan your app and generate your Bolt prompt
- [Anthropic Console](https://console.anthropic.com) — Get your API key
- [Sage](https://www.sageinterview.co) — The voice AI interview coach built by your workshop facilitator

---

*Built at CareerBytes Workshop*
