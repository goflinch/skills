# Questions

> The three question types, every field, and how to ask many at once.

A call to `/v1/decide` carries one `text` and a list of `questions`. Each question gets one answer.

## The three types

### `yes_no`

A question with two answers. `probs` is `[no, yes]`.

```json
{"id": "refund", "type": "yes_no", "ask": "Is the customer asking for a refund?"}
```

### `choice`

Pick one of your options. `probs` has one number per option, in your order.

```json
{"id": "team", "type": "choice", "ask": "Which team should handle this?",
 "options": ["billing", "shipping", "technical", "none of these"]}
```

Set `"multi": true` when several options can be right at once. The answer then adds `selected`, every option that applies.

### `score`

Rate the text on an ordered scale. Without `options` the scale is `1` to `5`. Give your own levels, lowest first, to make the scale say what you mean:

```json
{"id": "urgency", "type": "score", "ask": "How urgent is this?",
 "options": ["can wait a week", "today", "within the hour", "right now"]}
```

## Fields

| Field | Rules |
| --- | --- |
| `id` | lowercase letters, digits and `_`, starts with a letter, up to 40 characters; the key of the answer |
| `type` | `yes_no`, `choice` or `score` |
| `ask` | the question, 3 to 300 characters |
| `options` | for `choice` (at least 2) and `score`; up to 30 |
| `multi` | `choice` only: several options can be right |

Up to **20 questions** per call.

## Ask many at once

Flinch reads the text once and answers every question together. Asking ten small questions in one call is barely slower than asking one, and much more accurate than one big question that tries to cover everything.

## Writing questions that answer well

- **One thing per question.** "Is it urgent and about billing?" is two questions.
- **Say it plainly.** Flinch reads literally. Ask what you mean, not around it.
- **Give an exit.** Add `none of these` or `other` when the text might not fit your options.
- **Words, not math.** Let your code do arithmetic and date comparisons; ask Flinch for the parts.
- **Only the text that matters.** Cut boilerplate, signatures and unrelated history before you send it.

---

# Confidence

> How sure Flinch is, and how to turn that into behaviour.

Every answer carries two kinds of number:

- **`probs`**: how likely each option is, in your order. They add up to 1.
- **`confidence`**: how sure Flinch is of the answer it gave, from 0 to 1.

## Act, ask, or escalate

Confidence is a second axis. The answer tells you **what**; confidence tells you **whether to act on it**.

```python
a = answers["team"]
if a["confidence"] >= 0.85:
    assign(a["answer"])            # act
elif a["confidence"] >= 0.6:
    suggest(a["answer"])           # a person confirms
else:
    send_to_human()                # Flinch isn't sure: don't guess
```

## Picking your thresholds

There is no universal number. Take a few hundred real examples with known answers, run them through Flinch, and pick the threshold where the answers you'd act on are right often enough for **your** cost of a mistake. A refund router can act at 0.8; a fraud block might need 0.97 and a person for everything below.

## Use the whole distribution

- Two options close together (`0.48` and `0.46`) means the text genuinely fits both. Ask a sharper question, or show both.
- For a score, the spread matters: most weight on one level is a clear answer; weight across three levels is a judgment call.
- Sort by `probs`, don't just take the top answer, when you're ranking things.
