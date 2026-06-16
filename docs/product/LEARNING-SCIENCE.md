# Canon — Learning Science Research Notes

**What this is:** Working notes on the cognitive science principles behind Canon's design. Not a finished thesis — an ongoing exploration. I'm reading the primary literature, understanding the debates, and figuring out what Canon actually implements vs. what it should.

**Why it's here:** Canon is built on the hypothesis that these principles improve how I learn. This document is where I check that hypothesis against the evidence.

---

## Evidence ratings

- **H (High):** Multiple meta-analyses or large systematic reviews. Robust and replicable.
- **M (Moderate):** Some meta-analytic support, but caveats or contested findings.
- **Contested:** Active debate in the literature. Not settled.

---

## 1. Retrieval practice

**Evidence: H**
**Key paper:** Rowland, C.A. (2014). The effect of testing versus restudy on retention: A meta-analytic review. *Psychological Bulletin*, 140(6), 1432–1463. [PMC link](https://pmc.ncbi.nlm.nih.gov/articles/PMC7889502/)

**Also see:** Roediger, H.L. & Karpicke, J.D. (2006). Test-enhanced learning: Taking memory tests improves long-term retention. *Psychological Science*, 17(3), 249–255.

### What the research says

Testing yourself on material produces substantially better long-term retention than re-reading it. Effect sizes in the Rowland meta-analysis range from g = 0.50 to 0.61. Roediger & Karpicke found 61% retention at one week for a test group vs. 40% for a re-reading group.

This is the most robustly supported learning technique in cognitive psychology. Dunlosky et al. (2013) — the authoritative review of ten learning techniques — rated it **HIGH utility**, one of only two techniques to receive that rating.

### The honest caveats

- The testing effect is strongest for factual and declarative knowledge. Effects on conceptual understanding and transfer are more mixed.
- Feedback after retrieval is important — retrieval without knowing whether you were right or wrong is less effective.
- Most studies use relatively simple material (word lists, passages). Less clear for complex professional knowledge.

### What Canon does today

The Evaluate tab's "Ask Yourself" self-check questions are a form of retrieval practice. After completing a module, you attempt to answer probing questions without looking at your notes. This is the right mechanism but a partial implementation.

**What's missing:** There's no systematic testing loop — no spaced retrieval, no quiz mechanism, no way to surface material from earlier modules for retrieval. The "Ask Yourself" questions are static per module. Canon doesn't currently surface module 2 questions when you're on module 7.

**Canon status:** WIP

### Open questions

- Should Canon have an active recall quiz mode — separate from the Evaluate tab — where it surfaces questions from any completed module?
- Is self-generated retrieval (writing from memory before reading) worth building? The "generation + retrieval" combination may be stronger than either alone.
- How do you design retrieval practice for the kind of conceptual, judgment-heavy PM knowledge in this curriculum vs. factual knowledge?

---

## 2. Spaced practice

**Evidence: H**
**Key paper:** Cepeda, N.J., Pashler, H., Vul, E., Wixted, J.T., & Rohrer, D. (2006). Distributed practice in verbal recall tasks: A review and quantitative synthesis. *Psychological Bulletin*, 132(3), 354–380. [DOI](https://doi.org/10.1037/0033-2909.132.3.354)

### What the research says

Spreading study sessions over time produces better retention than the same amount of study crammed together (massed practice). This is one of the most replicable findings in cognitive psychology. The Cepeda meta-analysis covers 839 assessments across 317 experiments and 184 articles.

The optimal spacing interval depends on the retention interval — how long you want to remember the material. For long-term retention, longer gaps between sessions are better.

Dunlosky et al. (2013) rated spaced practice **HIGH utility** — the other of the two top-rated techniques.

### The honest caveats

- Most research uses relatively short time horizons (days to weeks). Less studied at the scale of a multi-month curriculum.
- The ideal spacing interval is not fixed — it depends on the material and how long you want to retain it.
- Spaced practice requires a system to track what was studied when and surface it at the right time. Without the system, the benefit doesn't happen.

### What Canon does today

The pace note banner encourages distributing study over time rather than sprinting through modules. That's the principle, but it's not a mechanism.

**What's missing:** Canon has no spacing algorithm. Nothing tracks when you last visited module 2 and surfaces it for review. There's no "revisit" cue. The student is entirely responsible for spacing their own study.

**Canon status:** To be explored

### Open questions

- Is a full spaced repetition system (Anki-style) right for this kind of curriculum? The material is conceptual and artifact-based, not fact-recall. The classic SRS model may not translate directly.
- Could a lighter version work — a "revisit" cue that surfaces one prior module per week for review, without a full SRS algorithm?
- Is the act of writing artifacts (assignments) itself a form of spaced retrieval — returning to earlier module themes as later modules build on them?

---

## 3. Generation effect

**Evidence: H**
**Key paper:** Bertsch, S., Pesta, B.J., Wiscott, R., & McDaniel, M.A. (2007). The generation effect: A meta-analytic review. *Memory & Cognition*, 35(1), 201–210. [Springer link](https://link.springer.com/article/10.3758/BF03193441)

### What the research says

Producing information — generating it, writing it, completing it — produces better retention and recall than reading or re-reading it. The Bertsch meta-analysis covers 445 effect sizes across 86 studies, with an overall effect of d = 0.40.

### The honest caveats

- "Encodes deeper" is a theoretical interpretation, not a direct finding. The mechanism is contested — mental effort, item-specific processing, and elaboration are all proposed but none is fully settled.
- The effect is strongest for word-level and fact-level material. It is weaker and less consistent for reading prose or conceptual multi-paragraph text. A 2024 replication (Schindler et al., *Applied Cognitive Psychology*) found no generation benefit for expository texts in several conditions.
- "Better retention" is the accurate claim. "Deeper encoding" is interpretation.

### What Canon does today

This is Canon's core mechanic. Every module ends in a named artifact. Reading alone does not count toward completion. The assignment workspace requires production, not consumption.

**Canon status:** Yes

### Open questions

- Does producing a written artifact count as the same mechanism as the generation effect in the laboratory studies? The lab studies are often word-completion tasks. Canon assignments are multi-paragraph professional documents. Are these the same phenomenon?
- Is there a design difference between "write about what you read" (closer to summarisation, low utility) and "produce an artifact that requires applying what you read" (closer to generation + retrieval + transfer)? Canon aims for the latter. Worth checking that assignments are actually designed that way.

---

## 4. Autonomy

**Evidence: H**
**Key paper:** Bureau, J.S. et al. (2022). Pathways to student motivation: A meta-analysis of antecedents of autonomous and controlled motivations. *Psychological Bulletin*, 148(3-4), 218–271. [PMC link](https://pmc.ncbi.nlm.nih.gov/articles/PMC8935530/)

**Also see:** Deci, E.L., Koestner, R., & Ryan, R.M. (1999). A meta-analytic review of experiments examining the effects of extrinsic rewards on intrinsic motivation. *Psychological Bulletin*, 125(6), 627–668.

### What the research says

Autonomy is one of three basic psychological needs in Self-Determination Theory (SDT). People sustain motivation when they feel their actions are self-chosen and personally endorsed. Bureau et al.'s meta-analysis of 144 studies (N > 79,000) found autonomy is the second strongest predictor of self-determined motivation (39% of predictive weight), after competence.

The Deci et al. (1999) meta-analysis found that tangible external rewards (grades, money, certificates) significantly undermine intrinsic motivation — especially for tasks people already find interesting.

### The honest caveats

- SDT defines autonomy as *volition and endorsement of one's actions* — not simply self-direction or choice. A learner can feel autonomous while following a structured curriculum if they chose that structure. Conversely, unlimited choice can reduce autonomy if it creates decision fatigue or anxiety.
- The evidence strongly supports autonomy *support* predicting motivation. The evidence that learner-controlled pacing and content sequencing directly improve *learning outcomes* (not just motivation) is weaker and more mixed. Novice learners given full control sometimes make poor sequencing decisions.
- Canon's skip/restore feature is the clearest implementation. But the deeper question is whether the student feels they own the curriculum — not just whether they can skip items.

### What Canon does today

Skip/restore on readings and watching items. Student chooses the curriculum, the pace, what to engage with deeply. Design mode lets the admin (currently only me) reshape the curriculum entirely.

**Canon status:** Yes

### Open questions

- Does the feeling of autonomy hold when the canonical curriculum is published by an admin? The student receives the curriculum rather than building it. Is that still autonomy-supportive?
- The AI curriculum generator (planned) would give students a curriculum generated for their specific context. Is that more autonomy-supportive than receiving a fixed curriculum? Probably yes — it's built around their stated goals.

---

## 5. Competence

**Evidence: H**
**Key paper:** Bureau, J.S. et al. (2022). Same meta-analysis as Autonomy above. [PMC link](https://pmc.ncbi.nlm.nih.gov/articles/PMC8935530/)

**Also see:** Howard, J.L., Slemp, G.R., & Wang, X. (2024/25). Need support and need thwarting: A meta-analysis in student populations. *Personality and Social Psychology Bulletin*. [PMC link](https://pmc.ncbi.nlm.nih.gov/articles/PMC12276404/)

### What the research says

Competence is the single strongest predictor of self-determined motivation in the Bureau et al. meta-analysis (42% of predictive weight). People sustain learning when they feel effective — when they experience genuine progress and mastery, not just activity.

### The honest caveats

- The SDT evidence is for *perceived competence* — the subjective feeling of being effective — not for visible progress metrics specifically. Progress bars are a design inference, not directly evidenced in the SDT literature.
- Perceived competence comes from mastery experiences (actually succeeding at challenges), not just tracking. A progress bar that shows 70% complete does not produce competence if the student doesn't feel they understood the material.
- There's a risk: if Canon's progress tracking rewards checkbox-ticking rather than genuine mastery, it could undermine competence by creating false signals.

### What Canon does today

Progress bars per module and overall. Module completion status. The Evaluate tab's Weak/Strong rubric is also a competence signal — it asks the student to honestly assess whether they reached the strong standard.

**Canon status:** Yes

### Open questions

- Is the current completion model producing genuine competence signals or just activity signals? "Module complete" requires the assignment to be done — that's a production signal, which is better than just "readings checked." But is the assignment standard high enough?
- The rubric (Weak/Strong/Ask Yourself) is a competence self-assessment. But self-assessment is notoriously inaccurate. Is there a way to make the rubric more calibrating — e.g., by comparing your self-assessment to a reference answer or worked example?

---

## 6. Elaborative interrogation

**Evidence: M**
**Key paper:** Dunlosky, J., Rawson, K.A., Marsh, E.J., Nathan, M.J., & Willingham, D.T. (2013). Improving students' learning with effective learning techniques. *Psychological Science in the Public Interest*, 14(1), 4–58. [Sage link](https://journals.sagepub.com/doi/abs/10.1177/1529100612453266)

### What the research says

Asking "why does this work?" — generating explanations for facts and concepts — produces better retention than summarising or re-reading. Dunlosky et al. rate it **moderate utility**.

### The honest caveats

- Works for declarative knowledge (facts, concepts) more reliably than for procedural or deep conceptual understanding.
- Depends heavily on prior knowledge. If you're entering a new domain, you can't generate accurate elaborations because you don't know enough to explain why things are true. The benefit shrinks for novice learners.
- "Beats summarising" is accurate. Does not beat retrieval practice.

### What Canon does today

The Reflection tab: an open prompt after every module asking for reflection on what you learned and why it matters. The prompt is not scripted — it's free-form, which is less targeted than proper elaborative interrogation but preserves autonomy.

**Canon status:** Yes

### Open questions

- Should the reflection prompt be more structured — explicitly asking "why does this matter?" rather than an open prompt? The literature suggests a more targeted why-question is more effective than a general reflection.
- The current prompt is the same across all modules. Module-specific reflection prompts tied to the module's content might be more effective.

---

## 7. Deliberate practice

**Evidence: M (contested)**
**Key paper:** Macnamara, B.N., Hambrick, D.Z., & Oswald, F.L. (2014). Deliberate practice and performance in music, games, sports, education, and professions: A meta-analysis. *Psychological Science*, 25(8), 1608–1618. [Sage link](https://journals.sagepub.com/doi/abs/10.1177/0956797614535810)

**Original theory:** Ericsson, K.A., Krampe, R.T., & Tesch-Römer, C. (1993). The role of deliberate practice in the acquisition of expert performance. *Psychological Review*, 100(3), 363–406.

### What the research says

Ericsson's original claim: deliberate practice — effortful, targeted practice at the edge of your current ability, with immediate feedback — is the primary driver of expert performance. The 10,000-hour rule is a pop-science version of this.

Macnamara et al.'s 2014 meta-analysis challenged this. Deliberate practice explained:
- 26% of variance in games (chess, etc.)
- 21% in music
- 18% in sports
- **4% in education**
- Less than 1% in professions

The dispute between Ericsson and Macnamara is live and unresolved. Ericsson argues the Macnamara meta-analysis misidentified studies that don't meet his definition of deliberate practice.

### The honest caveats

- The explained variance is much lower than the original Ericsson claims. Other factors — working memory capacity, age of starting, domain-specific aptitude — account for substantial variance.
- The framing "targeting specific weaknesses with feedback improves performance" is the most defensible version of the claim. It does not support the stronger claim that deliberate practice is the primary driver of expertise.
- More domain-dependent than other principles. Stronger in music and chess, weaker in professions and education — which is closer to what Canon is doing.

### What Canon does today

The Evaluate tab's Weak/Strong rubric identifies specific areas where the student fell short (the "weak" standard is designed to be recognisable and sting a little to read). This is the feedback component. But Canon doesn't have a structured practice loop — identify weakness, practice that specific thing, get feedback, repeat.

**Canon status:** WIP

### Open questions

- For PM skills, what would targeted deliberate practice actually look like? A rubric identifies a gap. But practicing that gap requires a task, feedback, and repetition. None of those are built.
- Is the artifact-first model a partial substitute? If your assignment came back weak, the implicit next step is to redo it at a higher standard. But Canon doesn't prompt this — it just lets you mark it done.

---

## What Canon is missing

The two highest-evidence techniques (retrieval practice, spaced practice) are the weakest in Canon's current implementation. Both are WIP or To be explored. The features that best implement the evidence (Evaluate tab Ask Yourself, pace note) are directionally right but not systematic.

A future Canon that builds proper retrieval and spacing mechanisms would have a much stronger claim to being grounded in cognitive science.

---

*Last updated: 2026-06-16. Sources verified against PubMed and publisher pages where accessible.*
