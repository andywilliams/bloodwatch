# 🧛 BLOODWATCH
### Game Design Document v0.1
*A GBA turn-based tactics RPG*

---

## Premise

The village of Ashveil has been silent for three days. No ravens. No traders. No word.

You are the last active chapter of the Order of Bloodwatch — a ragtag band of hunters, scholars and unfortunates who track vampire incursions across the realm. Your orders: descend into Castle Vourdalak, find out what happened, kill whatever's responsible.

You will lose people. That's the job.

---

## Core Loop

```
Town → Dungeon Floor → Combat → Loot/Rest → Next Floor → Boss
```

- **Town phase** — hire replacements, buy supplies, read lore, upgrade gear
- **Dungeon phase** — procedurally generated grid floors, fog of war
- **Combat** — turn-based tactics on the room grid
- **Boss floor** — handcrafted, named vampire lord with a backstory

Each run is a **campaign** — 5 dungeon depths, escalating vampire power. Beat the Vampire Lord to win. Lose your whole party and it's over.

---

## The Party

You field up to **4 hunters** at once. You can recruit up to 8 total — the rest wait in town.

### Starter Classes

🗡️ **Warden**
The backbone. Sword + shield, can hold a doorway indefinitely. Boring to play, devastating to lose.
- *Passive:* Adjacent allies take 1 less damage
- *Special:* Brace — skip turn, next hit does 0 damage

🏹 **Strider**
Crossbow, light armour, coward's instincts. Stays at the back, picks off anything that gets past the Warden.
- *Passive:* +2 range on open tiles
- *Special:* Bolt of Faith — silver bolt, ignores vampire resistances

📖 **Prior**
Old man. Shouldn't be here. The only one who knows what any of this means.
- *Passive:* Allies within 2 tiles resist charm effects
- *Special:* Last Rites — heal an adjacent ally OR damage an undead

🔥 **Ashcaller**
Burned her village down by accident once. Deeply apologetic about it.
- *Passive:* Fire lingers — burning tiles deal 1 dmg for 2 turns
- *Special:* Immolate — 3x3 AOE. Yes, it hits your own people.

🐺 **Greyborn**
Half werewolf. Technically on probation.
- *Passive:* Regenerates 1 HP per turn
- *Special:* Frenzy — double attacks, can't be controlled next turn
- *Full moon floors:* Goes berserk. Can be a problem.

---

## The Vampires

Not mindless monsters. Each type has **behaviour AI** and hungers:

🧛 **Thrall** — turned human, confused, tragic. Weak but numerous.
🦇 **Nightwing** — bat swarm form. Ignores walls. Terrifying in corridors.
🌫️ **Veilborn** — mist form, passes through your party, attacks from behind.
👁️ **Elder** — ancient, tactical. Will target your Prior first. Always.
🩸 **The Lord** — boss. Unique per campaign. Named. Has a history. *Wants something.*

---

## Death

When a hunter dies, they're gone.

Before they go, they get **one line**:

> *"Tell my brother I found it. The thing I was looking for."*
> — Brother Aldric, Strider. Floor 3.

You don't know what he was looking for. You never will.

---

## Procedural Generation

Each floor is built from **room templates** stitched together:

- Rooms have **tags**: corridor / chamber / shrine / crypt / trap
- Tags determine enemy spawns, loot tables, environmental hazards
- Connections are random but guaranteed solvable
- Every 5th floor: **handcrafted boss arena**, no procedural elements

---

## Tone

**Dark but not grimdark.** Your hunters make bad jokes when they're scared. The Prior complains about his knees. The Ashcaller names her fireballs.

But people die. The vampire lords have reasons. Some floors will make you feel like the monster.

---

## Platform & Tech

- **GBA ROM** — plays on real hardware + any emulator
- **Language:** C + devkitARM
- **Toolchain we build:**
  - Map/room template editor (web, outputs JSON)
  - Sprite/tile editor (or adapt existing)
  - ROM builder (Node — takes JSON assets → GBA binary)

---

## Build Order

1. Map editor — paint grid rooms, place tiles, define exits
2. ROM stub — GBA boots, shows a title screen
3. Grid renderer — draw a room, move a cursor
4. Combat prototype — one Warden vs one Thrall, turn-based

Everything else grows from there.

---

## Movement & Combat (Advance Wars style)

**Turn structure:** Select hunter → blue tiles show movement range → pick destination → choose action (attack / special / wait / item)

**Movement ranges:**
| Hunter | Tiles | Reason |
|--------|-------|--------|
| Warden | 3 | Heavy armour, not sprinting anywhere |
| Strider | 5 | Light, always repositioning |
| Prior | 3 | Old man. Knees. |
| Ashcaller | 4 | — |
| Greyborn | 6 (8 on Frenzy) | Half wolf |

**Party split — the core tension:**
Players *choose* whether to stay together or split up. Splitting enables:
- Strider taking high ground around a corner
- Flanking multiple enemies
- Covering two doors simultaneously

But splitting creates danger:
- Nightwings ignore walls — isolated hunters are vulnerable
- Veilborn pass through your party and attack from behind
- Elders target the Prior first. Always. Even across a split.

---

## Torch Mechanic (Fog of War)

Each hunter has a **torch radius** — their personal vision cone.

**Stay together:** Overlapping torches = wide vision, slow bright target, enemies know where you are

**Split up:** Half the dungeon goes dark. You don't know what's in that side room until someone's already in it.

The split party question becomes genuinely agonising:
- Do you send the Strider ahead? She'll have vision. She'll also be alone.
- Do you keep the Prior with the group? Safe. But the Warden's around a blind corner.

Rooms beyond torch range show as **dark tiles** — you know the layout (mapped on entry) but not what's in them.

---

## Sprite System — Layered Composition

Rather than unique sprite sheets per character, all humanoid units are built from **stacked layers** on a shared base skeleton. The walk/idle animation lives on the base — equipment rides on top.

### Layer Stack (back to front)
```
1. Cape / cloak       (behind body)
2. Base body          (naked silhouette, drives all animation)
3. Legs / boots
4. Torso armour
5. Arms / gloves
6. Head / helmet
7. Weapon             (right hand)
8. Shield / off-hand  (left hand)
9. FX overlay         (fire, glow, berserk aura etc.)
```

### Base Bodies (only 2 needed)
- `human_standard` — Warden, Strider, Prior, Ashcaller, Thrall
- `human_large` — Greyborn, Elder

### Example compositions
| Character | Body | Torso | Helm | Weapon | Shield | FX |
|-----------|------|-------|------|--------|--------|----|
| Warden | human_standard | plate_heavy | great_helm | sword | kite_cross | — |
| Strider | human_standard | leather_light | hood | crossbow | — | — |
| Prior | human_standard | robes_worn | — | staff_holy | — | — |
| Ashcaller | human_standard | leather_burnt | — | flask_fire | — | ember_particles |
| Greyborn | human_large | tattered_ranger | wolf_head | claws | — | fur_overlay |
| Thrall | human_standard | clothes_tattered | — | — | — | pale_overlay |

### Animation frames needed (base bodies only)
- Idle: 2 frames
- Walk down / up / left / right: 2 frames each (8 total)
- Attack: 2 frames
- Death: 1 frame
- Hit: 1 frame
**Total: 14 frames × 2 base bodies = 28 frames** (vs 85 unique before)

### Character Creator Tool
Web-based tool that:
1. Lets you pick base + each layer from dropdowns
2. Shows live composited preview
3. Exports JSON definition: `{ base, torso, helm, weapon, shield, palette, fx }`
4. Generates merged sprite sheet ready for GRIT

Future: ship a version of this in-game for hunter customisation. Zero extra art cost.

### Palette swapping
GBA hardware supports palette swapping natively — same sprite, different 15-colour palette = different character. One Thrall sprite, 4 palette variants = 4 visually distinct enemy types for free.

---

## Art Pipeline

1. Find Fire Emblem GBA reference sprites (freely ripped, good community resources)
2. Feed reference image + text prompt to gemini-image — anchors perspective, palette, pixel size
3. Trace result in Aseprite on 32x32 canvas, apply strict 15-colour GBA palette
4. Export sprite sheet → GRIT converts to GBA format

**Key:** Always pass a reference sprite image alongside the prompt. Consistency goes way up vs text-only prompts.

---

## Future: The Vampire Platformer

Companion game, shared universe. You play the vampire. The hunters from Bloodwatch are the enemies.
Working title: **VOURDALAK**

---

*"They say the Order of Bloodwatch hasn't had a full chapter in forty years. Too many doors opened that shouldn't have been."*
