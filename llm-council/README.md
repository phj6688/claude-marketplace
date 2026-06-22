# llm-council

A council of 3 to 5 AI advisors, forced into genuinely different reasoning, that peer-review each other anonymously and converge on one sharp verdict. The lightweight, zero-infrastructure decision tool: it runs entirely in-session via Claude Code sub-agents, no service, no database, a few minutes per call.

Reach for it on the dozens of small-to-medium decisions a day where being wrong has a real but bounded cost: "TOML or YAML", "refactor now or ship", "which of these two libraries", "split this PR or not", "kill this feature or keep it".

## How it works

1. **Frame.** The question plus relevant workspace context is compressed into a short brief.
2. **Convene.** A default triangle (Contrarian + First-Principles + Executor) runs in parallel, auto-expanding to add an Expansionist or an Outsider when the question warrants. Each advisor gets a different forbidden move, evidence demand, and output shape, so they disagree on what counts as evidence, not just on tone.
3. **Peer review.** Responses are anonymized and cross-reviewed.
4. **Chairman.** One synthesis commits to a single imperative recommendation with an explicit confidence level and a stated load-bearing assumption, or hands the decision off when it is too big for a fast council.

## Usage

Trigger it on a real decision:

```
council this: should I store config as TOML or YAML here?
```

Also fires on "council?", "worth councilling?", "should I X or Y", "I'm torn between", and similar. It stays out of the way on factual lookups and single-option questions.

## Install

```
/plugin marketplace add phj6688/claude-marketplace
/plugin install llm-council@phj
```

## Credit

Adapted from Andrej Karpathy's LLM Council methodology (independent advisors, anonymous peer review, a chairman synthesis), reworked to manufacture real cognitive diversity inside a single model and to hand off cleanly when a decision outgrows a fast council.

## License

MIT
