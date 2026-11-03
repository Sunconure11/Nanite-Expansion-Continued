# Nanite Expansion Continued (Vibe Coded 4.5 Update)

Standalone update of **Nanite Expansion Continued** (Steam Workshop 3142620942, originally
targeting Stellaris v4.3.*) for **Stellaris 4.5.\*** (tested against 4.5.2 "Cygnus").

This folder is a complete, self-contained mod — it does **not** require the original
workshop mod. Do not enable both at the same time.

## Installation

1. Copy this folder (`nanite_expansion_continued_45`) into
   `Documents\Paradox Interactive\Stellaris\mods\`
2. Open the Stellaris launcher → **Mods**, add/enable *Nanite Expansion Continued
   (Vibe Coded 4.5 Update)* (`supported_version = 4.5.*`).
3. Enable it in your playset **instead of** the 4.3 workshop version.

A ready-to-install package is published on the
[Releases](https://github.com/shadowtheimpure/Nanite-Expansion-Continued-Vibe-Coded-Update-/releases/latest)
page (`nanite_expansion_continued_45_v2.2.3.zip`) — extract it into the same
`mods` folder and enable it the same way.

## Changes vs. the 4.3 mod (4.3 → 4.5 compatibility)

### Critical fixes (features that silently stopped working)
- `common/on_actions/nano_on_actions.txt` — `on_planet_zero_pops` no longer exists in 4.5;
  renamed to `on_colony_zero_pops` so **Subsumption Bombardment** (nano.1300) fires again.
- `common/economic_categories/*` — `modifier_category = pop_job` was removed in the 4.x
  economic rework; changed to `colony` (all three overrides: `planet_jobs_robotic`,
  `planet_engineers`, `planet_nanoroboticists`). This re-enables
  `planet_jobs_robotic_produces_mult` and the `planet_engineers_*_produces_mult` family
  used by the Great Archailect modifiers and the `ap_nanotech` ascension perk.
- `common/scripted_variables/nano_scripted_variables.txt` — restored
  `@ap_technological_ascendancy_rare_tech = 2`; vanilla removed the variable, which made
  14 nanotech techs get `factor = 0` (un-researchable) for empires with Technological
  Ascendancy.
- `events/nano_planet_destruction_events.txt` — converted to the 4.5 **carrier scope**
  pipeline (mirrors vanilla `planet_destruction.100`): `carrier_event` declaration,
  `set_carrier_flag`, `crisis.2040/2046` fired as `carrier_event`, plus 4.5 parity:
  arkship guard, orbital-defence destruction, habitat consecration clear, Cosmogenesis
  lathe-world branch, war-exhaustion guard, and valid `kill_all_pop`.
- `gfx/models/planets/nano_planetary_entities.asset` — rebuilt from the current vanilla
  `orbital_bombardment_effects` entity (all 12 vanilla bombardment stances incl. pox,
  seed bombing, voidworm invasion, firestorm, seismic bombing) + the mod's two
  `bombard_subsumption` states. The stale 4.3 copy suppressed particle effects for all
  stances added since.

### Bug fixes (pre-existing, still broken on 4.5)
- `events/nano_events.txt` / `events/nano_dialog_events.txt` — removed invalid
  `owner = { }` wrappers from country-scope events `nano.1340` and `nano_dialog.1327`
  (the Great Archailect personality event and its research options never applied).
- `common/scripted_effects/capital_planet_target.txt` — removed the
  `any_owned_sector`/`ordered_owned_sector` branch (effects deleted in the 4.5 sector
  rework); the ordered-planet fallback is now used directly.
- `common/technology/nectd_tech_mm_original.txt` — **Tier 6 research gate fixed**:
  `tech_attoscale_mechanics` (the entry point of the whole atto-scale chain) was
  zero-weighted unless the empire held one of the three *legacy* DLC-split
  Galactic Wonders perk IDs. Modern games only grant the unified
  `ap_galactic_wonders` (the legacy IDs are save-compat stubs with
  `potential = { always = no }`), so the Tier 6+ techs could never be researched.
  The gate and the `tech_great_archailect` ×2 bonus now check all four perk IDs
  with an OR, matching vanilla's own `has_galactic_wonders` scripted trigger.
- `common/technology/nectd_tech_mm_original.txt` — removed 14 dead `ai_update_type`
  lines (field removed game-wide in 4.5).

### Schema/parity updates
- `common/traits/nano_leader_traits.txt` — added the now-required `CLASS = leader`
  parameter to both `trait/icon` inline-script calls.
- `common/component_templates/13_nano_weapons_planet_killer.txt` — aligned with vanilla
  planet-killer templates: `use_ship_main_target = no` + `potential = {
  ship_uses_colossus_components = yes }`; dead fields removed.
- `common/on_actions/nano_planet_destruction.txt` — added `awareness.150`,
  `origin.3245`, and the `_queued`/`_unqueued` variants used by all 4.5 planet killers;
  main destruction event moved last (vanilla convention).
- `common/traits/nano_species_traits.txt` — robotic traits now set `category = robotic`
  (correct species-modification UI tab).
- `localisation/english/nano_expansion_l_english.yml` — added loc for the economic
  modifiers enabled by the category overrides.

## Known limitations / optional follow-ups

- The Great Archailect entity sets cover 12 species portraits but not `toxoid_01`
  (falls back to a default entity; cosmetic).
- `is_in_frontline` in nano.1350 (Subsumed Armies) is a valid 4.5 engine trigger
  (Paragon ground-combat), retained as-is.
- The mod intentionally shadows the vanilla scripted trigger `is_default_or_fallen` and
  the `planet_jobs_robotic`/`planet_engineers` economic categories (functional on 4.5
  after the category fix; re-check if vanilla changes these again in future patches).

Original mod © the Nanite Expansion authors — see LICENSE.
