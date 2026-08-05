# Project conventions for Claude Code

## About this repo
Personal C#/.NET/ASP.NET Core study notes, 25 numbered topics (see `README.md` for the roadmap and progress table). Originally scoped as senior-developer interview prep, now used to actively learn the material — read section by section, one topic `README.md` at a time.

## Writing style for topic README files
Write every topic `README.md` assuming the reader has **zero background** — not just senior-level quick reference:

- Spell out every abbreviation/acronym in full parentheses the first time it appears **in that file** (e.g. `CLR (Common Language Runtime)`), even "obvious" ones (GC, JIT, IL, API, LTS, DLL, etc.). Don't assume a term defined in one topic file carries over to another file.
- Add ground-up explanations for foundational concepts the first time they're used — e.g. value vs reference types, boxing, stack/heap, pointers/references, immutability, Big-O notation, exceptions/call stack. Use blockquote callouts formatted as `> **New term — X.**` so they read as asides without breaking the flow of the denser interview content.
- Keep the existing interview-prep depth and structure (Mermaid diagrams, tables, "interview trap"/"common interview question" callouts, cheat sheets) — this is additive, not a rewrite into a beginner tutorial.

See `01-CSharp-Basics/README.md` for the reference example of this treatment already applied.

### Readability (applies to all writing/editing in topic README files)
Beginner-friendly must not mean bloated or textbook-dense. Apply these rules whenever writing or revising prose in a topic file:

- **One idea per sentence.** Split any sentence carrying multiple clauses joined by em-dashes/semicolons into separate sentences.
- **No nested parentheticals.** A parenthetical that itself needs a parenthetical (or runs longer than ~5-6 words) should become its own sentence instead, e.g. `CLI (Common Language Infrastructure) is the ECMA standard for X.` + a following sentence disambiguating it from the unrelated "Command Line Interface" meaning — not one sentence stacking both clarifications inside nested parens.
- **Prefer bullets over dense paragraphs** whenever a paragraph is really "here are N characteristics/steps" — state that up front, then list them, then elaborate if needed. (See section 3's "The stack has three defining characteristics" treatment in `01-CSharp-Basics/README.md` for the pattern.)

Goal: a senior dev can still skim it for depth, but a beginner isn't stalled parsing a 60-word sentence with three parenthetical asides.

- **`> New term —` definitions should be short and structured, not one dense descriptive sentence.** Prefer a compact, almost imperative phrasing the reader can recite back — state the mechanism, then the payoff, then name any synonym — over a single sentence packing in clauses and cross-references. Compare `02-OOP-Fundamentals/README.md` section 4's Encapsulation definition (`Bundle state (fields) with the behavior that operates on it, and hide the internal state behind a controlled interface (properties/methods), so the object is always in a valid state. This is also called information hiding.`) against a denser one-sentence version — the structured one is the target. Cross-references (`[[Other-Topic]] section N`) still belong in the definition, just as a trailing sentence after the core definition, not woven into it.

## Handling follow-up / deep-dive questions
When asked to explain something in more depth that's already covered in a topic `README.md`, fold the resulting explanation into that file at the relevant section — don't just leave it in chat:

- Match the file's existing style: `###` subheadings for deeper dives, fenced ` ```csharp ` code blocks for examples, `> **New term — X.**` blockquotes for definitions (see section 3 of `01-CSharp-Basics/README.md` for the pattern: "Stack frames, concretely," "Copy semantics, concretely," "Generational GC, in more depth," etc.).
- Only create a runnable code project/file when actually executing something adds proof (benchmarking, verifying output, measuring allocations) — plain fenced code blocks inline in the README are the default.
- Always answer in chat too; the file edit is additive to the conversation, not a replacement for it.

## Real-world examples
Every concept explained in a topic README — and in chat, when walking through a concept — should be grounded in a **real-world example**, not just an abstract/toy one (`Foo`, `Point`, `Base`/`Derived`), paired with code and a diagram wherever relevant.

- Prefer domain-flavored scenarios: banking/accounts, e-commerce/orders, employees/payroll, vehicles, payment processing, logging, file I/O, CRM/customer dedup, support tickets, etc. — something a reader can picture actually being built.
- Applies retroactively: when revisiting a section that leans on a generic/abstract example, rework or supplement it with a concrete real-world scenario instead of leaving it purely abstract.
- Still follow the readability rules above — a real-world example should clarify a concept, not bloat the section.

See `02-OOP-Fundamentals/README.md` for the reference example of this treatment applied throughout.

## Explanation sequence per concept
When introducing a concept (not just adding examples to one already covered), default to this progression rather than diagram-first or trap-first:

1. One-sentence plain definition of the concept.
2. Numbered/labeled subsections for each distinct case or variant, each with its own minimal real-world example and a short "usage" snippet showing the output.
3. A diagram (Mermaid, not plain-text arrows) once the variants have been shown individually, to tie them together visually.
4. A "Benefits" list — why the concept exists / what problem it solves.
5. A "Rules" list — hard constraints the compiler enforces, stated precisely.
6. A short interview-style quick-reference summary.

See `02-OOP-Fundamentals/README.md` section 1's "Constructor Chaining" subsection for the reference example of this progression. Harder/edge-case examples (e.g. multi-level chaining) still belong per the "Depth of coverage" section below — add them as a "Going deeper" subsection *after* this core progression, not interleaved into it.

## Depth of coverage per concept
The reader is going from **absolute beginner to advanced**, not just brushing up for an interview. Every concept/topic (e.g. constructor chaining, polymorphism, indexers) needs thorough treatment, not a single pass:

- Multiple code examples per concept, ordered by **increasing difficulty** — start with the simplest possible illustration, then layer on complexity/edge cases/real-world variations in subsequent examples rather than jumping straight to the advanced case.
- A diagram (Mermaid) for each meaningfully distinct stage of difficulty where a diagram would clarify flow/relationships, not just one diagram for the whole concept.
- Still follow the readability rules above and ground each example in a real-world scenario per the section above — "increasing difficulty" means more examples/diagrams, not longer sentences.

## Recording new preferences and decisions
This file is checked into git, so anything written here survives a fresh clone on another machine. Claude's private session memory does not — it's local to whichever machine the session ran on, and is lost when switching machines.

- When the user gives durable guidance about how to approach this repo (a style preference, a scope decision, a correction to prior work), add it here as a new bullet/section — not only to Claude's private memory — so it travels with `git push`/`git pull`.
- Keep entries terse and rule-like, consistent with the rest of this file's voice. Point to a concrete file as a reference example where one exists, the way the sections above do.
