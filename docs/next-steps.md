# Next Steps — Your 90-Day Adoption Playbook

A realistic path from "watched the talk" to "running this in production." Designed for one designer in a real team — not a green-field project.

---

## The shape

**Three phases. Lowest political cost first. No phase asks for budget or buy-in until the previous one has earned it.**

```
Phase 1 — Make CLAUDE.md (Week 1, solo)
    ↓ proves the model on your own work
Phase 2 — Add audit (Weeks 2–4, still solo)
    ↓ surfaces a real problem the team can see
Phase 3 — Schedule + share (Month 2+, team-visible)
```

---

## Phase 1 — CLAUDE.md, solo · Week 1

**Goal:** prove to yourself that the textual layer changes how AI behaves on your work.

**Steps:**
1. **Open [`CLAUDE.md.template`](CLAUDE.md.template).** Save as `CLAUDE.md` in one project root. Just one — pick a project you're already working on.
2. **Fill in 5 of the 8 sections.** Identity, architecture, conventions, 3 critical rules, file references. Skip reference tables and discovered rules for now. (~30 min)
3. **Use Claude Code on that project for one real task.** Pick anything: rename a token, document a component, refactor a class. Notice what the AI doesn't have to ask you.
4. **Add the first discovered rule.** Something the AI should have known but didn't. Append it to CLAUDE.md immediately.

**What you're proving to yourself:**
- The persistent context actually changes what the AI produces
- Updating CLAUDE.md mid-task is a habit, not a chore
- "Discovered rules" is the most-active section, not the critical rules

**Don't:**
- Don't show this to your team yet. They'll ask you to "make it standardized" before it's earned.
- Don't try to write all 30 critical rules. Three is enough.
- Don't try to cover every component. Just one project.

**Cost:** ~2 hours total over 5 days. Zero permission needed.

---

## Phase 2 — Audit, still solo · Weeks 2–4

**Goal:** surface real drift in your system. Get evidence the team will recognize.

**Steps:**
1. **Adapt [`audit-tokens.starter.js`](audit-tokens.starter.js) to your system.** Update PROJECT_ROOT, TARGETS, and rule patterns to match your stack. (~1 hour)
2. **Run it.** Don't fix anything yet. Look at what surfaces.
3. **Categorize the findings.** How many are real drift vs script over-flagging? Tune the rules. (~2 hours over a week)
4. **Pick one drift case to fix.** Just one. Fix it the right way — update the component, update the spec, or both.
5. **Add a critical rule to CLAUDE.md** based on what you learned about *why* the drift happened.

**What you're proving:**
- Your system has more drift than anyone realized — or it has none, which is also useful information
- The audit catches what code review misses
- Your CLAUDE.md is now informed by reality, not aspirations

**Don't:**
- Don't run the audit on the whole codebase yet. Start with the area you know best.
- Don't fix everything. Pick one. Show the team one fix that matters.
- Don't propose CI integration yet. Earn the conversation first.

**Cost:** ~4–6 hours over three weeks.

---

## Phase 3 — Schedule and share · Month 2+

**Goal:** make the discipline visible to the team. Prepare for adoption beyond you.

**Steps:**

### 3a — Schedule the audit (Week 5)

Schedule the audit to run weekly. Output goes somewhere YOU see it first — a file in the repo, an email, your own Slack DM. Not the team channel yet.

**What you're checking:** does the audit fire reliably? Does the output stay readable over weeks of accumulation? Is the cadence right (weekly is usually fine; daily is noise)?

### 3b — Share the findings (Week 6)

Pick the most surprising drift case from the past month. Write a 3-paragraph internal post:
1. What the drift was (visually + structurally)
2. How it shipped (no blame, just process)
3. What the audit catches now

Post it. See who responds.

### 3c — Earn the next conversation (Weeks 7–8)

The people who responded to the post are your allies. Have one-on-ones with them:
- *"Want to run this audit on your area?"*
- *"Want to see the CLAUDE.md I've been keeping?"*
- *"Want to co-author the next critical rule?"*

You're now seeding the practice. Not selling it.

### 3d — Propose the team rollout (Month 2+)

When 2–3 teammates are using their own CLAUDE.md and running the audit on their own areas, you can propose making it official:
- Audit runs in CI on every PR (fails on errors, warns on warnings)
- CLAUDE.md is required for new projects
- Quarterly review of accumulated discovered rules → roll into critical rules

By this point the conversation is *"yes, and how"* — not *"why bother."*

---

## When to NOT do this

This playbook assumes you have:
- An AI tool already in use on your team (Claude Code, Cursor, Copilot — at least someone uses one)
- Some autonomy over how you work (you can adopt CLAUDE.md without asking permission)
- A team that will eventually engage with quality findings

**If your team explicitly bans AI tools:** start by getting personal use approved. Run Phase 1 on your own time, on your own projects. Bring it to work when permitted.

**If your team has zero design system:** stop. The four pillars need a system to be ready for AI in. Build the system manually first; adopt this kit second.

**If you're drowning in delivery pressure:** Phase 1 is still ~2 hours. The benefit shows up by week two. Don't skip it because you're busy — do it because you're busy.

---

## Common failure modes

| Failure | Cause | Fix |
|---|---|---|
| CLAUDE.md becomes wishlist of rules nobody follows | Wrote critical rules before you discovered them in real work | Delete the aspirational ones. Keep only what you've actually hit. |
| Audit flags too much, team ignores it | Rules too strict, signal-to-noise low | Tune the rules until clean runs are common. Errors should be rare and meaningful. |
| Team adopts CLAUDE.md but doesn't update it | No habit of mid-task append | Make it a PR template field: "What did this PR teach us? → CLAUDE.md update?" |
| Scheduled audit fires but no one reads it | Output destination wrong | Move from a file in the repo to a notification channel the team actually checks. |
| You burn out before Phase 3 | Tried to do everything yourself | Phase 1 is solo. Phase 2 is solo. Phase 3 is the FIRST place you bring in others. Don't skip the solo phases. |

---

## A 30-second pitch for when someone asks what you're doing

> *"I'm making our design system AI-readable. There's a file in the repo that tells the AI our architecture and rules — so it stops hallucinating when we use it. There's a script that catches drift. Eventually it'll run weekly. The point isn't AI; the point is the discipline. AI is just what surfaces where the discipline is missing."*

That's it. Don't over-pitch. Demonstrate.
