# Canon — Video Script (aiEDU Application)
**Target: ~4 minutes | ~550 words spoken**
**Version 3 — rebuilt from raw notes, less polish**

---

## SECTION 1 — THE PROBLEM (0:00–0:40)

Hi, I'm Surabhi, a product manager with about a decade of experience. Still got heaps to learn, honestly. And that's kind of what this is about.

So the problem I was trying to solve was pretty simple, I wanted to upskill and become better at my job. I had some sense of the things that were intriguing me, things I wanted to go deeper on, specifically around AI product strategy and how the PM role is actually changing. And as I started mapping out what to study, I kept bouncing between specialised PM cohort courses, which are genuinely excellent but too structured and too generic for someone who already knows what their gaps are, and certifications, and a DIY maze of self-directed learning where you end up with forty open tabs and no clear thread connecting any of it.

I just wanted something that could help me keep track of what I was actually learning. Like, discover stuff, collate it, track whether I was executing on it, capture my takeaways, and maybe have something real to show for it at the end.

That's where Canon came from.

---

## SECTION 2 — WHAT IS CANON (0:40–1:10)

Canon is a curriculum execution workspace for DIY learners, where you can curate your own curriculum using links, AI curated listings and more candidly a mix of both. For each module there are readings, videos to watch, an assignment that produces a real artefact, reflection spaces, and a rubric so you can self-evaluate and know if your work is any good. Not just whether you finished.

It's for people who want something different than a certificate, and look, certificates have their place, and could even be part of your curriculum and learning journey on Canon! But I mean people who want to be long-term learners, are self-motivated, find the real thinkers in their field, and honestly, for anyone who LOVEEEED studying something they never got a proper chance to go deep on.

---

## SECTION 3 — HOW I BUILT IT (1:10–2:30)

I've been building this solo over the last few weeks using Claude Code. Genuinely fun.

My background is enterprise PM, I've worked across Temasek, BlackRock, and a conversational AI company. So I approached this like I would any product. Define the problem, validate with the user, apply some design thinking, experiment, validate against the sole user, which in this case was me, build iteratively, test. A lot of that process was accelerated by Claude.

But this time I was the sole user, the PM, and the builder all at once.

For technical decisions, storage, auth, data architecture, I asked AI for multiple options with tradeoffs, then independently validated before committing. That's how I landed on Supabase for auth and sync. On the UX side I gave Claude very specific instructions. Colour system, layout, interaction patterns, glassmorphism. Not "make it look nice." Specced it like I would with a design team, albeit working with AI made it a lot faster.

I'm dogfooding it right now, I'm the primary user. I know the bias that comes with that, and I haven't run external user interviews yet. That's next, as I work through my own modules and figure out whether this could be genuinely useful to others apart from me.

---

## SECTION 4 — DEMO (2:30–3:30)

*[Screen share, dashboard]*

This is the dashboard. Fourteen modules, each with a named deliverable. Not a completion checkbox. An actual artefact.

*[Click into a module, Reading tab]*

Each module has readings and watching. There's a skip flow. Mark something irrelevant, give a reason, and it's excluded from your progress. Small UX call but it's also how the AI recommendations I'm building will personalise. Your skip reasons become signal.

*[Assignment tab]*

Assignment tab, rich text, draggable sections, you can cite readings inside your work so it stays connected to the material.

*[Reflection, then Evaluate]*

Reflection is private, auto-saves. And the evaluate tab has a rubric. Weak, strong, ask yourself. Completion is easy to fake. The rubric makes it harder to.

Dark mode. WHY NOT.

---

## SECTION 5 — EVALUATION + WHAT'S NEXT (3:30–4:00)

Honestly, I haven't produced fourteen polished deliverables. What I have made is this video, a living PRD, and Canon itself in production. I think those count.

My north star is simple: is it enjoyable, does it feel like real progress, does it build the habit without over-gamifying. I need more users and time to answer that properly.

What's next: AI recommendations personalised to your reflections, and multi-curriculum support so it's not just PM content. PRDs for both already written. Still figuring out how these things gooooo, but that's kind of the point.

---

*~550 words | ~4 minutes*
