# Day 2 — Lab 2A: Six-Pattern Drills (Turnkey Walkthrough)

**Time:** 90 minutes (11:15 — 13:00)
**Format:** Individual writing + paired peer-scoring
**Goal:** Each mentor rewrites the same student question SIX structurally distinct ways (one per pattern) and peer-scores using the 5-criterion rubric.

---

## Setup (5 min)

Each mentor opens:
- ChatGPT free + Claude.ai free in separate tabs (same session)
- A scratch doc / Google Doc titled `Day2_SixPatterns_<name>.md`
- The 5-criterion rubric printed and on their desk (or `templates/teach_back_rubric_14pt.md` open)

---

## The student question (everyone uses the SAME question)

> "Explain Big-O notation for a placement interview."

(Mentors may substitute a real student question from their subject, but for the lab the default is Big-O so peer-scoring is comparable.)

---

## Step 1 — Pattern 1: PERSONA (10 min)

**Goal:** Add a specific, named persona that constrains voice + audience assumptions.

**Bad** (do not write this): "You are a teacher. Explain Big-O."

**Good** (mentor writes):

> "You are a senior placement coach at Aditya University. You have prepared 200 B.Tech CSE students for TCS, Infosys, and Cognizant placement interviews. A 3rd-year CSE student with 7.5 CGPA asks you about Big-O notation. Explain it the way you would in a 1-on-1 prep session — concrete, conversational, exam-relevant."

Run on Claude free. Save the response in your scratch doc as **Pattern 1 — PERSONA: <output>**.

Score 1-5 on usefulness for a real 3rd-year B.Tech student. Note the score below the response.

---

## Step 2 — Pattern 2: FEW-SHOT (10 min)

**Goal:** Show 2-3 example Q&A pairs of the desired output shape, then ask the new question.

Write this prompt:

> "Here are example explanations from a placement coach:
>
> Q: What is recursion?
> A: A function calling itself with a smaller problem. Like Russian dolls — open one, find a smaller one inside. Stops at base case. Example: factorial.
>
> Q: What is a stack?
> A: LIFO data structure. Push to add, pop to remove. Like a stack of plates — you take from the top.
>
> Q: What is Big-O notation?
> A:"

Run on Claude. The model copies the format — short, analogy-driven, concrete example.

Score 1-5.

---

## Step 3 — Pattern 3: CHAIN-OF-THOUGHT (10 min)

**Goal:** Force step-by-step reasoning before the answer.

Prompt:

> "Explain Big-O notation to a placement interview candidate. Think step by step before answering. Step 1: define Big-O in one sentence. Step 2: give one concrete example with code. Step 3: explain why interviewers ask about it. Step 4: synthesise into a 3-sentence summary."

Run on Claude. Note that the response shows the steps explicitly (more text, but more transparent).

Score 1-5.

---

## Step 4 — Pattern 4: STRUCTURED OUTPUT (10 min)

**Goal:** Force JSON with a defined schema.

Prompt:

> "Explain Big-O notation for a placement interview. Return ONLY valid JSON with this exact shape — no markdown fences, no commentary:
>
> {
>   "definition": "one-sentence definition",
>   "intuition": "concrete analogy",
>   "code_example": "Python snippet showing O(n) and O(n²)",
>   "common_pitfall": "what students get wrong",
>   "interview_test_question": "a follow-up question an interviewer might ask"
> }"

Run on Claude. Note that Claude.ai sometimes wraps in markdown ```json fences anyway — flag this for tomorrow's Pydantic-validated lab (Day 2 Lab 2B).

Score 1-5.

---

## Step 5 — Pattern 5: SYSTEM PROMPT (10 min)

**Goal:** Use a short reusable role + a short variable user task.

In Claude free, click **Custom Instructions** (Profile → Settings → Customize). Set:

> System (paste in Custom Instructions):
> "You are a senior placement coach for B.Tech CSE students at an Indian engineering college. You prepare students for TCS / Infosys / Cognizant interviews. You explain technical concepts in 50-80 words max, conversationally, with one concrete code example."

Now in a new chat, ask:

> User: "Explain Big-O notation."

The output should be much shorter and more domain-aware than Pattern 1, because the system prompt does the heavy lifting and the user prompt stays minimal.

Score 1-5.

---

## Step 6 — Pattern 6: PROMPT CHAINING (10 min)

**Goal:** Decompose into 3 sequential prompts. Each call has one job.

Open 3 separate Claude conversations.

**Conversation 1 (Extract):**

> "List the 5 most important sub-concepts a B.Tech student must understand about Big-O notation. Just the list, no explanation."

Save the list (will be 5 items like: definition, common time complexities, comparing algorithms, space vs time, why interviewers ask).

**Conversation 2 (Expand):**

> "For each of these 5 sub-concepts of Big-O notation, write a 1-paragraph explanation tailored to a 3rd-year B.Tech CSE student preparing for placement interviews:
>
> [paste the 5 from Conversation 1]"

Save the 5 paragraphs.

**Conversation 3 (Polish):**

> "Synthesise these 5 paragraphs into ONE concise 80-word interview-prep explanation of Big-O notation. Preserve all 5 sub-concepts. Conversational tone. End with one practice question.
>
> [paste 5 paragraphs]"

Save the final output.

Note: 3 calls, 3 outputs. Higher quality than any single prompt could deliver.

Score 1-5.

---

## Step 7 — Pair-score (15 min)

Pair up. Swap your scratch docs with your neighbour.

Each mentor scores the OTHER mentor's six prompts on the 5-criterion rubric:

| Criterion | 0-2 | What you check |
|-----------|-----|----------------|
| Clarity | | Single intent, no "do everything" prompts |
| Context | | Audience + situation supplied |
| Specificity | | Concrete subject + scope |
| Format | | Output shape requested |
| Verification | | Citations or follow-up verify step |

Total = 10 per prompt. Pass = 7.

**Trainer note:** Pair-scoring inflates by 1+ point on average vs self-scoring. That's why we pair. After pair-scoring, ask 2 pairs publicly: "Where did you disagree?"

---

## Step 8 — Surface the best (10 min)

At 12:50, surface 2 mentors' highest-scoring rewrite per pattern on the projector. Compare. The room sees what "good Persona prompt" vs "good Chain-of-Thought prompt" actually looks like.

---

## Common bugs + recovery

- **Mentor writes 6 wording variations of the same pattern** → reject. Force structural difference. Persona vs Few-Shot is structurally different (one declares a role, the other shows example Q&A pairs); they should look NOTHING alike.
- **Self-scoring inflated** → pair-scoring is the fix.
- **Mentor uses paid Claude Pro for the lab** → fine for personal use, but require they screenshot the same 6 prompts on free Claude.ai. The bootcamp standard is reproducible on free tools.
- **Mentor says "all six gave similar answers"** → push back. They probably wrote weak prompts. Ask: "Where in your Pattern 4 did you specify the JSON schema?" If they didn't, the lab is about to teach them why structure matters.

---

## Trainer notes

1. Walk the room. Watch for cosmetic rewrites masquerading as pattern differences.
2. The teaching moment for Pattern 4 (structured output) is when Claude.ai wraps in ```json fences despite being asked not to. Flag this — tomorrow we fix it with response_schema + Pydantic.
3. The teaching moment for Pattern 6 (chaining) is when mentors realise 3 short prompts beat 1 long prompt for complex tasks. Don't say it; let them discover it.
4. If a mentor finishes by 12:30, give them the stretch goal: write a 7th pattern — SELF-CONSISTENCY (run the CoT prompt 5 times, majority-vote the answer). Used for math/logic tasks.

---

## Acceptance check

Push to `Day2_SixPatterns.md` in the ai-mentor-portfolio repo:
- ✅ Six prompts, clearly labelled by pattern
- ✅ Best output per pattern
- ✅ Self-score for each (1-5)
- ✅ Peer-score for each (sum of 5 criteria, out of 10)
- ✅ One paragraph at the bottom: "For my placement-prep students, the patterns I will use most are X and Y, because ___."

If a mentor cannot articulate which patterns they would use with students, the lab has not landed. Pair them with you for 5 minutes after lab.
