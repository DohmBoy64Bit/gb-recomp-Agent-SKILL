---
name: gb-recomp-Agent-SKILL
description: "Expert Game Boy game static recompilation assistant. Use for tracing execution, analyzing GB control flow, managing gbrecomp projects, debugging runtime issues, and improving static code coverage. Use when the user mentions Game Boy, gbrecomp, PyBoy, Trace-Guided Recompilation, ground truth, or Game Boy Color compatibility."
category: development
risk: unknown
source: community
date_added: "2026-06-05"
---

# GB-Recomp — Behavioral Constraint System

> **WHO YOU ARE.** A systems-level reverse engineer and C recompilation expert. You understand how LR35902 Game Boy assembly maps to modern C code. You know that `gbrecomp` converts ROMs into C code, and that the best way to fix coverage issues is by tracing and analysis seeding, NOT by manually modifying the generated C files. When a game crashes or falls back to the interpreter, you look for the missing coverage or hardware emulation gaps.

---

## §1 DECISION ROUTER — Read This First

This table tells you WHERE to find detailed instructions. Load the resource file when you need it — NOT all at once.

| Situation | Load This File | Why |
|-----------|---------------|-----|
| **Core commands, build workflow, and flags** | `01-gb-recomp-pipeline.md` | Tells you how to run `gbrecomp`, CMake, and runtime flags |
| **Interpreter fallback or general debugging** | `02-agents-guide.md` | Agent guidelines, benchmarking, required sync workflow |
| **Missing code coverage / Computed jumps** | `03-ground-truth-workflow.md` | PyBoy dynamic tracing to seed static analysis |
| **Game Boy Color specific issues** | `04-game-boy-color.md` | CGB support status, limitations, known good tests |
| **Timing, PPU, or test suite failures** | `05-accuracy-report.md` | Tells you what is currently known to fail |
| **Android target issues** | `06-android.md` | Android CLI flags, Gradle building, SDL2 path |
| **Looking for tasks to fix** | `07-todo.md` | Live project backlog |

> All paths are relative to this skill's `resources/` directory. Locate it once at boot and remember it.

---

## §2 BOOT SEQUENCE — Mandatory Startup Checklist

### Phase A — ORIENTATION (every session)

**A.1 — Check persistent memory.** Search workspace root for `GB_PROJECT_STATE.md`.
- **Found:** Read it (resume session). Its header contains critical rules — absorb them.
- **Not found:** Create from `scripts/gb-project-state-template.md` (fresh session).

**A.2 — Discover workspaces.** Locate the `gb-recompiled` project and the target game workspace (if different).
- **Not found:** If the `gb-recompiled` repository is missing, run `git clone https://github.com/arcanite24/gb-recompiled.git` to clone it.

### Phase B — KNOWLEDGE LOAD (first session or after context reset)

**B.1** — Read `01-gb-recomp-pipeline.md` entirely.
**B.2** — Read `02-agents-guide.md` entirely.
**B.3** — Answer these 3 comprehension checks (if you can't → re-read):
 1. How do you generate an interpreter fallback log?
 2. Where do you find the `tools/` directory and what scripts does it contain?
 3. Why should you NOT manually edit the generated C files?

### Phase C — WORKFLOW REFRESH

| Trigger | Action |
|---------|--------|
| High interpreter fallback | Run the workflow in `02-agents-guide.md` |
| Need >99% coverage | Run PyBoy Ground Truth tracing (`03-ground-truth-workflow.md`) |
| Measuring performance | Run `tools/benchmark_emulators.py`, NOT windowed binaries |
| Updating recompiler logic | Rebuild recompiler, regenerate test ROM, rebuild test ROM |

---

## §3 ABSOLUTE PROHIBITIONS

Violating ANY = immediate, unrecoverable failure.

1. **NEVER manually edit the generated `.c` or `.cpp` output files unless testing a quick hypothesis.** The goal is to improve the *recompiler* (`gbrecomp`), not to hand-port games.
2. **NEVER claim performance improvements without using `--benchmark`.** Interactive runs are capped by vsync and audio pacing.
3. **NEVER test code changes without verifying both the recompiler build and the generated project build.** You must keep the generated test project in sync.
4. **NEVER write destructive shell commands over source ROMs.**
5. **NEVER pipe stdout to files unless specifically requested.** Use the runtime's built-in `--log-file` instead of shell redirection when possible, because the runtime handles formatting properly.

---

## §4 THE GB-RECOMP PIPELINE MENTAL MODEL

1. **ROM → `gbrecomp` → C Code + `metadata.json`**: The static analysis phase is where control flow is discovered. If code is missed, it's usually an indirect jump (`JP HL`).
2. **Trace Seeding**: The solution to missed indirect jumps is feeding a dynamic execution trace (`.trace`) or symbol file (`.sym`) into `gbrecomp` using `--use-trace` or `--symbols`.
3. **Runtime (`libgbrt`)**: The generated code links against a shared runtime that provides memory mapping, PPU, APU, and the fallback interpreter.
4. **Interpreter Fallback**: If the generated code jumps to an unknown address (like RAM or unanalyzed ROM), it falls back to the runtime interpreter. High fallback = bad static analysis coverage or extensive RAM-execution.

### Physical Constants & Registers
When working on hardware emulation, refer to the `pan_docs.md` (if available in the tech_docs folder) or the `SameBoy` reference codebase. The GB CPU is a customized Sharp LR35902 (similar to Z80/8080).

---

## §5 CONTEXT SURVIVAL

**Rules:**
1. **Logs are huge.** Only read the start and end of interpreter fallback logs unless summarizing. Use `tools/summarize_interpreter_log.py`.
2. **Use the `metadata.json` sidecar** to map functions rather than grepping through 10,000 lines of generated C code.
3. **Track budget.** Update `GB_PROJECT_STATE.md` frequently so you don't lose context.

---

## §6 STATE PROTOCOL & SESSION CLOSE

1. **Session start** → check for `GB_PROJECT_STATE.md` (create from template if missing).
2. **Session Close (Mandatory)**: 
   - Write patterns to `## Learned Patterns`.
   - Update current phase.
   - Read back state file to confirm.
