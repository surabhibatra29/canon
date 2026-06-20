# Canon Curriculum Generator — Master Prompt

Use this prompt to generate a custom curriculum for any student. Paste it into Claude with the bracketed sections filled in. The output is valid JSON that imports directly into Canon via the Design mode upload.

---

## HOW TO USE

1. Fill in the five student context fields at the top
2. Paste the entire prompt into Claude (claude.ai, ideally Claude Opus or Sonnet)
3. Copy the JSON output
4. In Canon → Design mode → "Upload curriculum" → paste as a `.json` file or add an import-from-JSON button (future feature)

---

## THE PROMPT

```
You are designing a rigorous, artifact-first self-study curriculum for a specific person with a specific goal. The output must be a JSON object matching the exact structure below. Every field matters. Do not return anything except the JSON.

## THE PERSON
- Background: [e.g. Senior PM, 8 years experience, mostly B2B SaaS, took a 2-year career break]
- Target role/goal: [e.g. AI PM at a Singapore govtech or Series B startup]
- Specific gaps to close: [e.g. no hands-on AI experience, rusty on strategy frameworks, never owned pricing]
- Time available: [e.g. 10 weeks, ~10 hours/week]
- Geographic/market context: [e.g. Singapore, targeting public sector and fintech]

## CURRICULUM PHILOSOPHY — follow this precisely

**Artifact-first.** Every module produces one named, portfolio-ready output. Not "read about strategy" — "produce a Strategic Position Map that a hiring manager could read."

**Honest over comfortable.** Assignments should push into areas the person finds uncomfortable, not confirm what they already know. Rubrics must name what weak looks like — not just what strong looks like.

**Primary sources over summaries.** Every reading should link to the actual practitioner, academic, or institution — not a Medium summary. Prefer: HBR, MIT OpenCourseWare, TED.com, official author sites, Substack newsletters, Stripe Atlas guides.

**Watching items need direct links, not just search terms.** Where a stable URL exists (TED.com, OCW, official YouTube channel), use it. Where not, provide a search term specific enough to surface the exact video as the first result.

**Descriptions explain WHY, not WHAT.** Don't describe what the resource is — say why this specific person should read it. What will they understand that they don't now? What will it make uncomfortable?

**Spine modules are load-bearing.** 3–4 modules must be marked spine:true — these are the ones the person must complete even if life compresses the rest. Choose them based on the person's biggest gaps, not what sounds impressive.

**Rubrics must be honest.** The weak version should sting a little to read. The strong version should feel hard to achieve. selfCheck questions should probe the specific evasions this type of person makes.

## OUTPUT FORMAT

Return a single JSON object. No markdown code fences. No comments. Valid JSON only.

{
  "title": "...",
  "subtitle": "One sentence describing the curriculum's goal and audience.",
  "paceNote": "Aim for one module every [X] days. Modules [list spine module numbers] are load-bearing — do these even if life compresses the rest. Everything else deepens the spine.",
  "modules": [
    {
      "id": "m0",
      "number": 0,
      "spine": true,
      "title": "...",
      "subtitle": "One sentence. Honest, not motivational.",
      "whyItMatters": "2–3 sentences written to THIS person. Why does this module matter given their specific background and gap? Not generic — reference their context.",
      "artifact": "Named deliverable. Specific enough that two people would agree on whether it exists and whether it is good.",
      "rubric": {
        "weak": "What this artifact looks like if the person went through the motions. Should sting slightly to read.",
        "strong": "What it looks like if they genuinely leveled up. Should feel hard to achieve.",
        "selfCheck": "Three yes/no questions that probe specific evasions this type of person makes.\nOne question per line.\nEach should be uncomfortable to answer honestly."
      },
      "readings": [
        {
          "id": "m0r1",
          "number": "1",
          "title": "Full title with author and publication name",
          "url": "https://... (verified URL if confident it is stable and correct; empty string if uncertain)",
          "searchTerm": "Specific enough to surface the exact content as the first search result (empty string if url is provided)",
          "type": "verified",
          "description": "Why THIS person should read this. What will they understand that they currently do not? What assumption will it challenge? Who wrote it and why does that matter?"
        }
      ],
      "watching": [
        {
          "id": "m0w1",
          "title": "Speaker Name — 'Full Talk Title' (Platform, runtime)",
          "url": "https://... (TED.com, MIT OCW, official YouTube — or empty string if uncertain)",
          "searchTerm": "Exact search string that surfaces this video as the first result (empty if url provided)",
          "description": "Why this talk. What does it do that the readings do not. What should the person pay attention to."
        }
      ],
      "assignment": "Multi-part assignment. Numbered steps. Each step names a specific output, not a vague activity. Final paragraph names the artifact and what quality it should reach.",
      "reflection": "One or two questions. Not 'what did you learn?' — something the person might flinch to answer honestly, specific to this module's content."
    }
  ]
}

## SOURCE DISCOVERY PROTOCOL

Before generating readings and watching items, work through this hierarchy. The goal is primary sources and trusted practitioners — not popular summaries.

**Step 1: Go to the originator.**
Every concept has an originator. Find them first. Dweck on growth mindset, not HBR on Dweck. Kahneman on cognitive bias, not a "top 10 biases" article. Feynman on learning, not a Feynman-technique summary. The originator's own writing or talks are always preferred over any interpretation.

**Step 2: Find the gold standard sources for this field.**
Each field has its own trusted practitioners. The examples below are illustrative — use the same logic for any domain not listed.

**For any field, ask three questions before picking sources:**
1. Who are the practitioner-writers with skin in the game? (people who built or do the thing, not people who write about it)
2. Who did the original research? (go to the researcher, not the HBR summary of the researcher)
3. What is the highest-signal publication in this space — written by practitioners, not journalists?

The answers to these three questions produce your source list for any field.

---

*The following are field-specific examples. They are not exhaustive — apply the same logic to any field.*

**For product management:**
- Practitioners: Shreyas Doshi, Marty Cagan (SVPG), Lenny Rachitsky, Julie Zhuo, Paul Graham, Jason Fried (37signals), the Linear team, Teresa Torres (producttalk.org — continuous discovery and opportunity solution trees), Jackie Bavaro (PM interview frameworks and product sense)
- Publications: Lenny's Newsletter, SVPG blog (svpg.com), Signal v. Noise, Farnam Street (fs.blog) for mental models
- Watching: Lenny Rachitsky podcast, Shreyas Doshi talks, Y Combinator lectures, Exponent mock interviews, a16z AI Summit, First Round Capital interviews
- Discovery methods: any module covering product discovery must include Teresa Torres' Opportunity Solution Tree (producttalk.org/opportunity-solution-tree/) and either JTBD (Intercom Job Stories: intercom.com/blog/using-job-stories-design-features-ui-ux/) or the Mom Test (Rob Fitzpatrick). These are primary frameworks tested in interviews.

**For thinking and mental models (any field):**
- Shane Parrish / Farnam Street (fs.blog) — the most systematic treatment of mental models available
- Tim Urban / Wait But Why (waitbutwhy.com) — best long-form explanatory writing on complex ideas
- Richard Feynman — primary source for first principles reasoning ("Fun to Imagine", BBC YouTube)
- Annie Duke — decision quality and separating process from outcome

**For AI (any field that touches AI products or tools):**
- Ethan Mollick / One Useful Thing (oneusefulthing.org) — best practitioner writing on AI changing knowledge work
- Eugene Yan (eugeneyan.com) — production AI systems and LLM product patterns; his "Patterns for Building LLM-based Systems & Products" covers evals, RAG, guardrails, and defensive UX
- Simon Willison (simonwillison.net) — the most honest practitioner writing on AI failure modes, hallucination, and trust
- Hamel Husain (hamel.dev) — practitioner guide to designing AI eval sets; "Your AI Product Needs Evals" is required for any module that involves building or evaluating AI products
- Primary researchers over pop science: Kahneman, Bjork (retrieval practice), Deci & Ryan (SDT)

**For design and craft:**
- Julie Zhuo ("The Year of the Looking Glass", Medium)
- 37signals "Getting Real" and Signal v. Noise
- Linear Method (linear.app/method)
- Paul Graham essays (paulgraham.com)

**For data science and analytics:**
- Practitioners: Cassie Kozyrkov (Google's former Chief Decision Scientist), Eugene Yan, Jeremy Howard (fast.ai)
- Publications: Towards Data Science (practitioner-authored pieces only), fast.ai blog, Chip Huyen (huyenchip.com)
- Watching: fast.ai practical deep learning course, Andrej Karpathy lectures, StatQuest (YouTube) for fundamentals

**For software engineering:**
- Practitioners: Martin Fowler (martinfowler.com), Will Larson (lethain.com), Dan Abramov
- Publications: The Pragmatic Engineer (newsletter), High Scalability, engineering blogs from Stripe, Shopify, Airbnb
- Watching: System design interviews (Gaurav Sen, ByteByteGo), Fireship for overviews

**For any other field:**
Apply the three questions above. Find the practitioner-writers, the original researchers, and the highest-signal practitioner publication. Use those as your source list.

**Step 3: Verify the URL exists and points to the actual piece.**
Do not include a URL you are not confident in. An empty `url` field with a precise `searchTerm` is better than a broken or wrong URL.

**Step 4: Check the source is accessible.**
Free and directly accessible preferred. Paywalled pieces need an alternative or a note. Full books should be excerpted to specific chapters.

**Sources to avoid:**
- Generic "top 10" or "ultimate guide" articles (no author accountability)
- LinkedIn posts and thought leadership carousels
- McKinsey/BCG reports on practitioner topics
- HBR summaries of academic research (go to the researcher instead)
- Medium posts by unknown authors

---

## RULES — enforce all of these

1. **8–12 modules.** Number from 0. The final module must be a synthesis/portfolio module — not more content, but a document that pulls everything together into a career decision.
2. **3–4 spine modules.** Choose based on the person's stated gaps. Mark with spine: true.
3. **Every module: minimum 5 readings, minimum 7 watching items.**
4. **No placeholder readings or watching items.** Every item must be a real, findable, high-quality resource. Practitioners, academics, primary sources only. No blog aggregators, no generic newsletter roundups.
5. **No vague search terms.** "AI strategy" is not acceptable. "Hamilton Helmer 7 Powers Invest Like the Best Patrick OShaughnessy" is acceptable.
6. **At least 60% of readings must have verified URLs** (type: "verified", url field populated).
7. **At least one watching item per module must have a direct URL** from TED.com, MIT OpenCourseWare, or an official institutional channel.
8. **Every module must include at least one watching item that applies the module's concepts in a real or simulated context** — not a lecture, but a demonstration, critique, case study, or practice session. Match the format to the field:
   - Product management: Exponent mock interviews (@tryexponent), topic-matched to the module
   - Design: portfolio critiques, live design reviews, teardown sessions
   - Data science: Kaggle solution walkthroughs, case study presentations, live analysis sessions
   - Software engineering: system design walkthroughs, code review sessions, architecture teardowns
   - Any field: find the equivalent — where does a senior practitioner apply this skill in real time while you watch?
9. **Prioritise high-signal practitioner sources in the watching list.** For product management: Lenny Rachitsky podcast, Shreyas Doshi talks, Y Combinator lectures, Exponent mock interviews, Julie Zhuo talks, Marty Cagan / SVPG, First Round Capital interviews, a16z AI Summit. For other fields: apply the same principle — find the practitioners who talk in public about how they actually do the work, not theorists explaining the concepts. These are the sources students most need to hear in their own voice, not just read.
10. **Descriptions are written to the person, not about the resource.** First word should not be the resource name.
11. **Rubric weak versions must name the specific shortcut this person is likely to take**, not a generic "not enough detail" critique.
12. **Do not begin generating until all five context fields above are filled in.** If any are missing or vague, ask one clarifying question per missing field before proceeding.
13. **Any module covering AI strategy or AI tools must include at least one reading on AI evaluation and trust.** The reading must cover: what hallucination or failure looks like in a product context, how to design or reason about an eval set, and what honest user communication of AI limitations requires. Eugene Yan, Simon Willison, or Hamel Husain are the primary sources for this.
14. **The final synthesis module must include a domain-specific performance practice gate.** The principle: the person must perform their skill out loud to a real human before the module is done. Reading about interviews is not the same as doing one. The format depends on the field:
   - Product management: (a) written 90-second personal narrative (background, decision, what was built, what comes next); (b) written product sense answers for at least three real products; (c) one mock interview with a real human — gated, not optional
   - Design: (a) portfolio walkthrough script written out; (b) live critique session with someone who can give honest feedback — gated
   - Data science: (a) case study presented out loud; (b) take-home analysis reviewed by someone with domain knowledge — gated
   - Software engineering: (a) system design explanation for one problem; (b) mock technical interview with a real human — gated
   - Any field: identify the equivalent real-world performance context and gate the module on completing it. The assignment must name the gate explicitly and state that it is not optional.
```

---

## EXAMPLE FILLED-IN CONTEXT

```
- Background: UX researcher transitioning into PM, 5 years in design research at a consultancy, no shipping experience
- Target role: Product Manager at a health-tech startup in Southeast Asia, Series A–B
- Specific gaps: No metrics ownership, no roadmap or prioritisation experience, no stakeholder negotiation at exec level
- Time available: 12 weeks, 8 hours/week
- Geographic/market context: Kuala Lumpur / Singapore, targeting digital health and wellness apps
```

---

## NOTES FOR THE FUTURE FEATURE

When this is built into Canon as a student-facing feature:

- The five context fields become a structured form (not a free-text prompt)
- The JSON output is validated against the curriculum schema before import
- Watching items with `url` get the "Watch directly ↗" button; those with only `searchTerm` get "Search YouTube"
- The `spine` flag and `paceNote` auto-populate the dashboard pace banner and Core pills
- Rubrics auto-populate the Evaluate tab for each module
- All reading and watching items get the ✕ Skip / ↩ Restore button by default
