# Known limits

> Where Flinch is weak today, and what to do instead.

We'd rather you hear it here than find it in production.

| Limit | Do this instead |
| --- | --- |
| **Reads literally.** It answers what you asked, not what you meant. | Spell the condition out. Split a vague question into sharp ones. |
| **Arithmetic and counting.** | Do the math in code. Ask Flinch for the parts (which amount, which date), not the result. |
| **Dates and intervals.** | Extract the date, compare in code. |
| **Double negatives and indirection.** | Ask directly: "Is it allowed?", not "Is it not prohibited?". |
| **Noise in the text.** Unrelated history and boilerplate lower accuracy. | Send only what the question is about. |
| **Languages.** English is strongest; some languages are clearly weaker. | Test your language on your own examples before you rely on it. |
| **Steering attempts in the text.** Guard catches most, not all. | Treat Guard as a strong filter, not a guarantee. Keep agents on least privilege. |
| **No writing.** Flinch never generates text. | Use it to decide; use a generative model to write. |
| **Questions don't check each other.** Two related answers can disagree. | Ask the one question you actually need, or reconcile in code. |

Each release of Flinch narrows this list. The [changelog](https://goflinch.com/docs/api#versions) says what moved.

---

# Models

> Nano, Pro and Max. One family for edge, client and datacentre.

| Model | Built for | Runs |
| --- | --- | --- |
| **Flinch Nano** | Edge | phones, small machines, every browser tab |
| **Flinch Pro** | Client | laptops and desktops |
| **Flinch Max** | Datacentre | servers and workstations |

Same question types, same answers, same API. Flinch System picks the model that fits your silicon and memory; you don't change a line of code to move between them.

## Speed

Flinch Nano decides in about **4 ms** on a typical laptop. Pro and Max trade a little time for accuracy on harder text. Every response tells you exactly how long it took on your machine (`ms`).

## Cost

$0 a decision. Flinch runs on your silicon, so there's no token bill. Flinch System counts decisions against your plan; your first 10K are free.

## Input

- Text only. Turn images, audio and PDFs into text first.
- Long text is read in parts; `/v1/check` tells you when it had to cut (`truncated`).
- English is strongest. Other languages work, with lower accuracy. Test yours before you rely on it.

## Your data

Nothing you send to Flinch leaves your machine. Flinch System doesn't keep your text; it keeps counts.
