# nate-specify

A minimal three-stage specification workflow for competent coding agents.

This repository exists so you can vendor a small, self-contained
`.nate-specify/` directory into other projects. It is intentionally much
simpler than GitHub's [spec-kit](https://github.com/github/spec-kit): no
runtime components, no integration hooks, just a tiny amount of
structure and a single helper script.

## Adding `.nate-specify` to a project

The intended way to adopt nate-specify is to **copy the directory into
your repo root**:

```bash
# from somewhere you have this repo cloned
cp -R path/to/nate-specify/.nate-specify /path/to/your/project/
```

Then, in your project:

1. Commit `.nate-specify/` to the repository.
2. From the project root, create an epic with:

   ```bash
   bash .nate-specify/create-epic <lowercase-hyphenated-name>
   ```

   This creates `specs/NNN-<name>/` and copies in the templates.
3. Always give agents the **epic directory explicitly** (for example,
   `specs/001-my-epic/`). Do not rely on branches or hidden state to
   infer an "active" epic.

After copying `.nate-specify/` you never have to depend on this
repository again.

## How it works

### Files and directories

The `.nate-specify/` tree is designed to be read directly by agents:

- `.nate-specify/README.md` – short, local explainer for agents.
- `.nate-specify/create-epic` – a tiny Bash script that:
  - finds the next numeric prefix under `specs/`;
  - creates `specs/NNN-name/`;
  - copies the templates into that directory; and
  - prints the created path.

  It **does not** inspect or modify Git state (branches, remotes,
  staging, etc.).

- `.nate-specify/instructions/` – stage-specific instructions:
  - `design.md` – how to define required behavior in `<epic>/spec.md`.
  - `research.md` – how to resolve uncertainties in `<epic>/research.md`.
  - `plan.md` – how to turn the spec and research into
    `<epic>/plan.md`, `<epic>/tasks.md`, and `<epic>/quickstart.md`.

- `.nate-specify/templates/` – sparse Markdown templates copied into
  each epic directory:
  - `spec.md` – behavioral design/contract for the epic.
  - `research.md` – questions, findings, and evidence.
  - `plan.md` – implementation plan, architecture, and tradeoffs.
  - `tasks.md` – ordered, dependency-aware task list with exact file
    paths.
  - `quickstart.md` – macro-level acceptance / end-to-end validation.
  - `data-model.md` – optional data model details (delete when not
    needed).
  - `contracts/` – empty directory for protocol/API contracts when they
    are useful.

Running, for example:

```bash
bash .nate-specify/create-epic add-billing-endpoint
```

produces:

```text
specs/
  001-add-billing-endpoint/
    spec.md
    research.md
    plan.md
    tasks.md
    quickstart.md
    data-model.md   # remove if the epic has no data model impact
    contracts/
```

You then point agents at `specs/001-add-billing-endpoint/` when asking
for design, research, planning, or implementation work.

### The three stages

nate-specify separates thinking work into three minimal stages.

1. **Design** (`instructions/design.md`, `<epic>/spec.md`)
   - Define the problem, motivation, required behavior, scenarios,
     constraints, success criteria, scope, and non-goals.
   - Treat `spec.md` as a **behavioral contract**, not an implementation
     plan.
   - Do not generate tasks or user stories yet.

2. **Research** (`instructions/research.md`, `<epic>/research.md`)
   - Read the spec and the relevant code.
   - Identify what is unknown and classify each question
     (repo/docs, experiment, user evidence, product decision).
   - Run only the minimal experiments or diagnostics needed.
   - Record findings, evidence, decisions, and remaining unknowns in
     `research.md` and optional supporting artifacts (such as
     `data-model.md`).

3. **Planning and Tasks** (`instructions/plan.md`, `<epic>/plan.md`,
   `<epic>/tasks.md`, `<epic>/quickstart.md`)
   - Derive the smallest set of independently valuable user stories.
   - Choose one implementation approach and describe it in `plan.md`.
   - Write `tasks.md` as an ordered checklist with concrete actions and
     exact file paths.
   - Write `quickstart.md` as an end-to-end acceptance/validation
     procedure focusing on macro-level behavior, not micro tests.

Once these artifacts exist, a competent coding agent can implement the
work by following `tasks.md`, using `plan.md` and `spec.md` as the
source of truth.

## What it does (and does not) do

**It provides:**

- A **consistent epic layout** under `specs/NNN-name/` across
  repositories.
- Clear, minimal instructions that assume the agent can read code and
  make decisions, rather than filling huge forms.
- A strict separation between design, research, and planning so that
  implementation work is based on explicit decisions and evidence.
- A focus on macro-level validation ("does the system behave correctly
  end-to-end?") rather than inventories of unit tests.
- A workflow that is independent of Git platforms, CI systems, languages
  or frameworks.

**It deliberately does _not_ provide:**

- Any CLI beyond the `create-epic` Bash script.
- Any GitHub or Git integration, bots, or status checks.
- Any attempt to infer an "active" epic from branches or PRs.
- Any runtime services, databases, or external dependencies.

This is a **pure scaffolding** and documentation layer.

## Why use this instead of a heavier system?

Compared to something like GitHub's spec-kit, nate-specify is:

- **Copy-pasteable** – just vendor `.nate-specify/` into any repo.
- **Tiny** – a handful of short Markdown files and one small script.
- **Language-agnostic** – works for Python, Go, Rust, front-end, or
  mixed stacks.
- **Agent-friendly** – instructions are written for mature agents that
  do not need hand-holding but benefit from a clear, shared structure.
- **Safe by default** – the tooling never touches Git state and
  introduces no new runtime moving parts.

If you want the smallest possible, repeatable way to get serious design,
research, and planning artifacts for agent-driven work, nate-specify is
meant to be that middle ground.