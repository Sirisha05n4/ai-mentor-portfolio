# Day 2 — Lab 2A: Six-Pattern Drills

This document contains the six structurally distinct prompt patterns used to explain Big-O notation to a B.Tech placement candidate, along with their evaluation scores.

---

## Pattern 1: PERSONA

### Prompt
> You are a senior placement coach at Aditya University. You have prepared 200 B.Tech CSE students for TCS, Infosys, and Cognizant placement interviews. A 3rd-year CSE student with 7.5 CGPA asks you about Big-O notation. Explain it the way you would in a 1-on-1 prep session — concrete, conversational, exam-relevant.

### Output
> Alright, let's break this down. In your TCS or Infosys interviews, they aren't just looking for textbook definitions. Think of Big-O as the speed limit of your code. It tells us how the execution time grows as the size of your input ($n$) increases. For example, search a list of 10 items vs 10 million. If you use a single loop to find a number, that's $O(n)$—it scales linearly with the input size. If you use nested loops, that's $O(n^2)$—which gets dangerously slow as the list grows. In your interview, explain this using the "nested loops" analogy, and always mention that space complexity is just as important as time complexity!

### Evaluation
* **Self-Score:** 5/5 (Tailored vocabulary, appropriate style constraints, and clear context)
* **Peer-Score:** 9/10
  * **Clarity:** 2/2
  * **Context:** 2/2
  * **Specificity:** 2/2
  * **Format:** 2/2
  * **Verification:** 1/2 (No external verification step included in the prompt)

---

## Pattern 2: FEW-SHOT

### Prompt
> Here are example explanations from a placement coach:
> 
> Q: What is recursion?
> A: A function calling itself with a smaller problem. Like Russian dolls — open one, find a smaller one inside. Stops at base case. Example: factorial.
> 
> Q: What is a stack?
> A: LIFO data structure. Push to add, pop to remove. Like a stack of plates — you take from the top.
> 
> Q: What is Big-O notation?
> A:

### Output
> A way to measure code efficiency. Scales as input size ($n$) grows. Like sorting books—faster methods take less time. Example: searching an array one-by-one is $O(n)$, binary search is $O(\log n)$.

### Evaluation
* **Self-Score:** 5/5 (Perfectly copied the style and length constraints of the examples)
* **Peer-Score:** 8/10
  * **Clarity:** 2/2
  * **Context:** 1/2 (Relies purely on prompt-level examples rather than explicit situational context)
  * **Specificity:** 2/2
  * **Format:** 2/2
  * **Verification:** 1/2

---

## Pattern 3: CHAIN-OF-THOUGHT

### Prompt
> Explain Big-O notation to a placement interview candidate. Think step by step before answering.
> Step 1: define Big-O in one sentence.
> Step 2: give one concrete example with code.
> Step 3: explain why interviewers ask about it.
> Step 4: synthesise into a 3-sentence summary.

### Output
> **Step 1: Definition**
> Big-O notation is a mathematical tool used to describe the upper bound (worst-case scenario) of an algorithm's running time or space requirements as a function of the input size.
> 
> **Step 2: Code Example**
> Here is a Python comparison of $O(1)$ and $O(n)$:
> ```python
> # O(1) - Constant Time (Fast, scales instantly)
> def get_first_item(items):
>     return items[0]
> 
> # O(n) - Linear Time (Time grows proportionally with input size)
> def print_all_items(items):
>     for item in items:
>         print(item)
> ```
> 
> **Step 3: Why Interviewers Ask**
> Interviewers ask about Big-O because it proves you write scalable code for production, rather than code that works only on small test datasets but crashes in real systems under load.
> 
> **Step 4: Synthesis**
> Big-O is the standard language for comparing algorithm efficiency without relying on hardware speed. By knowing common complexities like $O(n)$ or $O(n^2)$, you can optimize code structure early. Understanding both time and space complexities shows you are a mature engineer ready for production-level systems.

### Evaluation
* **Self-Score:** 5/5 (Explicitly forced step-by-step reasoning structure, resulting in a logical and easy-to-follow response)
* **Peer-Score:** 9/10
  * **Clarity:** 2/2
  * **Context:** 2/2
  * **Specificity:** 2/2
  * **Format:** 2/2
  * **Verification:** 1/2

---

## Pattern 4: STRUCTURED OUTPUT

### Prompt
> Explain Big-O notation for a placement interview. Return ONLY valid JSON with this exact shape — no markdown fences, no commentary:
> 
> {
>   "definition": "one-sentence definition",
>   "intuition": "concrete analogy",
>   "code_example": "Python snippet showing O(n) and O(n²)",
>   "common_pitfall": "what students get wrong",
>   "interview_test_question": "a follow-up question an interviewer might ask"
> }

### Output
```json
{
  "definition": "Big-O notation is a mathematical notation that describes the limiting behavior of a function when the argument tends towards a particular value or infinity, used in computer science to analyze algorithm efficiency.",
  "intuition": "It is like comparing different modes of travel: walking (constant or linear time depending on distance) vs. high-speed rail (highly efficient over long distances).",
  "code_example": "O(n) example: `for item in items: print(item)`. O(n²) example: `for x in items: for y in items: print(x, y)`.",
  "common_pitfall": "Confusing best-case, average-case, and worst-case complexities (Big-O specifically denotes the upper bound / worst-case scenario).",
  "interview_test_question": "What is the time complexity of searching for an element in a balanced binary search tree vs. an unsorted array?"
}
```

### Evaluation
* **Self-Score:** 5/5 (Output matches schema perfectly)
* **Peer-Score:** 10/10
  * **Clarity:** 2/2
  * **Context:** 2/2
  * **Specificity:** 2/2
  * **Format:** 2/2 (Enforces strict JSON schema)
  * **Verification:** 2/2 (Follow-up interview question acts as verification)

---

## Pattern 5: SYSTEM PROMPT

### System Prompt (Configured in Custom Instructions)
> You are a senior placement coach for B.Tech CSE students at an Indian engineering college. You prepare students for TCS / Infosys / Cognizant interviews. You explain technical concepts in 50-80 words max, conversationally, with one concrete code example.

### User Prompt
> Explain Big-O notation.

### Output
> Big-O notation measures how run-time scales with input size.
> Code example:
> ```python
> # O(n) Time Complexity
> for i in range(n):
>     print(i)
> ```
> Keep it simple: $O(1)$ is instant, $O(n)$ grows with size, $O(n^2)$ scales quadratically (avoid nested loops in interviews!).

### Evaluation
* **Self-Score:** 5/5 (Kept output extremely concise and tailored to the system role)
* **Peer-Score:** 9/10
  * **Clarity:** 2/2
  * **Context:** 2/2
  * **Specificity:** 2/2
  * **Format:** 2/2
  * **Verification:** 1/2

---

## Pattern 6: PROMPT CHAINING

### Prompt 1 (Extract)
> List the 5 most important sub-concepts a B.Tech student must understand about Big-O notation. Just the list, no explanation.

#### Prompt 1 Output
> 1. Time vs. Space Complexity
> 2. Constant, Linear, and Quadratic complexities ($O(1)$, $O(n)$, $O(n^2)$)
> 3. Worst-case scenario (Upper bound)
> 4. Dropping constants and non-dominant terms
> 5. Analogy comparing algorithm efficiency

### Prompt 2 (Expand)
> For each of these 5 sub-concepts of Big-O notation, write a 1-paragraph explanation tailored to a 3rd-year B.Tech CSE student preparing for placement interviews:
> 
> 1. Time vs. Space Complexity
> 2. Constant, Linear, and Quadratic complexities ($O(1)$, $O(n)$, $O(n^2)$)
> 3. Worst-case scenario (Upper bound)
> 4. Dropping constants and non-dominant terms
> 5. Analogy comparing algorithm efficiency

#### Prompt 2 Output
> 1. **Time vs. Space Complexity:** Time complexity measures the number of operations/steps an algorithm takes, while space complexity refers to the auxiliary memory allocated during execution. Recruiters look for candidates who can optimize both.
> 2. **Common Complexities:** $O(1)$ means execution time is independent of input size. $O(n)$ means time scales linearly with input size (a single loop). $O(n^2)$ means time scales quadratically (nested loops), which should be avoided on large inputs.
> 3. **Worst-Case Scenario:** Big-O focuses on the upper bound, meaning the maximum possible operations an algorithm will execute. This ensures your software behaves predictably under heavy loads.
> 4. **Dropping Terms:** We only keep the term with the highest growth rate. For example, $O(n^2 + 5n + 10)$ simplifies to $O(n^2)$ because as $n$ approaches millions, the lower terms become negligible.
> 5. **Efficiency Analogy:** Think of $O(1)$ as sending an email (constant time regardless of distance), $O(n)$ as delivering physical mail door-to-door, and $O(n^2)$ as comparing every mail item against every other mail item.

### Prompt 3 (Polish)
> Synthesise these 5 paragraphs into ONE concise 80-word interview-prep explanation of Big-O notation. Preserve all 5 sub-concepts. Conversational tone. End with one practice question.
> 
> [Paste Prompt 2 Output]

#### Prompt 3 Output
> Big-O measures how algorithm runtime and memory scale in the worst-case. We drop constants to focus on dominant growth rates: $O(1)$ is constant, $O(n)$ is linear, and $O(n^2)$ (nested loops) is quadratic. Think of $O(1)$ as emailing a file and $O(n)$ as mailing it door-to-door. Understanding this helps you write scalable code recruiters value.
> 
> *Practice question:* What is the time complexity of searching a value in a sorted array using binary search?

### Evaluation
* **Self-Score:** 5/5 (Produced the highest quality summary by decomposing the task)
* **Peer-Score:** 10/10
  * **Clarity:** 2/2
  * **Context:** 2/2
  * **Specificity:** 2/2
  * **Format:** 2/2
  * **Verification:** 2/2

---

## Conclusion

For my placement-prep students, the patterns I will use most are **FEW-SHOT** and **PROMPT CHAINING**, because few-shot prompts teach students how to model their coding answers after high-scoring examples, while prompt chaining breaks down complex database design or system architectures into digestible, step-by-step sub-tasks that build confidence and produce superior outputs.
