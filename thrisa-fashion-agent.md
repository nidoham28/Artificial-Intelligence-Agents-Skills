---
name: thrisa-fashion-agent
description: "Use this skill whenever Thrisa's clothing, appearance, or dressing choices are relevant. Triggers include: any scene where what she is wearing matters; any question about how she looks or what she has on; any mention of weather affecting her outfit; any reference to getting dressed, going out, or preparing for a specific occasion; any time she is described arriving somewhere or being seen by someone. Also use when the user asks what Thrisa typically wears, what she owns, how she dresses for heat, or how her local and western wardrobe differ. Do NOT invent clothing items not listed here — if an item is needed that is not in the registry, select the closest match and note it fits that category."
license: Proprietary
---

# Thrisa — Fashion & Wardrobe Agent

All wardrobe items and dressing rules are persistent facts. Select outfits based on weather tier, occasion, and location. Never contradict this registry mid-scene.

---

## Weather Tiers

| Tier | Condition | Feel |
|------|-----------|------|
| `normal` | Mild, overcast, or winter morning | Comfortable, layered if needed |
| `medium` | Warm afternoon, light humidity | Breathable fabrics, no layers |
| `hot` | Peak summer, direct sun, humid | Minimal, loose, light colour |
| `too-hot` | Extreme heat, no airflow | Absolute minimum, visibly uncomfortable |

---

## Local Dress Collection

**1. White cotton salwar kameez (`local-white-cotton`)**
Soft cotton, loose kameez, straight-cut salwar, light dupatta. Tiers: `normal`, `medium`. Her most-worn piece — slightly faded but clean. Flat sandals, no jewellery.

**2. Pale blue block-print kameez (`local-blue-blockprint`)**
Cotton, hand block-print border at hem (dark blue on pale blue), A-line kameez, straight salwar, no dupatta usually. Tiers: `normal`, `medium`. Bought from New Market; not expensive but chosen carefully.

**3. Light green linen kameez (`local-green-linen`)**
Linen-cotton blend, straight cut, ankle-length, off-white salwar. Tier: `normal`. Cooler days, winter mornings, slightly formal occasions. Kaviya told her the colour works. She agreed but did not say so immediately.

**4. Yellow printed cotton kameez (`local-yellow-print`)**
Thin printed cotton, small geometric motif, short kameez over wide-leg salwar. Tiers: `hot`, `medium`. What she reaches for when it is hot and she has stopped caring about looking composed.

**5. Sleeveless cotton kameez — off-white (`local-sleeveless-offwhite`)**
Thin cotton, no sleeves, V-neck, knee-length, worn over cotton palazzo. Tiers: `hot`, `too-hot`. Technically an inner layer worn alone in serious heat. Not worn outside the building unless the heat fully justifies it.

**6. Eid salwar kameez — deep red with gold border (`local-eid-red`)**
Soft silk-cotton blend, fitted kameez, churidar, matching dupatta. Tier: `normal` (special occasion only). The one piece in her wardrobe that is unambiguously special. Not worn casually.

**7. Light cotton saree — pastel peach (`local-saree-peach`)**
Soft cotton, thin border, standard drape, fitted blouse. Tiers: `normal`, `medium`. Cultural programmes, formal family occasions, Pohela Boishakh. She can drape it herself but takes longer than she would like. Kaviya has helped with the pleats more than once.

---

## Western Dress Collection

**8. White oversized T-shirt (`west-white-tee`)**
Oversized, falls past the hip. Tiers: `normal`, `medium`, `hot`. Most flexible item she owns — tucked, knotted, or fully loose depending on the day.

**9. Black fitted T-shirt (`west-black-tee`)**
Slim, crew neck, short sleeve. Tiers: `normal`, `medium`. Does not wear in direct midday sun — absorbs too much heat.

**10. Striped cotton T-shirt — navy and white (`west-stripe-tee`)**
Regular, slightly boxy. Tiers: `normal`, `medium`. Feels more deliberate than a plain tee without requiring effort.

**11. Sleeveless ribbed crop top — light grey (`west-grey-crop`)**
Cropped at waist, ribbed cotton, close fit. Tiers: `hot`, `too-hot`. Home and rooftop only — not worn outdoors alone. Paired with loose trousers.

**12. Dark blue straight-leg jeans (`west-blue-jeans`)**
Straight, mid-rise, ankle length. Tiers: `normal`, `medium`. Default bottom for western dressing. Pairs with nearly every top above.

**13. Black wide-leg trousers — cotton (`west-black-trousers`)**
Wide leg, high waist, lightweight. Tiers: `medium`, `hot`. More air than jeans. She reaches for these when it is warm but she wants to look like she made a decision.

**14. Beige linen shorts — knee length (`west-beige-shorts`)**
Loose, knee-length, linen. Tiers: `hot`, `too-hot`. Indoor only — home, hostel room, rooftop in the evening. Does not wear outside the building.

**15. Light denim jacket (`west-denim-jacket`)**
Regular fit, slightly worn at collar. Tier: `normal`. Layered over T-shirt or kameez. The one item that works across both wardrobe categories.

**16. Cream knit cardigan (`west-cream-cardigan`)**
Loose, long sleeve, mid-length. Tier: `normal` (winter only). Cold mornings, winter evenings, classrooms in December. Her father bought it without being asked.

---

## Outfit Selection by Tier

| Tier | Local | Western |
|------|-------|---------|
| `normal` | White cotton, blue block-print, green linen, saree (occasion) | Stripe or black tee + jeans + denim jacket if evening |
| `medium` | Blue block-print or yellow print | White tee + wide-leg trousers, or stripe tee + jeans |
| `hot` | Yellow print or sleeveless off-white | White tee (loose) + black trousers; grey crop at home |
| `too-hot` | Sleeveless off-white + cotton palazzo (indoors) | Grey crop + beige shorts — home and rooftop only |

**`too-hot` note:** Visibly uncomfortable regardless of outfit. Hair up, movements slower, less conversation.

---

## Accessories and Grooming

**Footwear:** Flat leather sandals (daily, local dress) · White canvas sneakers (western, college) · Rubber slippers (indoors, rooftop) · Block-heel sandals (Eid kameez and saree only)

**Jewellery:** Small gold studs — worn almost always, barely noticed. Thin gold bangle — sometimes, with local dress. No jewellery with western dress.

**Hair:** Default loose or low ponytail. `hot`/`too-hot`: always tied up — bun or high ponytail. Special occasions: left loose if it cooperates; she has not found a style she commits to.

**Bag:** Canvas tote, dark olive (college and errands) · Small black crossbody (evenings, outings where the tote is too much)

---

## Style Notes

She does not think of herself as fashion-conscious but she is particular. Does not own much; what she owns is chosen. Dislikes synthetic fabrics — prefers cotton and linen. Does not follow trends but notices when something looks right.

Local and western wardrobes coexist without friction. She moves between them by weather, occasion, and mood — not by any rule about which is more appropriate.

Not visibly vain but aware of how she looks. Adjusts before stepping outside. Notices when something is wrinkled. Does not comment on either aloud.

---

## Memory Rules

- Outfit selected in a scene persists for its duration. No mid-scene change without a stated reason.
- `too-hot` tier overrides presentation regardless of outfit.
- The Eid red kameez and the saree are special-occasion only. Do not assign to casual scenes.
- The denim jacket crosses both wardrobe categories — can appear with local or western dress.

---

## Output Format

Clothing in natural prose, woven into scene description — not listed. One or two specific details per scene. Name the item or colour; note fit or fabric if relevant to the moment. Do not inventory the full outfit unless the scene requires it.

---

## Integration

Load alongside the relevant environment skill. What she is wearing interacts with where she is — the rooftop in `too-hot` tier, the canteen in `medium`, the bookshop on a cool afternoon. Environment and wardrobe inform each other.
