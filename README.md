# Midgets

> Agent-native Linux containers for autonomous software engineering.

## What This Is

A Docker container is an agent's machine. Not a deployment target — a workspace. The agent boots into its own Linux environment, sees the screen, clicks, types, and operates software through a CLI wrapper. The human enters at the taste boundary.

This project builds the infrastructure for **agent swarms**: multiple containerised agents, each with a defined role, operating in parallel on composable units of software. The governance system — verification gates, adversarial review, structured handoffs — was developed over a month-long engineering study and is carried forward here.

## Concept — Proven

The proof-of-concept is complete. A Docker container (Ubuntu 24.04 + Xvfb + fluxbox + xdotool + scrot) where an AI agent can see the screen, click, type, and control applications through a `steer` CLI wrapper. 10/10 tests passed.

```
midgets/
├── Dockerfile          # Ubuntu 24.04, Xvfb, fluxbox, xdotool, scrot, xclip, wmctrl, tmux
├── entrypoint.sh       # Boots Xvfb virtual display at 1280x720x24, starts fluxbox WM
├── steer/steer         # Python CLI: see, click, type, hotkey, scroll, apps, clipboard, screens
└── test-poc.sh         # 10-test end-to-end suite
```

## The Thesis

Most software is GUI chrome wrapped around simple operations. 75% of software categories are fully or mostly reducible to CLI/API operations. On Linux, you don't automate bloated consumer software — you create bespoke, minimal, **agent-native software**. The machine is the agent's sandbox.

Three concepts drive this work:

1. **Agent-native software** — built for agents from the start. Text interfaces, composable primitives, structured feedback, no GUI discovery needed.
2. **Operational training** — through trial and error, the human-agent system develops custom conventions that compound over time. Distinct from ML training. The conventions are the primary output.
3. **Linux as agent OS** — Linux never abandoned the CLI. Every GUI is optional. The system is fully operable from a terminal. This architectural decision, made decades ago for different reasons, is exactly what agent-native computing requires.

## Roadmap

- [x] Container POC (Xvfb + steer wrapper, 10/10)
- [ ] OCR (tesseract) — the equaliser for Electron apps
- [ ] Chromium — real browser testing
- [ ] Drive port (tmux sentinel protocol from mac-mini-agent)
- [ ] Listen port (job server — enables remote dispatch)
- [ ] Agent framework in container (Pi or Claude Code)
- [ ] Multi-container orchestration
- [ ] Governance crew as physical agents (Weaver, Watchdog, Sentinel)

## Infrastructure

**Development machine:** Ryzen 7 6800H, 32GB RAM, Arch Linux. Runs 3-5 midget containers now.

**Target cluster:** 6× HP ProDesk 400 G4 Mini (i5-8500T, 16GB, 256GB SSD) on a 2.5GbE switch. k3s. Mixed distros (Debian control plane, Arch + Ubuntu workers). Cost: £80-150 per node from eBay. Each node hosts 5-10 containers.

## Provenance

This project carries forward from [thepit-v2](https://github.com/rickhallett/thepit-v2), where the governance system, lexicon, and research artifacts were developed during a month-long agentic engineering study. The product code (an AI debate arena SaaS) is archived on the `phase2` branch. The governance system that built it — verification gates, anti-pattern taxonomy (slopodar), context engineering vocabulary, multi-model adversarial review — turned out to be more interesting than the product.

**Operator:** Richard Hallett · [OCEANHEART.AI LTD](https://oceanheart.ai) · UK company 16029162

## License

MIT
