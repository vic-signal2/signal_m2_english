# AGENTS.md

This repository follows **[CLAUDE.md](CLAUDE.md)**, for every agent and every
tool. The short version:

- The deterministic core is never rewritten, only evolved.
- This is a **generated** English build; Portuguese is the master.
- Route names, action names and data keys are identifiers, never text.
- `index.html` is a copy of `Signal_M2_English.html`; update both together.
- Prove a bug before fixing it; run the suites before delivering.

Read in this order:

| Document | What it answers |
|---|---|
| [CLAUDE.md](CLAUDE.md) | How to work here, and what breaks if you do not |
| [docs/RULES.md](docs/RULES.md) | What the product does, and why |
| [docs/DETERMINISTIC_CORE.md](docs/DETERMINISTIC_CORE.md) | How answers become text |
| [docs/TRANSLATION.md](docs/TRANSLATION.md) | How this build is generated, and how leftovers were found |
| [docs/AUDIT.md](docs/AUDIT.md) | What is verified, and how to verify it again |
| [docs/SCREENS_AND_FLOWS.md](docs/SCREENS_AND_FLOWS.md) | Screens and how they connect |
| [docs/DATA_MODEL.md](docs/DATA_MODEL.md) | What the backend stores |
| [docs/VISUAL_FIDELITY.md](docs/VISUAL_FIDELITY.md) | The closed design system |
