# Session Log Example: 4-Voice Verification Pipeline

> Sanitized excerpts from production coding sessions on the QUANTAREON platform.
> Real tasks, real outputs, with the 4-Voice pipeline visible in each one.
> The four voices are the methodology's internal kitchen — what matters
> is what they do: catch wrong plans, side effects, scope creep, and
> over-engineering before any code is executed.

---

## Session 1: Screenshot Module

**Context.** Production screenshot capture using SnapDOM. Server processes images with size limits.

**Request.**
> "Change `scale: 2` to `scale: 1` in the snapdom config. Don't touch DOM classes."

### Pipeline

```
[Proposal]
  Change scale: 2 → scale: 1 in snapdom configuration object.
  One parameter, in quantarion-vision.js.

[Risk Check]
  Output image dimensions halve.
  JPEG filesize depends on scale — verify output stays
  under MAX_IMAGE_SIZE (500KB).
  JPEG_QUALITY (0.92) is independent of scale — no side effect.

[Scope Guard]
  Scale change. Nothing else.
  Do NOT touch:
  - DOM classes (user explicitly said no)
  - CAPTURE_DELAY (separate parameter, not requested)
  - JPEG_QUALITY (working fine, no reason to touch)

[Minimal Action]
  One str_replace.
  File: webapp/quantarion-vision.js
  Lines affected: 1
```

### Result
- One str_replace, executed
- Nothing else touched
- Worked on first try
- User confirmed

### What a normal LLM assistant would do

1. Change `scale: 2` → `scale: 1` ✓
2. Also bump `CAPTURE_DELAY` from 3000 to 5000 ✗
3. Refactor the surrounding function "for clarity" ✗
4. Adjust `JPEG_QUALITY` "to compensate for lower scale" ✗
5. Add comments explaining the change ✗

Items 2–5 are scope violations. Each one is a chance to break working code.

---

## Session 2: Database Migration

**Context.** PostgreSQL with asyncpg. Users table, evolving schema, production data.

**Request.**
> "Add column `daily_chat_pairs_count INT DEFAULT 0` to users table."

### Pipeline

```
[Proposal]
  ALTER TABLE users ADD COLUMN IF NOT EXISTS
  daily_chat_pairs_count INT DEFAULT 0;
  Add to init_db() migration block.

[Risk Check]
  IF NOT EXISTS — survives re-deploy.
  DEFAULT 0 — existing rows get valid value.
  INT — fits a daily counter (max ~2B).
  No index — field isn't queried in WHERE.

[Scope Guard]
  One column. That's the whole task.
  Do NOT:
  - Modify other columns
  - Add related columns ("chat_pairs_limit") preemptively
  - Restructure the migration block
  - Add reset logic (not asked)

[Minimal Action]
  One line into init_db():
  await conn.execute("ALTER TABLE users ADD COLUMN
  IF NOT EXISTS daily_chat_pairs_count INT DEFAULT 0")
  No other files touched.
```

### Result
- Single ALTER TABLE
- No schema side effects
- Backward compatible
- Production-safe via IF NOT EXISTS

---

## Session 3: Context Builder

**Context.** AI context assembly. Raw text from multiple sources needs cleaning before it hits the LLM.

**Request.**
> "Add tag stripping to context_builder.py — strip custom tags, normalize whitespace, trim to 150 chars."

### Pipeline

```
[Proposal]
  Regex _TAG_RE strips [...] tags from text.
  Pipeline: strip tags → normalize whitespace → trim to 150.
  Private method _clean_context_text().

[Risk Check]
  Regex must not eat valid content inside brackets.
  \[.*?\] with re.DOTALL would match across lines — bad.
  Better: \[[A-Z_]+:.*?\] — matches only uppercase tag patterns.
  Order matters: strip → normalize → trim. Wrong order = broken output.

[Scope Guard]
  Tag stripping + whitespace + trim. That's it.
  Do NOT:
  - Touch tag generation upstream (separate concern)
  - Change how context is assembled (different function)
  - Add logging (not asked)
  - Touch the 150 char limit (user specified it)

[Minimal Action]
  _TAG_RE compiled regex (1 line)
  _clean_context_text() method (4 lines)
  One call site in the pipeline (1 line)
  Total: 6 added, 0 modified.
```

### Result
- Focused change, nothing extra
- Right order: strip → normalize → trim
- Safe regex, not over-eager
- No upstream/downstream damage

---

## Pattern Summary

Across 500+ production sessions on multiple frontier models from different vendors, the four voices catch the same failure classes consistently:

| Voice | Catches |
|-------|---------|
| **Proposal** | Wrong plan, misread intent |
| **Risk Check** | Side effects, dependency breaks |
| **Scope Guard** | Unauthorized "improvements" |
| **Minimal Action** | Over-engineering |

**Risk Check and Minimal Action carry the analytical load** — they're the heavy-lifters of the pipeline. Scope Guard fires as a thin sanity-check against compliance bias and scope overstepping. Proposal is the trigger that starts the process; its job is to launch, not to think.

The exact load distribution depends on the **innate temperament** of the model running the simulation — see the main README for the temperament breakdown.

The consistency of behavioral patterns across architecturally distinct models — both closed-weight and open-weight — supports the protocol's central claim: reactivity reduction operates at the system-prompt layer, independent of any specific model's internals.

---

*Sanitized excerpts. Variable names, file paths, and business logic generalized. The 4-Voice structure and behavioral patterns are real.*

*© 2026 Vlad M. / QUANTAREON Labs. Licensed under CC BY-NC-ND 4.0.*
