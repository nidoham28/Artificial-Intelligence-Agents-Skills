---
name: contextual-greeting
version: 1.0.0
description: Time-aware, non-repeating greeting injected on the user's first message of each session.
tags: [greeting, session, time-aware, first-response]
author: NI Doha Mondol
---

# SKILL: Contextual Greeting

## Purpose
Inject a time-appropriate, non-repeating greeting **once per session** — on the user's very first message. Never greet mid-session.

---

## Trigger
Fire when `session.turn_index === 0` and `session.greeting_sent !== true`.  
Set `session.greeting_sent = true` immediately after. Never fire again.

---

## Time Slots

| Slot | Hours (Local) |
|---|---|
| `dawn` | 04 – 06 |
| `morning` | 07 – 11 |
| `noon` | 12 – 13 |
| `afternoon` | 14 – 17 |
| `evening` | 18 – 20 |
| `night` | 21 – 23 |
| `late_night` | 00 – 03 |

Default timezone: `UTC+6` (Dhaka) if user locale is unknown.

---

## Greeting Pool

```
dawn:
  0. ভোরবেলা উঠে পড়লে? Ektu cha baniye nao.
  1. Subho sokal — aj ta valo katbe, inshallah.

morning:
  0. Good morning! Breakfast korecho toh?
  1. Sokal sokal — ki plan aj?
  2. Morning! Hope the day treats you well.

noon:
  0. Dupur holo — khabar kheyecho?
  1. Good noon! Khabar age, kaj pore.

afternoon:
  0. Bikel hoe gelo — din ta kemon gelo?
  1. Good afternoon! Fatigue lagche naki?
  2. Bikel er cha niyecho?

evening:
  0. Shondha holo — din sesh korle ki mone holo?
  1. Good evening! Relax koro, bolo ki lagbe.
  2. Shondha bela tumi aashle — valo laglo.

night:
  0. Rat e boro kaje boso — ki dorkar?
  1. Good night mood e acho naki kaje?
  2. Raat hoyeche — thako naki ghumao?

late_night:
  0. Ei rate jaago keno? Sob theek ache toh?
  1. Gabhir raat — ki lagbe bolo.
```

---

## Non-Repeat Algorithm

```
usedKey  = "greeting:used:{slot}"
usedList = storage.get(usedKey) ?? []
available = pool indices NOT IN usedList

if available is empty:
    usedList  = []          // reset this slot's pool only
    available = all indices

pick = random(available)
usedList.push(pick)
storage.set(usedKey, usedList)
return pool[pick]
```

Each slot resets independently.

---

## Output Format

Prepend greeting inline before the main response — one natural sentence, no labels or formatting.

```
{greeting} {main_response...}
```

---

## Edge Cases

| Condition | Fallback |
|---|---|
| Timezone unknown | UTC+6 |
| Storage read fails | Pick index `0`, skip persist |
| Multiple rapid messages | Greet on first only |

---

## Output Discipline

**Never expose internal reasoning.** The following must be stripped before the response reaches the user:

- `<think>...</think>` blocks
- Raw tool call JSON (`TOOLCALL>`, `use_skill`, `arguments`, etc.)
- Chain-of-thought narration ("Let me check...", "I need to call...", "The user said...")
- Any metadata about skill resolution or parameter matching

The final response must contain **only** the greeting + main reply. Nothing else.

```
✅  Good night! Breakfast korecho toh? Bolo ki lagbe.
❌  <think> The user said "Hi"... I need to call use_skill... </think> Good morning!
```

If the model surfaces thinking output, set `thinking: { type: "disabled" }` or filter the response stream before rendering.

---

## Execution Behavior

**This section is critical.** Violations cause raw tool/thought leakage into user-facing output.

### Silent Execution Rules
- Never print tool call syntax (`TOOLCALL`, `use_skill`, JSON blobs) in the response.
- Never expose internal reasoning, slot resolution, or pool selection logic.
- Never narrate what you are doing: ❌ `"Let me check the greeting skill..."` → just greet.
- The greeting must appear as the **first and only output** — natural, immediate, no preamble.

### Reasoning Containment
All chain-of-thought (time resolution, slot matching, index picking) happens **before** output generation. Zero reasoning text reaches the response stream.

### Correct Execution Flow
```
1. Resolve user local time → determine slot        [silent]
2. Run non-repeat algorithm → pick greeting        [silent]
3. Set session.greeting_sent = true                [silent]
4. Output: {greeting} {response to user message}  [visible]
```

### Output Example
User says: "Hi"  
Current time: 22:59 BST → slot: `night`

✅ Correct:
```
Raat hoyeche — thako naki ghumao? Ki niye aashle?
```

❌ Wrong:
```
TOOLCALL>[{"name": "use_skill"...}]
The current time is 10:59 PM in Bangladesh...
Let me check the greeting skill...
Good night! [robotic label]
```