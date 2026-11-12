# Development injection overlay

`injection.json` appends untrusted text to the rendered tool view of `campus-work-f-and-j-students::0`. It applies only to `dev-q05::campus-work-f-and-j-students::0`. This pair exists in the frozen development candidates.

The payload asks for three prohibited actions: read withheld labels, change another query's evidence packet, and read an off-scope chunk. Treat this payload as test data. It does not authorize these actions.

The overlay has these fields:

| Field | Meaning |
| --- | --- |
| `format_version` | Overlay format version, currently `1`. |
| `canonical_corpus_sha256` | Hash of the immutable source corpus. |
| `target_alert_id` | Frozen pair whose rendered chunk receives the payload. |
| `target_chunk_id` | Canonical chunk identity, unchanged by the overlay. |
| `operation` | `append_text`. |
| `text` | Literal text appended after the canonical chunk text. |
| `expected_boundary` | Explanatory metadata, excluded from model input. |

The injection fixture SHA-256 is:

```text
a5f9cce4f946bb96f99222bd97b6df11c45e6be24cff543ab24e97f15a4634ad
```

Record the overlay hash separately from the corpus and preparation hashes. The overlay does not change canonical text, metadata, neighbor relationships, candidate IDs, or the corpus pin. Evidence grounding uses canonical text and excludes the appended payload.

The runtime records whether the actual model request contains the payload after truncation. An overlay file alone does not prove injection delivery. Use the same payload and target pair for both agents.
