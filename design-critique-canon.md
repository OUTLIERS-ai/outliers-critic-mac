---
date: 2026-07-05
type: reference
ai-first: true
tags: [the-critic, design-critique, usability, design-canon, reusable]
---

> For future Claude: the reusable lens rubrics [[the-critic]] runs. Each lens is a VALUE SYSTEM with a signature question and a flag list — a rubric, NOT a character to impersonate. Output never speaks in-character ("as Steve Jobs…"); it judges plainly and tags the finding with the lens. Promotable to Wiki/Design/ once proven across a second app. Companion: [[Projects/The-Critic/Build-Plan-V3]].

# Design Critique Canon

Each lens produces its OWN independent report. They are meant to disagree — the synthesis adjudicates. When ≥3 lenses independently flag the same thing, that agreement is a high-confidence severity signal.

Standing input to every lens: also judge the live UI against the product's OWN stated design intent (for the app under audit: "Calm & Trustworthy" + the redesign docs). A product failing its own spec is the sharpest finding of all.

---

## Operating stance (applies to the WHOLE audit, every phase, every lens)

**1. Zero-prior — you have never seen this app.** Discard all recon knowledge of where things live. You do NOT know that limits are in Safety, that the daemon is on Automation, that a campaign is edited four levels deep. You find out by clicking, and when you go hunting for something and can't find it, you LOG THE HUNT — every wrong screen you opened, every place you expected it and it wasn't, how many clicks before you found it (or gave up). The getting-lost is not overhead; it IS the finding. Never shortcut to a known location — a real new user can't.

**2. Exhaustive, not tidy.** Completeness beats a clean list. Open every control, menu, modal, sub-panel, node type, and Settings sub-page. Probe every STATE of every element: empty, loading, error, hover, focus, disabled, filled, over-limit, first-run. Exercise keyboard paths (palette, tab order, escape). Do not stop because you have "enough" findings — stop when you have opened everything. A short report on a deep app is a failed audit.

**3. Narrate every query out loud, in writing.** At every screen and every interaction, ask and ANSWER the questions a real user has — What is this? What do I do here? Did that button actually work? Was that slow (ms)? Where did my thing go? Am I safe? Is it on? — and write EACH question and its answer, in order, to a running **`narration.md` transcript**. This live transcript is a FIRST-CLASS DELIVERABLE, not internal scratch: it's the play-by-play of a new person meeting the app, and it's often more revealing than the ranked findings. Every question, every query, every screen — nothing skipped because it "seems obvious". Where an answer is bad, it also becomes a finding.

**4. Functional truth BEFORE look-and-feel.** A user judges "can I do the thing, and is the app honest about its own state" before anything about how it looks. Confirm the app WORKS and the UI matches REALITY — cross-checked against the backend, not trusted from the screen — before you spend a word on design. A beautiful screen that lies about your state (says "not signed in" while the backend holds a live session) is a top-severity failure, not a polish note. This is Lens −1 below, and it outranks every design lens.

**5. RELENTLESS — never accept, always ask "why?", push what's POSSIBLE.** Your failure mode is being satisfied. Concluding "this is fine / looks solid / the app is honest" is the generous-critic failure and it is FORBIDDEN as a stopping point. The default stance is that everything could be better and something is being settled for — your job is to find it. At every element, screen, step and state, as a brand-new user, ask **"why is it this way?"** — then DO NOT accept the answer. Ask why again, and again (five-whys), until you hit either a genuine constraint or a settled-for compromise. Then push past it: **"what SHOULD this be? what would a demanding user expect? what's actually POSSIBLE here? why isn't it that?"** A finding is not only "this is broken/dishonest" — it is equally "this is settling short of what it should be, and here is the better version." Interrogate every choice as if you refuse to accept "that's just how it is." *Worked examples of the stance:* don't accept a 200-at-once sync — ask why not the gradual, safer, most-recent-first trickle the account deserves; don't accept an empty People page after syncing 200 real contacts — ask why the app doesn't connect them and why the user should tolerate two disconnected stores. Push, ask again, never settle. If a run ends with few findings, that is a signal you stopped interrogating too early, not that the app is good.

---

## Lens −1 — Functional Truth (does it actually work, and does it tell the truth?) — RUNS FIRST, OUTRANKS EVERY OTHER LENS
The class a real user hits in the first ten seconds, and the class the design lenses are blind to. An app that lies about its own state or can't complete its core job feels broken no matter how it looks — so these findings sit ABOVE all design findings. This lens is not done by looking; it is done by DOING and by CROSS-CHECKING against the backend. You have Bash — USE IT to curl the app's own `/api/*` endpoints, read its DB/process state, and watch the network, then compare that ground truth to what the UI claims.
**Signature:** "I tried to actually USE this. Did it work? Did it tell me the truth? Prove it against the backend — don't trust the screen."
Four checks:
1. **Ground-truth cross-check.** For every state the UI asserts — connected / signed-in / running / on / "X of 4 done" / counts / caps / "campaign running" / a saved value — fetch the AUTHORITATIVE backend truth (the matching `/api` endpoint, DB row, or process probe) and COMPARE. Every mismatch is a top-severity finding. *Archetype: UI "not signed in" while `/api/browser` returns `has_session:true` — the app lying about your own login.*
2. **Flow completion.** Actually DO each core job end-to-end with real intent and verify it COMPLETES and the effect is real and PERSISTS (survives a reload, and a backend restart where relevant): sign in → reach connected; change a limit → re-fetch confirms it saved; build+configure a campaign → it exists; sync → data appears. A flow that can't complete, or an effect that silently doesn't persist, is a top finding.
3. **Dead / lying / silent controls.** Every button must produce a REAL effect — a network call that SUCCEEDS plus a state change. Watch the network for failed / 404 / swallowed requests and silently-caught errors. A no-op, a dead link, or a control that says nothing when it fails is a finding. *Archetype: a "Finish signing in" button wired to a dead action that gives zero feedback.*
4. **Liveness / staleness.** Detect zombie or stale state — `running:true` with `pid:null`; a served frontend bundle that doesn't match the built source; a backend process that doesn't reflect a saved change (stale server not restarted after an edit). "The UI and the system underneath disagree" is a first-class category.

---

## Lens J — Journey / Job Completeness — walk the WHOLE job, find every break
Audit the complete arc the app exists to serve, not isolated screens. A screen-by-screen audit misses the breaks BETWEEN features — which is where a real user actually falls out. Map the end-to-end job a real user hired the app to do (for the app under audit: land → connect the account → find the right people → reach out → get replies → manage the conversation → convert), and WALK IT in order, as that user, trying to actually get to the end.
**Signature:** "I'm trying to get the whole job done, start to finish. Where does the chain break? Where does the app hand me off to nothing? Where does it abandon me mid-journey — and why should I accept that?"
**Flags:** a handoff between two steps that doesn't connect (sync 200 contacts → People still empty — two stores that should be one flow); a step in the job with no UI path; a point where the user finishes an action and has nowhere obvious to go next; a feature that exists but is orphaned from the flow that needs it; a "dead middle" where the user is stuck between started and done with no nudge forward. Apply the relentless stance (Operating stance 5) to every break: not just "this is broken" but "why is the journey cut here, and what's the complete version that wouldn't abandon the user?"

---

## Lens 0 — Naive First-Run (fresh eyes) — THE DOMINANT LENS
Not a designer. A brand-new user with zero context and no manual. This lens sets the stance for the whole audit (see Operating stance above): it carries the most weight, because the target complaint is "nothing is intuitive for a new person."
**Signature:** "I've landed here. What do I do? I want to [task]. Where is it?" — narrate confusion in real time, out loud, for EVERY screen and EVERY task, not just the landing page.
**Method:** pick the real jobs a new user arrives to do (e.g. change my daily limit, send a message, start a campaign, see who replied) and try to complete each COLD, logging every hunt, wrong turn, and dead-end with a click count.
**Measures:** time-to-first-value; clicks-and-wrong-turns per task; every "I don't know what this does"; the point a real person would rage-quit.
**Flags:** unlabelled controls; jargon ("keeper browser", "warm-up ramp"); dead-ends whose fix lives on another screen; onboarding gates that don't explain themselves; anything findable only if you already know where it is; anything requiring prior knowledge to use.

## Lens 1 — Jobs (ruthless subtraction)
**Signature:** "What can we remove? Where is the ONE obvious path? Why must the user choose this at all?"
**Principles:** simplicity is the ultimate sophistication — remove until it breaks; decide FOR the user; say no to a thousand things; it should just work (no manual); obsess the whole widget.
**Flags:** options that should be defaults or automatic; duplicated controls (same setting in 2–3 places = a decision the product refused to make); settings surfaces that could vanish entirely; features nobody asked for; anything that needs explaining.

## Lens 2 — Rams (honest & unobtrusive)
**Signature:** "Does every element earn its place? Is the product HONEST about what it does?"
**The 10 (compressed):** innovative · useful · aesthetic · understandable/self-explanatory · unobtrusive · **honest (no false affordances, no advertising a feature that isn't built)** · long-lasting (not trend-chasing) · thorough to the last detail · resource-light · as little design as possible.
**Flags:** decoration masquerading as function; dishonest affordances (looks clickable and isn't, or promises more than it delivers — e.g. an advertised-but-absent capability); inconsistency; unfinished detail; visual noise that isn't doing a job.

## Lens 3 — Cooper / Nielsen (interaction & heuristics)
**Signature:** "How deep is this buried? Where's the system status? Can the user recover? Does it match their mental model?"
**Nielsen's 10:** visibility of system status · match system↔real world · user control & freedom (undo/escape) · consistency & standards · error prevention · recognition over recall · flexibility & efficiency · aesthetic & minimalist · help users recover from errors · help & docs.
**Cooper goal-directed:** design for the persona's GOAL; kill **excise** (navigation the user only does to serve the software, not their goal); no modes that trap the user.
**Flags:** click-depth to core tasks (buried limits); hidden/implicit state (engine-off dead-ends); cross-screen dependency traps (screen A needs something started on screen B); two sources of truth for one setting; no recovery/undo; jargon; state the UI knows but never shows.

## Lens 4 — Tufte (clarity & information density)
**Signature:** "Is this legible or is it noise? Is the most important number the most prominent thing on screen?"
**Principles:** maximise the data-ink ratio; no chartjunk; graded visual hierarchy by importance; small multiples; layering & separation; the key metric dominates.
**Flags:** buried key metrics; decorative noise crowding the signal; flat hierarchy where importance should be graded; illegible density in data-heavy surfaces (dashboards, tables, inboxes, KPI strips); numbers presented without the context that makes them mean anything.

---

## Register rule
Findings are written in the plain analytical register (blunt, no softening, no flattery), NOT in the designer's voice. "Settings duplicates the caps editor across Home and Safety — pick one" — never "As Rams, I observe…". The lens is HOW it judged, not HOW it talks.
