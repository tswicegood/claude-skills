---
name: book-report
description: "Produces comprehensive book reports with an 8-section structure (BLUF, overview, summary, action items, strategic applications, hidden gems, critical analysis, personalized recommendations). Use whenever the user asks for book research, a book report, a book analysis, a deep dive on a book, wants to \"research\" a specific title, says \"Research Book: [title]\", or asks what they should take away from a book. Also trigger when the user names a book title and asks for takeaways, insights, lessons, action items, or recommendations. Do NOT use for one-line book recommendations, reading-list generation, or book-purchase decisions — those don't need the full report."
---

# Book Report

Generate comprehensive, actionable book reports with strategic insights, immediate action items, overlooked insights, and personalized recommendations.

## Trigger Phrases

Activate this skill when the user says:
- "Research Book: {Book Title}"
- "Create a book report for {Book Title}"
- "Analyze {Book Title}"
- Similar requests for book analysis or research

## Context Adaptation

This skill adapts to the project context (professional/business, personal development, academic, hobby). Infer context from:
- Project name and description
- Conversation history
- Project Knowledge documents
- Topics being discussed

**For detailed adaptation guidance**, read `references/context_adaptation.md` for tailoring tone, action items, and recommendations to specific contexts.

## Workflow

### Step 1: Confirm Book

When the user requests a book report:

1. Perform a web search for the book
2. Present findings with:
   - Full title
   - Author name
   - Publication date (original and latest edition if applicable)
3. Ask user to confirm this is the correct book
4. Wait for confirmation before proceeding to Step 2

**Example response format:**
```
I found the following book:
- **Title:** [Full Title]
- **Author:** [Author Name]
- **Publication Date:** [Date]

Is this the correct book you'd like me to research?
```

### Step 2: Adaptive Research Protocol

Once confirmed, execute the following structured research process:

**PHASE 1: RECONNAISSANCE (Max 5 searches)**
- Search for: [book title + author], [book title + reviews], [book title + summary]
- Assess information density:
  * HIGH: 3+ detailed sources found (professional reviews, author interviews, critical analysis)
  * MEDIUM: 2-3 basic sources (publisher description, brief reviews, goodreads)
  * LOW: 0-1 sources or only announcement/pre-release info

**DECISION CHECKPOINT:**
- IF information density = HIGH → Research Mode OFF (sufficient data in 10-15 total searches)
- IF information density = MEDIUM → Research Mode PARTIAL (up to 20 searches, focus on depth)
- IF information density = LOW → Research Mode OFF (stop at 15 searches, note limited availability)

Explicitly state your decision and reasoning before proceeding to Phase 2.

**PHASE 2: DEEP RESEARCH**
- Execute based on decision from checkpoint
- Gather information from: book reviews (professional and reader), academic analyses, expert commentary, author interviews, summary resources, related discussions
- Stop early if searches return redundant information

Proceed to Step 3 to write the report using the structure below.

### Step 3: Write the Report

Decide mode first:

- **Verbose mode** — triggered when the user's request includes any of: "detailed," "comprehensive," "full," "thorough," or "wordy." Roughly double the per-section targets below and skip the total-length cap.
- **Concise mode** (default) — hit the per-section targets below on the first pass. Do not draft-then-edit; write to length directly.

**Per-section length targets (concise mode):**

- Bottom Line Up Front: 80–120 words, one paragraph
- Book Overview: 2–3 sentences
- Comprehensive Summary: 4–5 concepts, 100–150 words per concept
- Immediate Action Items: 5–7 items, 2–3 sentences each
- Strategic Applications: 3–4 principles, ~100 words each
- Hidden Gems: minimum 5 takeaways, one tight paragraph each
- Critical Analysis: 150–250 words total, covering all four sub-points
- Personalized Recommendations: 2–3 books with 2–3 sentence rationale each

**Target total (concise mode): ~2,500–3,500 words. Verbose mode: ~5,000–7,000 words.**

Quality standards for writing to target:

1. Every sentence serves a clear purpose
2. No repetition of the same insight in different sections
3. Maintain the full 8-section structure regardless of mode
4. Preserve context-specific adaptations from `references/context_adaptation.md`
5. Keep all strategic insights from the "Hidden Gems" section

After writing (in either mode), save the report as a file and present it to the user per the Response Format section below. Also surface the one-paragraph BLUF summary inline in the chat response.

## Report Structure

### 1. Bottom Line Up Front Summary (1 paragraph)

Synthesize the book's most transformative insight and core value proposition. Answer: Why should someone read this book?

Include this paragraph both at the beginning of the artifact AND in the main response.

### 2. Book Overview (2-3 sentences)
- Core thesis and main themes
- Target audience and historical context
- Why this book matters

### 3. Comprehensive Summary
Break down into 4-5 key concepts or sections:
- Main idea for each concept
- Supporting evidence and arguments
- Relevant examples or case studies the author uses
- How concepts interconnect

### 4. Immediate Action Items (5-7 specific actions)
List concrete, implementable steps for the next 30 days:
- **Action:** Clear, specific step
- **Why it's impactful:** Evidence or reasoning
- **How to implement:** Practical guidance
- **Priority order:** From easiest to most challenging

Adapt actions to project context (business implementations vs. personal habits vs. research applications vs. hobby skills).

### 5. Strategic Applications (3-4 broader concepts)
Longer-term strategies and mindset shifts:
- Key principle or strategy
- How it applies to relevant domain (business/personal/academic/hobby)
- Potential obstacles and solutions
- How to measure progress or success

### 6. Hidden Gems: Overlooked Insights (minimum 5 takeaways)
Critical thinking required - identify insights that most readers miss:
- Ideas mentioned but not emphasized in typical reviews
- Subtle concepts often overlooked on first reading
- Counter-intuitive insights challenging conventional wisdom
- Connections between different parts of the book
- Small details with significant implications
- Nuances in the author's arguments

**Think deeply:** What would a casual reader miss? What do reviews underemphasize?

### 7. Critical Analysis
Balanced evaluation:
- **Strongest arguments:** What does the book do exceptionally well?
- **Weaknesses or gaps:** Are there limitations, biases, or criticisms from experts?
- **Comparative context:** How does this compare to similar works?
- **Applicability:** When is this book most/least useful?

### 8. Personalized Recommendations

Based on the user's inferred profile and project context:
- **2-3 complementary books:** Titles that extend or complement this book's ideas
- **Why these books:** How each recommendation connects
- **Most valuable insights:** Which parts of this book are most relevant to this specific user/context

Tailor recommendations to context:
- **Business:** Industry-relevant, practical business books
- **Personal:** Growth-oriented, habit/mindset books
- **Academic:** Scholarly works, research methodologies
- **Hobby:** Skill-building, practitioner guides

## Quality Standards

- **Adaptive research:** Follow the phased research protocol with density assessment and appropriate search limits
- **Original synthesis:** Avoid merely aggregating summaries - provide unique insights
- **Actionability:** Make every recommendation concrete and implementable
- **Context awareness:** Seamlessly adapt tone and focus to project context
- **Citation quality:** Focus on authoritative sources (author interviews, scholarly analyses, reputable publications)
- **Depth over breadth:** Better to deeply analyze 4-5 key concepts than superficially cover 10

## Response Format

After completing research:

1. **Main response:** Brief confirmation + one-paragraph BLUF summary + link to artifact
2. **Artifact:** Full markdown report with all 8 sections

**Example main response:**
```
I've completed comprehensive research on [Book Title] by [Author] and edited it for conciseness while preserving all critical insights.

[One-paragraph BLUF summary]

[View your complete book report](computer://...)
```

## Common Pitfalls to Avoid

- Don't just summarize the book - provide analysis and synthesis
- Don't ignore the user's context - adapt recommendations
- Don't list obvious insights - dig for hidden gems
- Don't copy book summaries verbatim - paraphrase and cite
- Don't skip the confirmation step - verify the correct book first

