# Repository Hygiene — Mandatory Rule Before Any Decision/Modification/Fix/Build/Deletion

## The Rule (Mandatory)

Before **any** operation — deletion, modification, fixing, building (Dockerfile/image), adding code/file, ignoring in `.gitignore`, moving, or **any architectural decision** — you must answer the following three questions **in writing**, and the operation must not be executed prior to this:

1. **Did I actually inspect the content/context?** (Not relying on name/extension/assumptions — but the actual code/reality).
2. **Do I understand why the component exists and its architectural rationale?** (Origin, references, author's intent, dependencies).
3. **Is the decision strictly scoped and evidence-based, or a superficial generalization?** (Prohibited example: "`data/` = runtime" or "this is excluded" without proof).

This rule applies to decisions **every single time**, not just for deletions.

---

## Architectural Depth (Mandatory Always, Do Not Re-prompt)

> **Recorded by Owner Order:** "Strictly abide by these rules always; I do not want to re-prompt and ask every time."

Every design/code/contract must reflect the **core of the cognitive system**, not a superficial traditional pattern:

1. **Governing Reference = `DESIGN_COGNITIVE_ENGINE.md`** (De-facto approved) + the Cognitive Sequence (ingest → distill → KG → rules → reason → verify → **prove every claim** → learn). Any design violating this sequence = rejected.
2. **No Superficial Patterns:** Purely weighted-sum, provider-picker-only, logic-less wrappers, stubs, or traditional "pick-model-then-generate" patterns are prohibited. The requirement is a **cognitive decision engine**, not a switcher.
3. **"Prove, Do Not Claim" Principle:** The output must carry a **proof requirement** (cite / abstain), not merely eloquent text.
4. **Local-First Sovereign:** Local is the default; external providers are an **explicitly documented escalation** (no silent fallbacks).
5. **When in doubt between "Advanced" and "Bloat":** Favor the deeper architectural choice consistent with core docs, without scope-creep outside the feature.

**Auto-Applied:** The owner does not need to re-request "depth / no patching / no traditional" — it is an implicit condition in every task. If a superficial pattern appears, it must be redesigned before proceeding.

---

## Prohibitions

- **No generalizing** over an entire directory without inspecting its sub-contents.
- **No deletion** outside the "Deletion Lifecycle" below. A git safety net is a permanent requirement in all cases.
- **No enforcing tracking or exclusion** of an ambiguous file (seed or runtime?) before opening its content. Move it to "Pending Review" instead.
- **When in doubt** → Ask the owner, do not make assumptions.
- **Strictly prohibited to use superficial text search tools** (e.g., `grep_search`, shell `grep`, `rg`, `Select-String`, `findstr`) — see the dedicated rule below.

---

## No Superficial Text Search (Mandatory Always)

> **Root Cause (Consistent with "No Superficial Inspection" philosophy):** Superficial regex matches tempt the AI to judge a component based on an isolated line without understanding its structure/algorithm. Deep structural reading (AST + full file read) is required.

**Absolutely Prohibited:**
- Superficial text search tools in shell or AI extensions.

**Mandatory Alternatives (Deeper inspection):**
1. `read_file` / `read_files` — Read the full file or specific semantic ranges.
2. `read_code` — AST analysis for signatures and symbols (supports `Class.method`).
3. `file_search` — Path finding (fuzzy matching), not content extraction.
4. `list_directory` — Explore structure visually.
5. **Local Python AST Analysis** — Run a script to read and structurally analyze files to prove zero-references or find dependencies.

**Auto-Applied:** Included automatically in every session.

---

## Deletion Lifecycle (Replaces absolute "No Deletion")

> **Reason:** Absolute "No Deletion" is useful during fixing to prevent system collapse, but as a permanent policy, it immortalizes dead code. The alternative is a disciplined lifecycle.

**No deletion during fixing unless all 5 conditions are met (in order):**

1. **Prove Zero References:** AST analysis proves the absence of any `import`/`COPY`/`redirect`/dynamic-dispatch for the component.
2. **Deprecated Tagging:** Add `# CANDIDATE FOR OWNER-APPROVED REMOVAL` + reason in the component's docstring.
3. **Observation Period:** Remains tagged for ≥ 1 PR or a week, to ensure no hidden caller emerges.
4. **Explicit Owner Approval:** Written decision from the owner on this specific component.
5. **Regression Test:** Passes after removal (no breaking of a live path).

**During Fixing (before meeting all 5):** Retain the component and tag it — no deletion. 

---

## Test Fixture Construction (Mandatory Always)

> **Root Cause Addressed:** Writing test fixtures from memory, instead of reading the production constructor and utilizing existing test helpers.

### Mandatory Rule (Applies before instantiating a production object in a test)

| Step | Obligation | Verification Method |
|---|---|---|
| **1. Read production `__init__`** | Mandatory before instantiation | Read actual file; do not rely on memory. |
| **2. Search for existing helper** | Mandatory before writing a new one | Search for `_build_*`, `_make_*`, `_setup_*` in `testing/`. |
| **3. Reuse helper** | Mandatory if it exists | If not, read the nearest test using the same class. |
| **4. No `MagicMock()` for settings** | Mandatory | Use real Config/Settings objects unless explicitly tagged as `STRUCTURAL TEST ONLY`. |
| **5. No `Spy*` subclass for tests** | Mandatory | Proof comes from **visible side-effects** (logger output, audit chain, file generation) via real handlers. |
| **6. No claiming arbitrary states** | Mandatory | Claims must be backed by operational evidence seen by the owner. |

### Self-Checklist Before Committing Any Added Test
- [ ] Read the `__init__` in the production file.
- [ ] Searched for builder helpers — found/not found.
- [ ] If found, reused or parametrized it (no duplication).
- [ ] Settings used are real (or mocked with strict explicit tagging).
- [ ] No Spy-subclass created for the Class-Under-Test.
- [ ] Proof of execution relies on visible side effects, not a spy counter.

---

## Pre-Build Component Search (Mandatory Always)

Before generating any new file in core cognitive or system modules, a mandatory section must be written in the **commit message** or final output:

```text
## Pre-Build Component Search (Mandatory)

Files read with evidence (Actually read, not just promised):
1. <path>:<line-range> — <role confirmed>

Existing components found that share role / overlap:
- <path>::<class> — <role overlap %> — <decision: reuse / share-logic / parallel-with-debt>

Decision rationale:
- Why a NEW file rather than extending <existing>: <one-line reason>
- Logic shared / duplicated: <explicit list>
- Technical debt incurred: <follow-up task or "none">
