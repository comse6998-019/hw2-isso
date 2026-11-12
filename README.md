# HW2: ISSO evidence selection

COMS E6998-019 HW2 materials for Aiden Wong's existing HW1 ISSO project.
The brief retains its instructor-review draft status.

Read the [assignment PDF](briefs/hw2-isso.pdf).
The [LaTeX source](briefs/hw2-isso.tex) contains the same assignment.

Extend your HW1 relevance agent into a code-action agent and an MCP agent.
Run both inside OpenShell and compare their behavior under a planted prompt injection.
Label **1 means relevant**. Label **0 means irrelevant**.
Relevant passages can contribute grounded evidence to each question's packet.

## Repository contents

| Path | Purpose |
| --- | --- |
| `briefs/hw2-isso.pdf` | Assignment brief |
| `briefs/hw2-isso.tex` | Brief source |
| `config.schema.json` | Config contract after TOML parsing |
| `report.schema.json` | Classification report and evidence-packet contracts |
| `trace.schema.json` | Per-event trace contract |
| `examples/` | Configs, candidate lists, preparation manifest, neighbors, packets, report, trace, and startup examples |
| `fixtures/` | Injection overlay and delivery instructions |

Reuse the corpus, query sets, and full-run reports from your HW1 submission, as specified in the brief.
The corpus and source reports remain in your HW1 project.

The examples illustrate formats. They contain synthetic results, rather than measured experiments.
You write both agents, the MCP server, and the preparation script.
The MCP server module and arguments are placeholders.
The policy paths and provider endpoint must match your sandbox image and provider.

Read the [example notes](examples/README.md) and [fixture notes](fixtures/README.md) for hashes and validation boundaries.
The [standard HW2 handout](https://github.com/comse6998-019/hw2-tool-boundary/blob/main/briefs/hw2.pdf) supplies the shared OpenShell setup procedure.
This ISSO brief defines the adapted task and contracts.

## Build the brief

With Tectonic installed, run:

```sh
cd briefs
tectonic hw2-isso.tex
```

This brief uses `listings`. It requires neither shell escape nor Pygments.
