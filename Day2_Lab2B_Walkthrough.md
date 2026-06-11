# Day 2 — Lab 2B: JSON Résumé Extractor (Turnkey Walkthrough)

**Time:** 90 minutes (14:00 — 15:30)
**Format:** Individual at Colab; trainer demos cell 1 then walks the room
**Goal:** Working notebook that processes 3 sample résumés through Gemini's structured-output API, validates with Pydantic, handles 1 broken input gracefully.

---

## Setup (5 min)

Each mentor opens:
- Colab → **File → Open** → `Day2_ResumeExtractor.ipynb` from the lab kit
- Their `data/sample_resumes.txt` (5 anonymised résumés in the kit)
- 1Password to access Gemini key

---

## Step 1 — Install + load API key (5 min)

Mentors run cell 1 (already in notebook):

```python
!pip install -q google-genai pydantic
import os, getpass
if 'GEMINI_API_KEY' not in os.environ:
    os.environ['GEMINI_API_KEY'] = getpass.getpass('Gemini API key: ')
```

**Acceptance:** No error. Mentor pastes key when prompted.

---

## Step 2 — Define the Pydantic schema (10 min)

Cell 2:

```python
from pydantic import BaseModel
from typing import List, Optional

class Education(BaseModel):
    degree: str
    institution: str
    year: int

class Resume(BaseModel):
    name: str
    email: str
    phone: Optional[str] = None
    education: List[Education]
    skills: List[str]
    projects: List[str] = []
    experience_years: float
```

**Trainer note:** The `Optional[str] = None` on phone is critical. Some sample résumés have no phone — without Optional, Pydantic raises. Walk the room and confirm every mentor has `Optional`.

**Acceptance:** Cell runs. No errors.

---

## Step 3 — Extract function with retry on failure (15 min)

Cell 3:

```python
from google import genai
from pydantic import ValidationError

client = genai.Client(api_key=os.environ['GEMINI_API_KEY'])

def extract_resume(raw_text: str, max_retries: int = 1) -> Resume:
    for attempt in range(max_retries + 1):
        try:
            resp = client.models.generate_content(
                model='gemini-2.5-flash',
                contents=f'Extract a Resume JSON from this text. Return ONLY JSON, no markdown.\n\n{raw_text}',
                config={
                    'response_mime_type': 'application/json',
                    'response_schema': Resume.model_json_schema(),
                },
            )
            return Resume.model_validate_json(resp.text)
        except ValidationError as e:
            if attempt == max_retries:
                raise
            # Retry once with the broken JSON in the prompt
            fix_prompt = (f'Fix this JSON to match schema. Errors: {e}. '
                          f'Original: {resp.text}')
            resp = client.models.generate_content(
                model='gemini-2.5-flash', contents=fix_prompt,
                config={
                    'response_mime_type': 'application/json',
                    'response_schema': Resume.model_json_schema(),
                },
            )
            return Resume.model_validate_json(resp.text)
```

**Trainer note:** The retry path handles ~80% of malformed-output cases. Without it, ~5% of calls fail in production.

**Acceptance:** Function defined. No syntax error.

---

## Step 4 — Run on 3 sample résumés (15 min)

Cell 4:

```python
# Load sample résumés from the lab kit
with open('../data/sample_resumes.txt') as f:
    resumes = [r.strip() for r in f.read().split('---') if r.strip()]

print(f'Loaded {len(resumes)} sample résumés')

results = []
for i, r in enumerate(resumes[:3]):
    try:
        parsed = extract_resume(r)
        results.append(parsed)
        print(f'\nRésumé {i+1}: {parsed.name} — {len(parsed.skills)} skills, '
              f'{parsed.experience_years} years exp')
    except Exception as e:
        print(f'\nRésumé {i+1}: FAILED — {type(e).__name__}: {str(e)[:200]}')

# Print full first result
if results:
    print('\n=== Full first result ===')
    print(results[0].model_dump_json(indent=2))
```

### Expected output

For sample résumé 1 (Ravi Kumar):
```
Résumé 1: Ravi Kumar — 6 skills, 1.0 years exp
```

For sample résumé 2 (Sneha Reddy):
```
Résumé 2: Sneha Reddy — 6 skills, 0.5 years exp
```

For sample résumé 3 (Arun Pillai):
```
Résumé 3: Arun Pillai — 9 skills, 1.0 years exp
```

(Exact skill counts may vary — Gemini has some flexibility in what it counts as a "skill" vs "experience".)

**Acceptance:** All 3 print without crash. The full first result shows valid JSON with all required fields populated.

---

## Step 5 — Test broken input (10 min)

Cell 5:

```python
# Empty string — should fail gracefully, not crash
try:
    bad = extract_resume('')
    print('Unexpected success:', bad.model_dump_json())
except Exception as e:
    print('Caught gracefully:', type(e).__name__)
    print('Message:', str(e)[:200])
```

### Expected output

```
Caught gracefully: ValidationError
Message: 1 validation error for Resume
name
  Field required ...
```

**Acceptance:** No traceback escapes; the function raises a handled exception.

---

## Step 6 — Document the 3 errors handled (10 min)

In the notebook README cell, mentor writes:

```markdown
## Day 2 Lab 2B — Errors handled

1. **Markdown fence wrapping** (`\`\`\`json ... \`\`\``)
   The retry prompt asks Gemini to output raw JSON without fences. Triggers on ~5-10% of calls.

2. **Hallucinated phone number when source has none**
   `Optional[str] = None` in Pydantic — model returns `null`, schema validates.

3. **Empty / whitespace-only input**
   Pydantic raises ValidationError with "Field required". Caller catches.

## Sample résumés processed: 3 / 3 successful
```

Push to GitHub: `Day2_ResumeExtractor.ipynb` + updated README.

---

## Common bugs + recovery

- **`Pydantic ValidationError: name Field required`** with a real résumé → the model decided the résumé was too sparse. Check the raw text — if the name is on line 1 and Gemini missed it, the prompt needs to be clearer (e.g., "the first line is the name").
- **Markdown fences in output** despite mime type → retry path handles. If still failing on retry, set temperature=0 in config: `'temperature': 0`.
- **`429 Resource exhausted`** mid-batch → backup Gemini key from 1Password OR switch to Groq via Day 11 fallback chain (preview).
- **Pydantic schema with required `phone: str`** → Optional. Walk back to step 2.
- **Notebook can't find `../data/sample_resumes.txt`** → mentor copied the notebook into a different folder. They drag-drop the kit's data folder into the same Colab session.

---

## Trainer notes

1. The teaching moment for the day: `response_schema` constrains the model at the decoding level. "Please return JSON" in the prompt is hope. `response_schema` is engineering. Show the difference live by removing `response_schema` from cell 3 and running on résumé 4 (Priya Nair, no email line) — model usually invents an email. Then put `response_schema` back; it now returns proper Optional / null.
2. When a mentor's call hits 429 mid-Step 4 and they panic — don't fix it. Walk through the rate-limit reality. "This is the 2026 free-tier reality. Tomorrow we add Groq fallback."
3. Pair-debug rule: if a mentor is stuck more than 10 minutes, pair them with someone whose code is working. Pair-debugging is faster than trainer-walks-over.
4. Acceptance verification at 15:25: ask each mentor to show the cell-4 output on the projector for 30 seconds. If 3 résumés processed, pass. If not, 5 min catch-up.

---

## Acceptance check (final 5 min)

For each mentor, verify:
- ✅ `Day2_ResumeExtractor.ipynb` runs end-to-end without errors
- ✅ 3 sample résumés processed successfully
- ✅ Empty-input case handled gracefully (ValidationError caught)
- ✅ README documents the 3 errors handled with technical detail
- ✅ Notebook pushed to GitHub

If any item missing, pair the mentor for the last 5 minutes. The Pydantic + Gemini pattern is the foundation for Day 6 capstone Sprint 1 (PlacementDataProcessor uses the same shape with a JD schema instead of Resume).
