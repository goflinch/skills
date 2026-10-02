# Patterns

> How to build real systems out of fast, typed decisions.

## Fan out

Ask everything you might need in one call, including questions you'll only use in some branches. Flinch answers them together, so the extra ones are nearly free, and your code reads only what applies.

```json
{"text": "...", "questions": [
  {"id": "is_order",  "type": "yes_no", "ask": "Is this about an order?"},
  {"id": "is_refund", "type": "yes_no", "ask": "Is the customer asking for money back?"},
  {"id": "angry",     "type": "yes_no", "ask": "Is the customer angry?"},
  {"id": "lang",      "type": "choice", "ask": "Which language is this written in?", "options": ["English", "Hindi", "Spanish", "other"]}
]}
```

## Gate on confidence

Act above your threshold, ask a person in the middle, refuse to guess at the bottom. See [Confidence](https://goflinch.com/docs/confidence).

## Guard before act

Every text your agent didn't get from its own user goes through [Guard](https://goflinch.com/docs/guard) first. Blocked means the agent never sees it.

## Composite scores

Don't ask "is this a good lead?". Ask the parts, and weigh them in code where you can see and tune the weights:

```python
q = [("budget", 0.4, "Does the sender mention a budget?"),
     ("timeline", 0.3, "Does the sender give a timeline?"),
     ("decider", 0.3, "Is the sender the person who decides?")]
a = flinch.decide(text, [{"id": i, "type": "yes_no", "ask": s} for i, _, s in q])["answers"]
score = sum(w * a[i]["probs"][1] for i, w, _ in q)
```

## Route to the cheapest model

Put [Route](https://goflinch.com/docs/route) in front of your LLM calls. Trivial turns go to the small model, hard ones to the big one, attacks go nowhere. Send feedback and the split gets better on its own.

## Pick one from many

Choose a tool, a skill, a function, a template: put the candidates in `options` and add `none of these`. Below your threshold, fall back to your default instead of the top guess.

---

# System Zero

> Decisions before thought. Why Flinch answers instead of writing.

A flinch is the decision your body makes before you've thought about it. Fast, certain enough to act on, and gone before the slow mind catches up.

**System Zero** is that layer for software. It doesn't reason out loud, doesn't write, doesn't chat. It reads, and it decides.

## Three promises

1. **Typed in, typed out.** You define the question and the possible answers. Flinch can only answer with one of them. No parsing, no "sorry, as an AI", no malformed JSON.
2. **Numbers you can act on.** Every answer comes with a probability for every option and a confidence. Your code decides what's good enough.
3. **On your silicon.** Flinch runs where your data already is. Nothing is sent anywhere, so a decision costs $0 and leaks nothing.

## Code stays in control

Flinch makes narrow calls; your code does everything else. Break a big judgment into small questions, ask them all at once, combine the answers with rules and weights **you** own. When Flinch isn't sure, it tells you, and your code chooses the fallback: a person, a bigger model, or a safe default.

Read [How to build with Flinch](https://goflinch.com/docs/patterns) next.
