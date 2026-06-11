---
name: tracer-bullets
description: "Force AI to build features as tiny end-to-end vertical slices before expanding. Prevents AI slop by stopping the agent from building horizontal layers in isolation. MANDATORY TRIGGERS: 'tracer bullet', 'think small', 'build end-to-end slice', 'stop building layers'. STRONG TRIGGERS: 'build this feature', 'implement this', 'add this functionality', any multi-layer feature request (API + DB, frontend + backend, service + UI). Applies the Pragmatic Programmer tracer bullet technique to AI-driven development. USE when starting any non-trivial feature to prevent runaway generation of unvalidated code."
argument-hint: "Describe the feature you want to build"
user-invocable: true
---

# Tracer Bullets: Build End-to-End, Not Layer by Layer

AI agents have a natural inclination toward sycophancy. In code, this means producing complete solutions all at once — building entire horizontal layers in isolation without ever testing whether the critical path actually works. The result is enormous chunks of code that need reworking. Slop.

The fix: **tracer bullets**. A concept from _The Pragmatic Programmer_. Build one tiny vertical slice end-to-end, test it, get feedback, then expand.

---

## The Problem This Solves

When asked to build a database service with an API, an AI will typically build:

- All API endpoints (`GET`, `POST`, `PUT`, `DELETE`)
- Complete request/response models
- Error handling middleware
- Authentication logic
- Rate limiting
- Logging infrastructure

…and only *then* try to connect to the database. By which point the connection string format is wrong, or it's using an incorrect column type, or the tests don't hit the real database. This is "outrunning your headlights." The AI builds in the dark with no feedback loops.

---

## The Tracer Bullet Workflow

When implementing a feature, follow this loop:

### Step 1 — Identify the vertical slice

Break the feature into a list of end-to-end slices. Pick the one that:
- Touches all system layers (e.g. DB → service → API → UI)
- Validates the riskiest assumption
- Can be tested immediately after build

**Example decomposition:**
```
Feature: "Reveal in File System" for videos

1. TRACER BULLET: Backend endpoint + wire to ONE UI location → test it
2. Add to standalone videos context menu
3. Add to video editor actions dropdown
4. Add to sidebar video context menu
```

### Step 2 — Build only the tracer bullet

Implement just that one slice. Do not build any other location, layer, or variation yet.

**Prompt to add to your build request:**

```
## Tracer Bullets

When building this feature, build a tiny, end-to-end slice first. Seek my feedback
before expanding out.

Tracer bullets come from the Pragmatic Programmer. Write code that gets you feedback
as quickly as possible — a small slice through all system layers. This validates the
architecture is sound before investing significant time in development.
```

### Step 3 — Test immediately

Run the slice. Validate that the critical path works end-to-end:
- Does the DB connection succeed?
- Does the API return real data?
- Does the UI render?
- Does the integration test pass against a real endpoint?

**Do not proceed until this slice passes.**

### Step 4 — Collect feedback and close the context window

Once the tracer bullet works, review:
- Are the patterns correct?
- Are there naming or structural issues to fix now before they multiply?
- Is the approach sound for the remaining slices?

Start a fresh context window for the next slice to avoid context drift.

### Step 5 — Expand

Now repeat: build the next slice, test immediately, close context, expand further.

---

## Checklist Before Starting Any Feature

Before prompting an AI to implement anything, verify:

- [ ] Have I decomposed the feature into vertical slices?
- [ ] Have I identified which slice touches the most system layers?
- [ ] Have I included the tracer bullet instruction in my prompt?
- [ ] Am I starting with *one* slice, not all of them?

---

## Signs You're Drifting Into Slop

Watch for these AI behaviors and stop the agent:

- Building multiple endpoints when only one was asked for
- Adding auth/middleware/rate-limiting before the core path is validated
- Creating models for all cases instead of just the one being tested
- Writing tests against mocks when real integration tests are needed
- Producing files the user didn't ask for "just in case"

If you see these, interrupt and redirect: *"Stop. Let's finish and test the tracer bullet before expanding."*

---

## Key Principle

> Build a tiny, end-to-end slice. Test it. Get feedback. Move to the next slice in a fresh context window. Repeat.

Context window constraints make this discipline **non-negotiable** with AI agents. You cannot ignore tracer bullets the way you might with a human developer — the consequences are immediate and visible.
