# CC0 animated low-poly character and props

Research for [#5](https://github.com/clemeno/calchimi/issues/5) (map [#1](https://github.com/clemeno/calchimi/issues/1)), 2026-11-22.

**Question:** where to get one free low-poly humanoid with at least idle and walk, in glTF/GLB or convertible to it, plus optional Garden/Laboratory props (plants, pots, tables, glassware, still), under CC0 or an equally permissive licence. Candidates: Quaternius, Kenney, KayKit, Poly Pizza, Mixamo.

**Method:** the asset sites' own pages, their licence files and the official KayKit GitHub repos. For the recommended character I also read the glTF JSON inside the GLB from the official repo, parsed into a temp folder outside this repo, to get the clip names and triangle counts. Kenney's download needs a signed link that this session wasn't allowed to fetch, so Kenney formats are unverified.

## What CC0 means here

CC0 deed: "You can copy, modify, distribute and perform the work, even for commercial purposes, all without asking permission." It leaves trademarks and patents untouched and comes with no warranty. ([CC0 1.0 deed](https://creativecommons.org/publicdomain/zero/1.0/))

So committing CC0 files to the public repo and serving them to browsers needs no credit and no permission. CC-BY 3.0 files need a credit line. Mixamo files must not be redistributed raw (see below).

## Summary

| Source | Licence | Formats | Character animations | Polygons | Attribution |
|---|---|---|---|---|---|
| **KayKit Adventurers 1.0** (GitHub) | CC0 | FBX, glTF (GLB for characters) | 76 clips embedded per GLB, incl. `Idle`, `Walking_A/B/C` | Mage: 5,683 tris with accessories, 4,509 without | Not required |
| KayKit Adventurers 2.0 (itch.io) | CC0 | FBX, glTF | "Basic movement animations"; more in the separate free Character Animations pack | Not stated ("low poly optimized") | Not required |
| Quaternius Universal Base Characters + Universal Animation Library | CC0 | glTF, FBX, OBJ, Blend | Characters ship without clips. The library is separate: 45 free, 120+ paid | Average 13k tris | Not required |
| Quaternius older animated packs | CC0 | FBX, OBJ, Blend (no glTF listed) | "Many animations", names not listed | Not stated | Not required |
| Kenney Mini / Blocky / Animated Characters | CC0 | Not stated on pages; Animated Characters series is FBX with separate animation files | "Animation" feature; only `idle.fbx` is named | Not stated | Not required |
| Poly Pizza | Per model: CC0 1.0 or CC-BY 3.0 | FBX/glTF or OBJ/glTF per model | Some models tagged "Animated", clips not listed | Not shown | CC-BY models need credit |
| Mixamo | Adobe terms, not CC0 | FBX (glTF not offered) | Large library | Varies | Not required, but raw files must not be redistributed |

## KayKit (Kay Lousberg)

- **Licence:** the official repo's `LICENSE.txt` reads: "License: (Creative Commons Zero, CC0) … This content is free to use in personal, educational and commercial projects. Support me by … crediting Kay Lousberg … (this is not mandatory)". ([LICENSE.txt](https://github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0/blob/main/LICENSE.txt)) The itch page also gives CC0 and "no attribution required". ([itch.io](https://kaylousberg.itch.io/kaykit-adventurers))
- **Adventurers 1.0 (GitHub):** the README lists "4 (+2 in the EXTRA tier)" characters, "75 animations", "Included files are .FBX, .GLTF", and a "single gradient atlas texture (1024x1024)". ([README](https://github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0/blob/main/README.md)) The repo tree has five character GLBs (Barbarian, Knight, Mage, Rogue, Rogue_Hooded), about 3.6 MB each. ([repo](https://github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0/tree/main/addons/kaykit_character_pack_adventures/Characters/gltf))
- **Inside `Mage.glb`** (read from the glTF JSON, generator "Khronos glTF Blender I/O v1.7.33"):
  - 76 animation clips, all in the one file. They include `Idle`, `Walking_A`, `Walking_B`, `Walking_C`, `Walking_Backwards`, `Running_A`, `Interact`, `PickUp`, `Use_Item`, `Sit_Chair_*`, `Spellcasting`, `Cheer` and `Throw`.
  - `Idle` and `Walking_A` each last 1.07 s.
  - One skin with 41 joints and one embedded PNG texture (16 KB).
  - 5,683 triangles in total. The spellbook (two variants), wand and staff are separate rigid nodes worth 1,174 tris, which can be hidden, leaving 4,509.
- **Adventurers 2.0 (itch.io):** the free tier has five characters with weapons and accessories. Extra ($7.95+) adds three characters and alternate textures. Source ($11.95+) adds .blend files. "Basic movement animations included." ([itch.io](https://kaylousberg.itch.io/kaykit-adventurers), [kaylousberg.com](https://kaylousberg.com/game-assets/characters-adventurers))
- **Character Animations (free, CC0):** "161 Humanoid animations" in FBX and glTF for Rig_Medium and Rig_Large, version 1.1 (devlog dated 2025-12-10). Other humanoids need engine retargeting. ([itch.io](https://kaylousberg.itch.io/kaykit-character-animations), [kaylousberg.com](https://kaylousberg.com/game-assets/character-animations))

## Quaternius

- **Licence:** "All models are under the CC0 License." "No, attribution is not necessary." Free for "commercial, educational, and personal projects". ([FAQ](https://quaternius.com/faq.html))
- **Universal Base Characters (Aug 2025):**
  - Formats: FBX, OBJ, Blend, glTF.
  - Six base models, "Average 13k tris".
  - The "Animated" field is marked with an X: no clips. The rig is "Compatible with Universal Animation Library". ([quaternius.com](https://quaternius.com/packs/universalbasecharacters.html))
  - The itch free tier is "2 base models and 5 hairstyles". ([itch.io](https://quaternius.itch.io/universal-base-characters))
- **Universal Animation Library:** CC0, "universal humanoid rig". The free Standard tier has "45 completely free animations". Pro ($9.99+) has "All 120+ animations in FBX and GLB". v3.0 (2026-06-16) added root motion. Clip names are not listed. ([itch.io](https://quaternius.itch.io/universal-animation-library), [quaternius.com](https://quaternius.com/packs/universalanimationlibrary.html))
- **Older packs:**
  - Ultimate Animated Character Pack (Nov 2019): 52 models in FBX, OBJ, Blend. ([page](https://quaternius.com/packs/ultimatedanimatedcharacter.html))
  - Animated Man Pack (Oct 2017): "atlas textured male with many animations", FBX, OBJ, Blend. ([page](https://quaternius.com/packs/animatedman.html))
  - Neither page lists glTF, clip names or triangle counts, so FBX would need converting to GLB.
- **Trade-off:** a CC0 path with glTF, but the character and the clips come from two packs that must be joined. At 13k tris the base characters are the heaviest option here.

## Kenney

- **Licence:** "all game assets on the asset pages are public domain licensed (CC0)… Attribution is not required." ([support](https://kenney.nl/support)) Each asset page shows "Creative Commons CC0". ([Mini Characters](https://kenney.nl/assets/mini-characters))
- **Characters:** all marked "Animation" on their pages; no page lists formats or clip names.
  - Mini Characters: 40 files, v2.0. ([page](https://kenney.nl/assets/mini-characters))
  - Blocky Characters: 20 files, v2.0 "Completely remade". ([page](https://kenney.nl/assets/blocky-characters))
  - Animated Characters Protagonists: 8 files, 2020. ([page](https://kenney.nl/assets/animated-characters-1))
- **Formats:**
  - The knowledge base says "The glTF file format is distributed as GLB in Kenney game assets", and that FBX and GLB "are equal in features but GLB is smaller". ([KB](https://kenney.nl/knowledge-base/game-assets-3d/importing-3d-models-into-game-engines))
  - The Animated Characters series uses `characterMedium.fbx` plus separate animation files such as `idle.fbx`. ([KB](https://kenney.nl/knowledge-base/game-assets-3d/importing-characters-and-animations))
- **Unverified:** whether the Mini or Blocky downloads include GLB files with idle and walk clips. The download needs a signed link that this session wasn't allowed to fetch.

## Poly Pizza

- An aggregator: "10,700+ free models" and an API. ([poly.pizza](https://poly.pizza/))
- **Licence per model:** each model page shows its own licence, for example "Public Domain (CC0)" ([Quaternius Cauldron](https://poly.pizza/m/QaWJOPa6Gt)) or "Creative Commons Attribution" ([Erlenmeyer flask, Poly by Google](https://poly.pizza/m/eqIGxcsBe1V)).
- **Credits:** generated credits pages use the form `"Chair" by "Quaternius" is licenced under CC0 1.0`. ([credits example](https://poly.pizza/l/cCOMNksEDO/credits))
- **Formats:** per model, "FBX/GLTF format" or "OBJ/GLTF format". Clips and triangle counts are not shown.
- **Watch out:** Poly Pizza lists Quaternius's "Animated Base Character" as CC-BY 3.0 ([page](https://poly.pizza/m/cwYvO5UauX)), while Quaternius's own site gives CC0 for all its models. Trust the author's page. When the author's page isn't available, follow Poly Pizza's label and credit CC-BY models.

## Mixamo (Adobe)

- **Not CC0.** Adobe's FAQ: characters and animations can be used "royalty free for personal, commercial, and non-profit projects including … creating video games". Free with an Adobe ID, no Creative Cloud subscription needed. Not available for Enterprise or Federated IDs. ([Mixamo FAQ](https://helpx.adobe.com/creative-cloud/faq/mixamo-faq.html); the page blocks automated fetching, so these lines come from Adobe's indexed page text.)
- **Redistribution:** the pinned community FAQ (by a community member, not labelled Adobe staff) forbids "Any type of free distribution of character or animation raw files" and says "really the only thing you can't do is distribute the raw character and animation files". Credit is not required. ([community FAQ](https://community.adobe.com/questions-696/mixamo-faq-licensing-royalties-ownership-eula-and-tos-589400))
- **Formats:** uploads are FBX, OBJ or ZIP ([rigging help](https://helpx.adobe.com/creative-cloud/help/mixamo-rigging-animation.html)). The only download format Adobe help names is FBX ([Aero import](https://helpx.adobe.com/africa/aero/using/import-mixamo-assets.html)). No glTF.
- **Verdict:** committing Mixamo FBX/GLB to the public `clemeno/calchimi` repo would be free distribution of raw files. Avoid it.

## Props for the Garden and Laboratory

The environment can stay primitives. If props are wanted, these are all CC0 and glTF unless noted:

| Need | Source |
|---|---|
| Crops and plants | Quaternius Ultimate Crops: "100+ models of different crops in 5 stages of growth", FBX/OBJ/Blend only ([page](https://quaternius.com/packs/ultimatecrops.html)). Kenney Nature Kit: 330 files ([page](https://kenney.nl/assets/nature-kit)). KayKit Furniture Bits: `cactus_*` potted plants ([repo](https://github.com/KayKit-Game-Assets/KayKit-Furniture-Bits-1.0)) |
| Pots, jars, bowls | KayKit Restaurant Bits: `pot_A/B`, `pot_large`, `jar_A–D` in three sizes, `bowl*`, `crate_*` with vegetables ([repo](https://github.com/KayKit-Game-Assets/KayKit-Restaurant-Bits-1.0)) |
| Tables, shelves, books | KayKit Furniture Bits: `table_*`, `shelf_*`, `book_set` ([repo](https://github.com/KayKit-Game-Assets/KayKit-Furniture-Bits-1.0)). KayKit Dungeon Remastered: `table_*`, `shelf_*` ([repo](https://github.com/KayKit-Game-Assets/KayKit-Dungeon-Remastered-1.0)). Kenney Furniture Kit: 140 files ([page](https://kenney.nl/assets/furniture-kit)) |
| Glassware, potions | KayKit Dungeon Remastered: `bottle_A/B/C` in brown and green, labelled variants, `candle*` ([repo](https://github.com/KayKit-Game-Assets/KayKit-Dungeon-Remastered-1.0)). Quaternius Potion Bottle on Poly Pizza ([page](https://poly.pizza/m/7qbuf1TS8C)). Quaternius Fantasy Props MegaKit: "potions", "books", 211 models, glTF, 60–70% free ([page](https://quaternius.com/packs/fantasypropsmegakit.html)) |
| Cauldron | Quaternius Cauldron on Poly Pizza: CC0, FBX/glTF ([page](https://poly.pizza/m/QaWJOPa6Gt)) |
| Still / alembic | No CC0 model found. The Poly Pizza "alembic" search returns no stills, and its glassware hits are CC-BY ([search](https://poly.pizza/search/alembic)). Build one from primitives: a cauldron, a cylinder and a tube. |

The KayKit packs above share the same `LICENSE.txt`: "(Creative Commons Zero, CC0) … free to use in personal, educational and commercial projects … (this is not mandatory)" ([Dungeon Remastered](https://github.com/KayKit-Game-Assets/KayKit-Dungeon-Remastered-1.0/blob/main/LICENSE.txt), [Furniture Bits](https://github.com/KayKit-Game-Assets/KayKit-Furniture-Bits-1.0/blob/main/LICENSE.txt), [Restaurant Bits](https://github.com/KayKit-Game-Assets/KayKit-Restaurant-Bits-1.0/blob/main/LICENSE.txt)). They also share one gradient-atlas look with the recommended character.

## Recommendation: KayKit Adventurers 1.0, `Mage.glb`

- **URL:** https://github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0/blob/main/addons/kaykit_character_pack_adventures/Characters/gltf/Mage.glb (itch page: https://kaylousberg.itch.io/kaykit-adventurers)
- **Licence:** CC0 1.0. Credit to Kay Lousberg is optional ([LICENSE.txt](https://github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0/blob/main/LICENSE.txt)).
- **Format:** a single binary GLB of about 3.6 MB with the texture embedded. It loads as-is with three.js `GLTFLoader`, no conversion. FBX is also in the repo.
- **Animations:** 76 clips embedded in the same file, including `Idle` and `Walking_A` (1.07 s each), plus `Running_A`, `Interact`, `PickUp`, `Use_Item`, `Sit_Chair_*` and `Spellcasting`. These cover tending plants, working at Setups and sitting in the Consultation room.
- **Size:** 5,683 tris, or 4,509 with the spellbook, wand and staff nodes hidden. 41-joint skin, 16 KB atlas texture.
- **Why this one:**
  - The only candidate verified to be CC0 and a single glTF file with idle and walk inside it.
  - The robed, hatted Mage reads as an alchemist.
  - KayKit Furniture, Restaurant and Dungeon Bits supply matching CC0 props.
  - The Quaternius route needs two packs joined and weighs about 13k tris. Kenney formats are unverified. Mixamo's terms forbid putting raw files in a public repo.
