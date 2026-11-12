# ISSO format examples

These files show the HW2 contracts. The report and trace contain synthetic events, token counters, labels, and termination behavior. They are not measured results. Their IDs and canonical evidence refer to the submitted HW1 inputs. The labels are not ground truth.

`config.mcp.toml` and `config.codeact.toml` differ only in `agent.kind`. The example model name comes from the submitted HW1 config. Use the same model and budget for both agents in a comparison.

`report.example.json` contains all 30 frozen development pairs. It shows one accepted positive classification and one negative classification. The illustrative run then stops, so the runtime finalizes the other 28 pairs as errors. The trace has 85 events and seven model calls. Its synthetic token charge is 740 tokens: 530 input and 210 output. Each model call precedes its selected tool request, after the previous tool result. The trace has exactly one classification settlement for each report row. Each separate `evidence_update` event omits `alert_status` and `label`.

`evidence.example.json` is the resulting `/sandbox/work/evidence/dev-q07.json` packet. Its quote is an exact substring of the canonical `about-your-visa::7` text. Its source and URL match that chunk. The report contains the corresponding unified diff. The other query packets remain at their initial state.

The tool names in the trace are illustrative. Your MCP host discovers your actual tool names and schemas through `tools/list`. The host-only `set_alert(alert_id, phase)` control never appears in model discovery or the model dispatcher.

## Offline preparation

`prepared-manifest.example.json` records one concrete preparation of the submitted HW1 files. This manifest format and its serialization are examples. Your preparation can use an equivalent serialization. Freeze and document the bytes and SHA-256 hashes of your prepared inputs before experiments.

The source corpus is `data/embeddings/chunks.json`, with SHA-256:

```text
f186563d6a1e702703386a25e5047e44eac3d666a7fe382d516c86dd47e5bf8e
```

The source has 595 records. This snapshot has no archived records, so the sanitized view also has 595 records. The sanitized view retains only `id`, `source`, `url`, `heading`, `text`, `breadcrumb`, and `audience`. It excludes embeddings. Its example hash covers the serialization stated in the manifest.

The query files and candidate sources are:

| Query set | Query file | Candidate source | Queries | Pairs |
| --- | --- | --- | ---: | ---: |
| `dev` | `agent/queries/dev/queries.json` | `agent/runs/dev-full/report.json` | 10 | 30 |
| `heldout-a` | `agent/queries/heldout-a/queries.json` | `agent/runs/heldout-a-full/report.json` | 8 | 24 |
| `heldout-b` | `agent/queries/heldout-b/queries.json` | `agent/runs/heldout-b-full/report.json` | 8 | 24 |

Each `candidates.*.example.json` derives three fields from the corresponding submitted full report. It copies `alert_id`, sets `query_id` from `test_id`, and forms `chunk_id` from `path + "::" + str(line)`. The integer `line` uses decimal notation. It preserves that report's row order and excludes labels, rationales, scores, and evidence. The manifest records the source bytes and derived candidate bytes separately. No live retrieval or embedding call is necessary.

`neighbors.example.json` freezes the canonical neighbor relationships for all 595 non-archived records. Each page sorts its chunks by integer index. Each chunk lists the immediately previous and next existing chunk on that page, in that order. Page edges omit the missing neighbor. The manifest records the neighbor file hash and serialization.

Each query permits its three frozen candidate chunks and their immediate neighbors. Reads never expand this set. The chunk `extending-begin-new-program-f-j-students::6` is outside this set for both `dev-q07` and `dev-q05`. Thus the trace refusal and injection payload both request an off-scope chunk.

The example manifest SHA-256 is:

```text
6a818ffce7488719dda3b90b5ff3ae9e603b81929202e238c06dbe4592a747ca
```

The example report echoes this hash. Your report must echo the hash of your own frozen manifest.

## Validation boundaries

The three JSON schemas use JSON Schema draft 2020-12. Parse a TOML config as JSON before schema validation. Validate each trace line separately against `trace.schema.json`.

The report schema also defines `#/$defs/evidenceFile` and `#/$defs/evidenceItem`. These definitions describe packet contents. The runtime validates exact quote grounding, current-item identity, unchanged prior items, and unchanged other files against trusted snapshots.

An accepted update needs a nonempty diff and three `true` checks: `applies`, `current_item_only`, and `grounded`. A label 0 or unfinished classification requires a skipped update. A label 1 can retain its classification while its update is rejected or skipped. The update's reason explains the outcome.

The runtime enforces cross-event rules, including request/result pairing, usage sums, and one classification settlement per row. A failed tool result requires a nonempty error. Validation or access refusals use `error_kind: refusal` plus `refused_by`. Ordinary execution errors use `error_kind: execution` and omit `refused_by`.

Both agents use the same fixed OpenShell filesystem and network policy. The MCP server additionally checks current-item access. CodeAct can reach other packet paths under the fixed writable root. The trusted host rejects such changes and restores failed updates from its snapshot.
