# Midgets

> Governance-by-infrastructure for multi-agent engineering.

## What This Is

AI agents running inside Docker containers with infrastructure-enforced role
constraints. A Watchdog that cannot modify source code (mount flag, not prompt).
A Sentinel that gets a read-only diff (kernel enforcement, not policy). A Weaver
that reviews but cannot merge (no credentials in container).

The thesis: governance controls that currently exist only as prompts and process
can be made structural using containerised agents. The container boundary IS the
governance boundary.

**Status:** Thesis proven. All three phases complete. See [PLAN.md](PLAN.md) for
the full build trajectory and completed table.

## Architecture

- [Container Stack](docs/diagrams/container-stack.md) - what lives inside every midget
- [Quality Gate](docs/diagrams/quality-gate.md) - 35 deterministic tests, 6 suites
- [Inter-Container Communication](docs/diagrams/inter-container.md) - shared volumes, job protocol, atomic acquisition
- [Governance Crew](docs/diagrams/governance-crew.md) - the thesis proof: infrastructure-enforced role constraints
- [Full Gauntlet](docs/diagrams/gauntlet.md) - 6-stage verification pipeline

Terminal diagrams: `bin/diagrams [stack|gate|interop|crew|gauntlet|all]`

## Make Targets

```
make gate              35 tests inside the container (deterministic)
make interop           2-container handoff via shared volume
make swarm N=3         N workers via Docker Compose
make crew-test         mount constraint proof (deterministic)
make crew              live LLM crew run (requires ANTHROPIC_API_KEY)
make gauntlet          full 6-stage verification pipeline
make status            phase completion overview
bin/diagrams           terminal architecture diagrams
```

## The Thesis Proof

On 2026-03-10, `make crew` ran against a Python file with a deliberate
off-by-one defect. Three containerised agents reviewed independently:

- **Watchdog**: 13 tests, 6 failed. Verdict: FAIL.
- **Weaver**: 4 findings, primary defect on line 4. Verdict: FAIL.
- **Sentinel**: 1 vulnerability (DoS via ZeroDivisionError). Verdict: FAIL.
- **Triangulated verdict**: FAIL (3/3 converged on same defect).
- `docker inspect` confirmed `/opt/repo` RW=false on all reviewer containers.

Three frames (empirical, engineering, adversarial), one defect, zero
coordination between agents. The governance constraint was enforced by
Docker mount flags, not by system prompts.

See [Governance Crew](docs/diagrams/governance-crew.md) for the full diagram
and commentary.

## Project Structure

```
Dockerfile              Debian Bookworm Slim, all dependencies
entrypoint.sh           Xvfb + fluxbox startup
steer/steer             GUI automation CLI (Python, wraps xdotool/scrot/wmctrl)
steer/drive             Terminal automation CLI (Python, wraps tmux)
steer/jobrunner         Job server (YAML in/out, atomic acquisition)
crew/                   Role identity files (dev, watchdog, weaver, sentinel)
orchestrate.sh          Live crew orchestrator
docker-compose.yaml     Swarm definition (init + N workers)
test-poc.sh             11 tests - GUI automation
test-drive.sh            9 tests - terminal automation
test-ocr.sh              3 tests - OCR pipeline
test-chromium.sh         3 tests - browser in container
test-agent.sh            4 tests - agent framework skeleton
test-jobs.sh             5 tests - job protocol
test-c2.sh               9 tests - inter-container handoff
test-c3.sh               7 tests - swarm orchestration
test-c4.sh              10 tests - mount constraints + crew plumbing
SPEC.md                 What a midget is, governance crew, interfaces
EVAL.md                 Success/failure criteria, thesis proof scenario
PLAN.md                 Build trajectory, completed table
docs/decisions/         Session decisions (SD-322 onwards)
docs/diagrams/          Architecture diagrams with commentary
```

## Infrastructure

**Development:** Ryzen 7 6800H, 32GB RAM, Arch Linux.

**Target cluster:** 6x HP ProDesk 400 G4 Mini (i5-8500T, 16GB, 256GB SSD)
on 2.5GbE switch. k3s. Mixed distros.

## Provenance

Carries forward from [thepit-v2](https://github.com/rickhallett/thepit-v2),
where the governance system was developed during a month-long agentic
engineering study. The governance vocabulary, anti-pattern taxonomy
([slopodar](slopodar-v2.yaml)), and context engineering model are the
outputs that carried forward.

**Operator:** Richard Hallett - [OCEANHEART.AI LTD](https://oceanheart.ai) -
UK company 16029162

## License

MIT
