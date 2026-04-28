# Session Log Example: 4-Voice Verification Pipeline

> Sanitized excerpt from a production coding session on QUANTAREON platform.
> Demonstrates how the ConsciousAI Protocol prevents scope creep and code-breaking.

---

## Session 1: Screenshot Module Fix

**Context:** Production screenshot capture module using SnapDOM library.
Server-side image processing with size constraints.

**User Request:**
> "Change `scale: 2` to `scale: 1` in the snapdom config. Don't touch DOM classes."

### 4-Voice Pipeline

```
[Proposal]     Change scale: 2 → scale: 1 in snapdom configuration object.
               Single parameter change in quantarion-vision.js.

[Risk Check]   Output image dimensions will halve.
               JPEG filesize depends on scale — verify output stays
               under MAX_IMAGE_SIZE (500KB).
               SnapDOM quality setting (JPEG_QUALITY: 0.92) is independent
               of scale — no side effect there.

[Scope Guard]  Task is ONLY scale change.
               Do NOT touch:
               - DOM classes (explicitly prohibited by user)
               - CAPTURE_DELAY (separate parameter, not requested)
               - JPEG_QUALITY (working correctly, no reason to modify)

[Min Action]   One str_replace operation.
               Old: scale: 2
               New: scale: 1
               File: webapp/quantarion-vision.js
               Lines affected: 1
```

### Result
- ✅ Single str_replace executed
- ✅ No collateral modifications
- ✅ Working on first attempt
- ✅ User confirmed correct behavior

### What would happen WITHOUT the protocol
A standard LLM coding assistant typically:
1. Changes `scale: 2` → `scale: 1` ✓
2. Also "improves" `CAPTURE_DELAY: 3000` → `5000` ✗ (not requested)
3. Refactors the surrounding function "for clarity" ✗ (not requested)
4. Adjusts `JPEG_QUALITY` to "compensate for lower scale" ✗ (not requested)
5. Adds comments explaining the change ✗ (not requested)

Items 2-5 are **scope violations** — modifications the user did not ask for,
each carrying risk of breaking working code.

---

## Session 2: Database Field Migration

**Context:** PostgreSQL database with asyncpg.
Users table with evolving schema, production data.

**User Request:**
> "Add column `daily_chat_pairs_count INT DEFAULT 0` to users table."

### 4-Voice Pipeline

```
[Proposal]     ALTER TABLE users ADD COLUMN IF NOT EXISTS
               daily_chat_pairs_count INT DEFAULT 0;
               Add to init_db() migration block.

[Risk Check]   IF NOT EXISTS prevents crash on re-deploy.
               DEFAULT 0 means existing rows get valid value.
               INT is appropriate for daily counter (max ~2B).
               No index needed for this field (not queried in WHERE).

[Scope Guard]  Task is ONLY adding one column.
               Do NOT:
               - Modify existing columns
               - Add related columns preemptively (e.g. chat_pairs_limit)
               - Restructure the migration block
               - Add reset logic (not requested yet)

[Min Action]   One line added to init_db():
               await conn.execute("ALTER TABLE users ADD COLUMN
               IF NOT EXISTS daily_chat_pairs_count INT DEFAULT 0")
               No other files touched.
```

### Result
- ✅ Single ALTER TABLE executed
- ✅ No schema side effects
- ✅ Backward compatible
- ✅ Production-safe (IF NOT EXISTS)

---

## Session 3: Context Builder Tag Stripping

**Context:** AI context assembly pipeline.
Raw text from multiple sources needs sanitization before LLM input.

**User Request:**
> "Add tag stripping to context_builder.py — strip custom tags,
> normalize whitespace, trim to 150 chars."

### 4-Voice Pipeline

```
[Proposal]     Add regex _TAG_RE to strip [...] tags from text.
               Apply: strip tags → normalize whitespace → trim to 150 chars.
               Add as private method _clean_context_text().

[Risk Check]   Regex must not strip valid content inside brackets.
               Pattern \[.*?\] with re.DOTALL could match across lines.
               Better: \[[A-Z_]+:.*?\] to match only uppercase tag patterns.
               Order matters: strip tags FIRST, then normalize whitespace,
               then trim. Wrong order = broken output.

[Scope Guard]  Task is tag stripping + whitespace + trim.
               Do NOT:
               - Modify the tag generation logic (upstream, separate concern)
               - Change how context is assembled (different function)
               - Add logging (not requested)
               - Modify the 150 char limit (user specified it explicitly)

[Min Action]   Add _TAG_RE compiled regex (1 line)
               Add _clean_context_text() method (4 lines)
               Call it in the existing pipeline (1 line)
               Total: 6 lines added, 0 lines modified.
```

### Result
- ✅ Minimal, focused change
- ✅ Correct operation order (strip → normalize → trim)
- ✅ Safe regex pattern
- ✅ No upstream/downstream modifications

---

## Pattern Summary

Across 500+ sessions, the 4-Voice pipeline consistently catches
these failure classes:

| Voice | Catches | Frequency |
|-------|---------|-----------|
| **Proposal** | Wrong action plan, misunderstood intent | ~10% of requests |
| **Risk Check** | Side effects, dependency breaks | ~25% of requests |
| **Scope Guard** | Unauthorized modifications, "improvements" | ~40% of requests |
| **Minimal Action** | Over-engineering, unnecessary refactoring | ~30% of requests |

**Scope Guard is the most active voice** — reflecting the dominant failure
mode of LLM coding assistants: doing more than asked.

---

*These logs are sanitized excerpts. Variable names, file paths, and business
logic have been generalized. The 4-Voice structure and behavioral patterns
are authentic.*

*© 2026 Vlad M. / QUANTAREON Labs. Licensed under CC BY-NC-ND 4.0.*
