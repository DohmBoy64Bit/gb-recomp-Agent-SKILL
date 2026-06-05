# GB Recomp Mastery Skill

Welcome to the **GB Recomp Mastery Skill**: an agentic framework designed to guide LLMs (like Antigravity or Cursor) in autonomously recompiling Game Boy games, resolving interpreter fallbacks, and improving the `gb-recompiled` static analysis engine.

## ⚠️ How to Treat the Agent

**Do not treat it as a chatbot. Treat it as a Junior Reverse Engineer working on your machine.**

1. **It is Autonomous:** The agent runs cmake builds, traces ROMs using PyBoy, and executes the recompiled games to test its coverage.
2. **It has Persistent Memory:** The agent will create a `GB_PROJECT_STATE.md` file in your root directory. This is its "external hippocampus".
3. **It Follows Procedures:** It knows *not* to manually patch generated C code, but rather to improve the static recompiler or feed it dynamic traces.

---

## 🛠️ Prerequisites (Your PC Setup)

For the agent to work flawlessly, your machine must have the following ready:

1. **CMake & Ninja**: Required to build the recompiler and the generated projects.
2. **C/C++ Compiler**: Clang, GCC, or MSVC.
3. **SDL2 Development Libraries**: Required for the runtime's audio and video output.
4. **Python 3.x**: Needed for the trace capture and benchmarking scripts.
5. **PyBoy (Python Module)**: Required if you want the agent to automatically capture Ground Truth traces for >99% coverage. (`pip install pyboy`)

---

## 🚀 Start a Session

Copy-paste this **entire block** into your AI IDE to activate the skill.

```text
Read the skill file `gb-recomp-Agent-SKILL/SKILL.md` and the build/agent references (`resources/01-gb-recomp-pipeline.md` and `resources/02-agents-guide.md`).
Do NOT proceed until you have read all three files.

Then execute this startup sequence:

1. INSPECT my workspace. Look for:
   - Any `GB_PROJECT_STATE.md` (persistent memory from a previous session)
   - The `gb-recompiled` repository (if missing, clone it: `git clone https://github.com/arcanite24/gb-recompiled.git`)
   - Any ROM files or output directories
   
2. If `GB_PROJECT_STATE.md` exists, read it to recover our progress. If it doesn't, copy `gb-recomp-Agent-SKILL/scripts/gb-project-state-template.md` to `GB_PROJECT_STATE.md` in the project root.

3. ASK me for anything you still don't know. Typical questions:
   - Which ROM are we porting or testing?
   - Are we trying to fix a bug in the recompiler, or just get 100% coverage on a specific game?

4. REPORT: Tell me what you found, what phase I'm in, and what the next concrete step is. Then wait for my go-ahead.
```

## Quick Resume

If you are just continuing a previous session in a new chat:

```text
Read the skill `gb-recomp-Agent-SKILL/SKILL.md` and the build references.
Then read `GB_PROJECT_STATE.md` to recover our progress. Resume from there.
```

### Fresh Game (you know exactly what you want)

If you're starting a new game and want to skip the Q&A:

```text
Read the skill `gb-recomp-Agent-SKILL/SKILL.md` and the build references.
I want to recompile [GAME NAME].
The ROM is at: [ABSOLUTE PATH TO ROM]
Start at Phase 0 — check the workspace and let's get a baseline coverage test running.
```

---

## 🤝 Collaboration

- **Monitor `GB_PROJECT_STATE.md`**: You will see the agent filling out tables of coverage and interpreter fallbacks. Correct it if it hallucinates.
- **Provide ROMs**: Ensure you have legally obtained ROMs placed in the `roms/` directory.
- **Let it trace**: The agent may run the Ground Truth pyboy capturer for a few minutes. Let it run so it can seed the static analysis!

---

## 🚨 Troubleshooting

* **The Agent wants to manually edit the generated C files:** Tell it: "No, read your Skill. You must fix coverage via tracing or improving the recompiler analysis."
* **The Agent claims performance is better without running a benchmark:** Tell it: "Run the benchmark script `tools/benchmark_emulators.py` to prove it."
* **The Agent is confused about paths:** Remind it to check `GB_PROJECT_STATE.md` or look at its `resources/` folder.

---

## 🙏 Acknowledgements & Credits

The structural framework and operational model for this AI Skill are directly derived from the amazing [**PS2Recomp Agent Skill**](https://github.com/hkmodd/ps2-recomp-Agent-SKILL) created by **hkmodd**. Their work laid the foundation for building highly constrained, persistent memory agent loops tailored for reverse engineering. Huge thanks to hkmodd!

This skill directly integrates with the phenomenal [**gb-recompiled**](https://github.com/arcanite24/gb-recompiled) static Game Boy recompiler project created by **arcanite24**. The recompiler's architecture and tooling are what make this automated workflow possible. All credit for the actual recompilation engine goes to arcanite24 and their contributors!
