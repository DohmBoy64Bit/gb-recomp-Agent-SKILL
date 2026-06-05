# GB Project State

> **CRITICAL AGENT RULES:**
> 1. Update this file after EVERY major phase completion or test run.
> 2. Read this file at the start of EVERY new session.
> 3. NEVER delete information unless you are replacing it with a verified correction.
> 4. If the generated project falls back to the interpreter heavily, record the hot spots here.

---

## 🎯 Current Objective
**Target ROM:** `[Game Name]`
**ROM Hash (MD5):** `[Hash]`
**Goal:** (e.g. "Achieve >99% static coverage", "Fix rendering bug on Level 2", "Implement missing CGB register")

---

## ⚙️ Active Configuration

- **Recompiler Build Dir:** `build/`
- **Generated Output Dir:** `output/[game_name]`
- **Benchmark Build Dir:** `output/[game_name]/build_bench_o3`
- **Trace/Symbol Files In Use:** `[List any .trace or .sym files feeding the recompiler]`

## 🏃 Active Runner Command

> *Agent: Copy-paste the exact shell command used to run the latest generated game or benchmark here so you don't forget flags.*

```bash
# Example: ./output/game/build/game --log-file logs/interpreter.log --limit 500000 --log-frame-fallbacks --report-interpreter-hotspots
```

---

## 📊 Interpreter Fallback Status

*Record the top fallbacks reported by the runtime so you can target them in the recompiler or via tracing.*

| ROM Address (Bank:PC) | Suspected Function/Context | Status (Unanalyzed, RAM code, etc.) |
|-----------------------|----------------------------|-------------------------------------|
| `??:????` | `...` | Needs trace seeding |

---

## 🧠 Learned Patterns & Hardware Quirks

*Agent: When you figure something out about the GB hardware or the recompiler's architecture, write it here so you remember it tomorrow.*

- **Pattern 1:** ...
- **Pattern 2:** ...

---

## 📝 Session Log

- **[Date]**: Initialized state file.
- **[Date]**: ...
