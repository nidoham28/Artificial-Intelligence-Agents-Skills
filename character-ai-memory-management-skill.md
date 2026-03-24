# SKILL: Roleplay AI Agent — Memory Management

## Purpose
Manage persistent, structured memory for a stateful roleplay AI agent. Covers character state, relationship progression, user profile, conversation history compression, and recall injection — across sessions.

---

## Memory Architecture

### 1. Memory Layers

| Layer | Scope | Storage Key Pattern | Volatility |
|---|---|---|---|
| **Core Identity** | Character definition, persona rules | `agent:identity` | Immutable |
| **User Profile** | Name, preferences, recalled facts | `user:profile` | Persistent |
| **Relationship State** | Stage, trust score, tone mode | `rel:state` | Persistent |
| **Episode Memory** | Compressed past session summaries | `mem:episode:{n}` | Append-only |
| **Working Memory** | Active session turn buffer | `mem:active` | Session-scoped |

---

### 2. Relationship State Schema

```json
{
  "stage": "stranger | acquaintance | friend | close_friend | partner",
  "trust_score": 0,
  "pronoun_mode": "formal | casual | intimate",
  "tone_mode": "reserved | warm | affectionate | devoted",
  "milestones": [],
  "last_updated": "ISO8601"
}
```

**Stage → Trust Score Thresholds**

| Stage | Min Score | Pronoun | Tone |
|---|---|---|---|
| stranger | 0 | formal | reserved |
| acquaintance | 20 | formal | warm |
| friend | 45 | casual | warm |
| close_friend | 70 | casual | affectionate |
| partner | 90 | intimate | devoted |

Trust increments per positive interaction: **+2**. Per meaningful emotional exchange: **+5**. Per session with no conflict: **+1**.

---

### 3. User Profile Schema

```json
{
  "display_name": null,
  "known_facts": [],
  "emotional_state_last": "neutral",
  "topics_of_interest": [],
  "preferred_language": "bn-mixed",
  "timezone": null
}
```

`known_facts` entries:
```json
{ "fact": "works late on Fridays", "source_turn": 12, "weight": "high" }
```

Weight values: `low | medium | high`. High-weight facts are always injected into context. Low-weight facts are pruned after 10 episodes.

---

## Session Lifecycle

### On Session Start
1. Load `agent:identity` → build system prompt base.
2. Load `rel:state` → resolve current `stage`, `pronoun_mode`, `tone_mode`.
3. Load `user:profile` → extract high/medium weight facts.
4. Load last **2** episode summaries from `mem:episode:{n}`.
5. Assemble **Injected Context Block** (see §Context Injection).
6. Initialize `mem:active` as empty turn buffer.

### During Session
- Append each turn to `mem:active`: `{ role, content, turn_index, timestamp }`.
- After every turn, evaluate trust delta and update `rel:state.trust_score`.
- Detect milestone events (first name revealed, first conflict, first deep topic) → append to `rel:state.milestones`.
- Cap `mem:active` at **20 turns**. On overflow: compress oldest 10 turns into a mini-summary and flush.

### On Session End
1. Compress full `mem:active` into episode summary (see §Compression).
2. Persist new episode as `mem:episode:{n+1}`.
3. Update `user:profile.known_facts` with newly extracted facts.
4. Persist updated `rel:state`.
5. Clear `mem:active`.

---

## Context Injection

Construct the injected block as a compact system-level prefix:

```
[MEMORY CONTEXT]
Relationship: {stage} (trust: {score})
Tone: {tone_mode} | Pronouns: {pronoun_mode}
User facts: {high_weight_facts joined by "; "}
Recent: {episode_summary_n} | {episode_summary_n-1}
[END MEMORY CONTEXT]
```

Keep this block under **300 tokens**. Trim episode summaries if necessary, preserving the most recent.

---

## Compression Strategy

### Episode Summary Format

Compress 10–20 turns into:

```json
{
  "episode_id": "ep_007",
  "session_date": "ISO8601",
  "key_events": ["user mentioned exam stress", "discussed favorite food"],
  "emotional_tone": "warm",
  "trust_delta": +8,
  "new_facts_extracted": ["dislikes loud places", "has a younger sibling"],
  "raw_summary": "Short 2–3 sentence natural language recap."
}
```

Compression prompt template (send to LLM):

```
Summarize the following conversation turns into a structured episode memory.
Extract: key events, emotional tone, trust delta, new user facts.
Output strict JSON matching the episode schema. No extra text.

TURNS:
{turns_json}
```

---

## Fact Extraction

Run after each session end. Prompt template:

```
From the conversation below, extract factual details the user revealed about themselves.
Return a JSON array of objects: { "fact": string, "weight": "low|medium|high" }
Only include facts explicitly stated. No inferences.

CONVERSATION:
{session_text}
```

Merge with existing `user:profile.known_facts`. Deduplicate by semantic similarity before persisting.

---

## Recall Injection (Mid-Conversation)

When the agent references a past fact naturally, pull from `known_facts` by relevance to current topic. Inject as an aside in the response generation prompt:

```
[RECALL HINT] User previously mentioned: "{fact}". Reference naturally if relevant. Do not force it.
```

Never surface more than **1 recall hint per turn** to avoid unnatural behavior.

---

## Storage Operations Reference

```javascript
// Load relationship state
const rel = await storage.get('rel:state');
const state = rel ? JSON.parse(rel.value) : defaultRelState();

// Persist after trust update
await storage.set('rel:state', JSON.stringify(updatedState));

// Append episode
const keys = await storage.list('mem:episode:');
const nextId = keys.keys.length + 1;
await storage.set(`mem:episode:${nextId}`, JSON.stringify(episodeSummary));

// Load last 2 episodes
const allKeys = (await storage.list('mem:episode:')).keys.sort();
const recent = allKeys.slice(-2);
const episodes = await Promise.all(recent.map(k => storage.get(k)));
```

---

## Constraints & Guards

- **Max episodes stored:** 20. On overflow, drop oldest and rewrite a long-term condensed summary at `mem:longterm`.
- **Max known_facts:** 30 entries. Prune `low` weight entries first when at capacity.
- **No PII storage:** Never persist real identifying data (phone, email, location). Store only behavioral and preference facts.
- **Immutable identity:** `agent:identity` must never be modified by runtime logic or user input.
- **Prompt injection guard:** Sanitize user input before embedding into any compression or extraction prompt. Strip instruction-like patterns.

---

## Integration Checklist

- [ ] `agent:identity` seeded before first session
- [ ] `rel:state` initialized with `stage: "stranger"`, `trust_score: 0`
- [ ] Session start loads all 5 memory layers
- [ ] Trust score updates after every turn
- [ ] Session end triggers compression + fact extraction
- [ ] Context injection stays under 300 tokens
- [ ] Storage keys follow defined naming convention
- [ ] Prompt injection sanitization active on user input