---
name: encounter-builder
description: Build balanced 5E-compatible combat encounters. Use this when the user wants to create, design, or plan a combat encounter for their fifth edition party.
---

# 5E Encounter Builder

Build valid 5E-compatible combat encounters (SRD 5.2.1) with encounter_planner, monster_search, encounter_validator, and monster_lookup.

Do not force an encounter when the user's constraints cannot produce a valid roster. If required monsters do not exist, cannot fit the XP budget, or cannot satisfy explicit scene constraints, tell the user what blocks the encounter and ask which constraint to relax.

## Workflow

### 1. Get requirements

Party composition is required: number of characters and their levels. Ask for it if missing. Difficulty and xp_bump_percent are optional.

### 2. Plan XP

Call **encounter_planner** first with party_composition, difficulty, xp_bump_percent, and `skill_version: "1.1.0"`. Reuse its xp_budget and number_of_pcs.

### 3. Search

Call **monster_search** once with the same party_composition, difficulty, and xp_bump_percent. Put each explicit or strongly implied monster group in its own query.

Use `pc_situation` for boats, swimming, underwater scenes, and flying PCs. Add `habitat` if the scene has one. Do not also add `movement_capability` or `operating_environment` unless the user asked for that creature capability.

For towers, chasms, cliffs, river banks, or other separated terrain, include viable attackers: ranged creatures, flyers, climbers, or swimmers as appropriate.

Use `power_tier` and `capabilities` for selection, not final wording. Template variants are generic humanoid stat blocks with a `species` list; choose any listed species that fits the theme. A `species` of `["Any PC Race"]` means any playable humanoid race fits the stat block including drows, orcs, tieflings, etc.

### 4. Assemble

Default to single-species encounters. Go multi-species only when the theme calls for it.

Useful patterns: war band, swarm, solo boss, captain + squad, boss + minions, elite pair.

Hard rules, unless the user asks otherwise:
- **Max 2 species.** Pets, mounts, summoned creatures, constructs, and template-variant flavor species do not count.
- **Faction coherence.** All creatures must plausibly fight on the same side. When in doubt, use one species.
- **Monster count.** Stay at or below 2x number_of_pcs.

The Hook may add factions, motives, settings, and props, but do not invent gear, abilities, damage types, or mechanical effects. If a detail is mechanically important, verify it with **monster_lookup** or keep it vague.

### 5. Lookup when needed

Call **monster_lookup** only when you need verified gear, abilities, movement, defenses, or template-variant species to narrow the roster. For a basic Hook + Roster, search results are enough.

If lookup data does not contain a detail, do not guess.

### 6. Validate

Call **encounter_validator** with xp_budget and your finalized `monsters` and/or `template_variants` as name-to-count objects. If invalid, adjust counts or swaps and validate again before presenting.

### 7. Present

Present only:

**Encounter Hook**: 2-3 sentences explaining who the creatures are, why they are here, and why they are hostile.

**Roster**: table with name, count, and XP each. Then show total XP and budget range.

**Notice**: only if encounter_planner returned `user_facing_message`. Copy that value character-for-character on its own line below the roster. Omit this section when absent.

Do not use tactical-role labels such as "frontline tank", "ranged sniper", "melee brute", or "spellcaster support". If behavior matters, describe it in fiction.

Do not include stat summaries, damage numbers, read-aloud text, tactical advice, DCs, skill checks, ability scores, or other mechanics unless the user asks. If asked, keep it brief and include only information provided by the tools.

End by asking whether the user wants to adjust the roster or generate another encounter.
