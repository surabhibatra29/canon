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

## RULES — enforce all of these

1. **8–12 modules.** Number from 0. The final module must be a synthesis/portfolio module — not more content, but a document that pulls everything together into a career decision.
2. **3–4 spine modules.** Choose based on the person's stated gaps. Mark with spine: true.
3. **Every module: minimum 5 readings, minimum 3 watching items.**
4. **No placeholder readings.** Every item must be a real, findable, high-quality resource. Practitioners, academics, primary sources only. No blog aggregators, no generic newsletter roundups.
5. **No vague search terms.** "AI strategy" is not acceptable. "Hamilton Helmer 7 Powers Invest Like the Best Patrick OShaughnessy" is acceptable.
6. **At least 60% of readings must have verified URLs** (type: "verified", url field populated).
7. **At least one watching item per module must have a direct URL** from TED.com, MIT OpenCourseWare, or an official institutional channel.
8. **Descriptions are written to the person, not about the resource.** First word should not be the resource name.
9. **Rubric weak versions must name the specific shortcut this person is likely to take**, not a generic "not enough detail" critique.
10. **Do not begin generating until all five context fields above are filled in.** If any are missing or vague, ask one clarifying question per missing field before proceeding.
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
