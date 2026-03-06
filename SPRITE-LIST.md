# BLOODWATCH — Sprite List

## Party Units (5 characters × ~12 frames each)

Each character needs:
- Idle (2 frames)
- Walk down / up / left / right (2 frames each = 8 frames)
- Attack (2 frames)
- Death (1 frame)
- Hit flash (1 frame)

| Character | Status |
|-----------|--------|
| Warden | 🎨 reference generated |
| Strider | ⬜ todo |
| Prior | ⬜ todo |
| Ashcaller | ⬜ todo |
| Greyborn | ⬜ todo |

---

## Enemy Units (5 types × ~8 frames each)

Each enemy needs:
- Idle (2 frames)
- Walk (2 frames)
- Attack (2 frames)
- Death (1 frame)
- Hit (1 frame)

| Enemy | Status |
|-------|--------|
| Thrall | ⬜ todo |
| Nightwing | ⬜ todo |
| Veilborn | ⬜ todo |
| Elder | ⬜ todo |
| Vampire Lord (boss) | ⬜ todo |

---

## UI / Interface

| Element | Notes | Status |
|---------|-------|--------|
| Cursor / selector tile | Flashing square, Advance Wars style | ⬜ |
| Movement range tile overlay | Blue tint | ⬜ |
| Attack range tile overlay | Red tint | ⬜ |
| HP bar | Small, per unit | ⬜ |
| Action menu | Attack / Special / Wait / Item | ⬜ |
| Torch radius overlay | Darkness vignette | ⬜ |

---

## Tileset (dungeon environment)

| Tile | Status |
|------|--------|
| Stone floor | ⬜ |
| Stone wall | ⬜ |
| Door (closed / open) | ⬜ |
| Stairs down | ⬜ |
| Torch on wall | ⬜ |
| Altar / shrine | ⬜ |
| Coffin | ⬜ |
| Chest | ⬜ |
| Pit / trap | ⬜ |
| Rubble / decoration | ⬜ |

---

## Town Screen

| Element | Status |
|---------|--------|
| Inn / rest area | ⬜ |
| Shop | ⬜ |
| Recruitment board | ⬜ |
| Town background | ⬜ |

---

## Title Screen

| Element | Status |
|---------|--------|
| BLOODWATCH logo / gothic font | ✅ generated (concept art) |
| Background castle art | ⬜ |
| Menu cursor | ⬜ |

---

## Total scope estimate (with layered system)

**Base bodies:** 2 × 14 frames = 28 frames (was 85 unique)
**Equipment parts:** ~25 individual pieces (most are ~10×10px, trivial)
**Enemy overlays:** ~5 FX layers
**UI elements:** ~10
**Tileset:** ~10 tiles
**Town:** ~5 elements

**Total: ~80 pieces** — but most are tiny modular parts, not full animation sets.

**With the reference pipeline + layered system:** the art problem is almost solved. Base body animation is the only hard part, and you only do it twice.

**Build order:**
1. `human_standard` base body (14 frames) — unlocks all 4 human characters + Thrall
2. Equipment pieces for Warden (5 parts) — first playable character done
3. Tileset (10 tiles) — dungeon is renderable
4. Everything else at leisure
