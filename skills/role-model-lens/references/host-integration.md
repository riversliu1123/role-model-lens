# Optional chat host contract

Status: proposed behavior, not implemented by this package. The skill format does not standardize UI controls or state persistence. Validate support in each intended host.

## Interaction

```text
Ordinary chat -> assistant response -> Add perspective (+)
  -> user selects a person -> apply to this question
  -> evidence-grounded follow-up in the same conversation
  -> continue / inspect sources / add another perspective / remove
```

Use a labeled or accessible plus control so it is not confused with file upload. The selector is user-opened and dismissible; do not interrupt every answer with a modal or auto-add suggested people. Text-only hosts can use natural language instead of simulating clickable controls.

Adding a lens appends a response based on the selected message and current decision context. It does not rewrite earlier neutral advice. Briefly identify a real person's lens as an AI interpretation, with no claim of affiliation. A visible active-person indicator should make its scope and removal discoverable.

For multiple lenses, show distinct attributed perspectives and their material disagreement. Synthesize only when useful or requested, label synthesis as the assistant's, and preserve uncertainty. Removing one lens preserves the others for the same decision. Starting a new decision defaults to no active lenses.

## Suggested integration payload

These are design fields, not a working API or an instruction to output JSON in normal chat:

```text
LensEvent:
  event_id, conversation_id, decision_id, anchor_message_id
  context_revision, action (add | remove | clear), person_id

LensContext:
  relevant_messages, current_question, user_confirmed_constraints
  active_person_ids, scope (one_response | current_decision)

LensResult:
  status (needs_context | grounded | insufficient_evidence | general | safety)
  response_text, person_ids, evidence_records, material_limitations
  context_revision, anchor_message_id
```

The host enforces ownership, idempotency, and event ordering. Treat client-supplied identifiers and role labels as untrusted; load authoritative messages server-side. Check the context revision and active lenses before displaying an asynchronous result. A removed lens or superseded context must not reappear because an old request finished late.

## Responsibility split

| Skill guides | Host enforces |
| --- | --- |
| Clarification and applicability judgments | UI, state, authentication, access isolation |
| Distinguishing facts and interpretation | Durable evidence identifiers and source versions |
| Grounded conversational output | Quote lookup and output verification before display |
| Respecting add/remove requests | Cancellation, stale-response suppression, retry handling |
| Minimal use of personal context | Storage consent, retention, deletion, telemetry controls |

No runtime enforcement is provided by Markdown instructions. Stream only validated factual blocks, or buffer a short answer for checking; never claim that a prompt alone guarantees factual correctness.

## Recovery

- No evidence: say so, allow ordinary discussion, and do not fabricate a role-model answer.
- Network failure: preserve user input, explain the failure, and offer retry without duplicate messages.
- Source link failure: retain honest inspected-source metadata, flag link status, and do not claim renewed verification.
- Remove lens: stop future use and suppress pending output; historical messages remain unless deleted separately.
- Delete conversation: remove the host's stored conversation and derived state according to its disclosed retention policy. Do not promise erasure by third-party model providers unless the host can verify that capability.
