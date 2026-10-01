# VOXEL DEPTHS — POC DEMO - 


Testing a voxel-based isometric roguelite dungeon crawler built with Three.js. You descend through three procedurally generated floating terraces, fight creatures, solve environmental puzzles, and survive the abyss.

---

## 🎮 Core Gameplay Loop

1. **Enter the Depths** from Depth 1's spawn point
2. **Explore rooms**, corridors, and secrets across the terrace
3. **Fight enemies**: Gloom Slimes, Voidbats, Stone Golems, Wraith Wisps
4. **Collect loot**: Gold, Shards, Potions, Power Crystals
5. **Use stair-ports** to descend deeper or ascend back up
6. **Reach the bottom**: Defeat the Abyssal Colossus boss and win!

---

## 🌍 World Structure

The world consists of **three floating terraces**, each procedurally generated:

### Depth 1 — THE SHATTERED TERRACES
> *Gloom slimes & voidbats drift between the broken walks*

- Multiple interconnected rooms connected by corridors
- Lava pits requiring bridge-platforms to cross
- Secret vaults behind teleported gates
- Stair-ports leading downward (and occasionally upward)

### Depth 2 — THE VAULT OF GATE-LIGHT
> *Wraiths guard the secret vaults — bring light*

- More dangerous enemies including wisps
- Additional secret vaults hidden behind paired teleportation gates
- The bridge-platform mechanic returns
- Leads deeper... or back up if needed

### Depth 3 — THE COLLOSSAL HOLLOW
> *Something vast has woken beneath the stone*

- A massive hollow arena for the final boss
- Fewer regular enemies, but the **ABYSSAL COLOSSUS** awaits
- Victory requires reaching and defeating the boss!

---

## 🗺️ World Generation Details

### Procedural Elements (Per Run)
Each game uses a random seed for:
- Room placement and corridors
- Lava pit locations and bridge positions
- Enemy spawning points
- Item and chest locations
- Secret vault positions

**Three floors share the same X/Z coordinates**, stacked vertically with 6 units between each terrace.

### Special Features
- **Stair-ports**: Portal staircases (5 steps + portal disc) connecting floors
- **Teleport gates**: Paired light-gate arches; one leads to a secret vault
- **Lava pits**: Hazardous drops into magma below
- **Moving bridge-platforms**: Cross lava safely by riding the floating plate

---

## 🎯 Player Character

### Attributes (Upgradable)
| Stat | Base Value | Scale |
|------|------------|-------|
| Health | 100 HP | +12 HP per level |
| Damage | 12 Dmg | +1.4 Dmg per level |
| Level cap | Unlimited | Each floor starts at LV 1, but persists across runs |

### Equipment
- **Blade of Dawn**: Your primary weapon with a visible swing arc
- **Cape of Shadows**: Visual flair (no mechanical effect)

### Movement
- **WASD/Arrows**: Move the character
- **Player speed**: ~5.4 units per second
- **Grounded**: You stand on floor tiles, stair heights, or platform surfaces
- **Fall damage**: Falling into lava kills instantly (35 damage)

### Combat Mechanics
| Ability | Input | Cooldown | Description |
|---------|-------|----------|-------------|
| **Strike** | SPACE | Instant | Swing sword in facing direction; hits enemies in front |
| **Dash** | SHIFT or Right-click | 0.85s | Quick teleport in movement/dash direction; grants i-frames |

### Leveling Up
Gain XP from:
- Killing enemies (10–120 XP depending on type)
- Picking up Power Crystals (instant level-up)

**Level-up effects**:
- +12 Health, scaled to max HP
- +1.4 Damage
- Visual burst effect and toast notification

---

## 👹 Enemy Types

### Gloom Slime (HP: 26)
- **Behavior**: Bounces toward the player when within 8 units
- **Attack**: Jumps at you; contact deals 9 damage
- **Spawn rate**: Most common enemy
- **Drop**: Gold, Gems, or Potions

### Voidbat (HP: 22)
- **Behavior**: Flies around erratically; circles the player when close
- **Attack**: Fire magic missiles every ~3s from a distance
- **Spawn rate**: Common in Depths 1–2
- **Drop**: Gold, Gems, or Potions

### Stone Golem (HP: 70)
- **Behavior**: Slow but tanky; charges when nearby
- **Attack**: Melee slam (14 damage) and area-of-effect projectiles at range
- **Spawn rate**: Rare; more dangerous in late-game
- **Drop**: Heavy loot—Gold, Gems, Power Crystals

### Wraith Wisp (HP: 34)
- **Behavior**: Orbits the player from above; always airborne
- **Attack**: Fires orbs that seek you; also spawns floating particles
- **Spawn rate**: Exclusive to secret vaults (Depth 2+)
- **Special drop**: Always drops a Power Crystal on death
- **Note**: Cannot be contacted melee-wise due to flight

### Abyssal Colossus (HP: 260) — Final Boss
- **Behavior**: Massive golem; patrols the boss room
- **Attack 1**: Charges at close range (18 damage); creates shockwave particles
- **Attack 2**: Fires ring of 10 projectiles in all directions (10 each, AoE)
- **Enrage**: At 30% HP, the Colossus rages with increased speed
- **Loot**: Massive drop—multiple Gold, Gems, and Power Crystals
- **Win condition**: Defeat it to end the run

---

## 💎 Items & Loot

### Collectibles (Spawn from Enemies/Chests)

| Type | Value/Effect | Notes |
|------|--------------|-------|
| **Gold Coin** | +5–16 gold | Currency for... aesthetics? |
| **Health Potion** | +30 HP (capped at max) | Found in chests and enemy drops |
| **Gem Shard** | +5 XP | Increases level progress; visual glow effect |
| **Power Crystal** | +2.5 DMG, +8 HP (granted immediately) | Always grants a level-up sound and toast |

### Chests
- **Regular chests**: Found in normal rooms; contain Gold/Gems/Potions
- **Secret vaults**: Behind teleport gates; guaranteed to have a Power Crystal

**Opening a chest**:
1. Walk close (<1.1 units) while alive
2. Lid swings open with animation
3. Loot drops onto the floor (animated particles, glow effects)
4. Toast notification: "CHEST OPENED" or "SECRET VAULT OPENED"

---

## 🔀 Gate Teleportation System

Paired teleport gates connect **normal rooms** to **secret vaults**:

1. **Find a gate archway** in a normal room (blue/teal glowing ring)
2. **Approach the portal disc** at waist height (~1.1 units above floor)
3. **Wait for cooldown** to expire after using a gate
4. **Step into the portal** → visual fade transition
5. **Arrive in secret vault**: New room with wraith wisps + Power Crystal

**Cooldown**: ~1.2s between uses (player variable `gateCd`)

**Visual cues**:
- Gate arches: Dark stone frames with glowing circular portals
- Portal discs pulse slowly; rings rotate
- Minimap marks gate locations with cyan dots

---

## 🪜 Stair-Port System

Stair-ports are your **vertical transit** between depths:

### Downward (Depth N → N+1)
1. Find the stair-port near Depth 2 or 3 spawn areas
2. Approach the green glowing portal at y=5
3. Stand on the platform (~0.15 units high) to enter
4. Portal activates with particle burst + fade
5. You descend to the next depth

### Upward (Depth N → N-1)
1. Return to Depth 1 after exploring deeper floors
2. Use the violet portal near spawn points
3. Teleport back up to retake areas or restart

**Cooldown**: ~2s while near a stair-port (player variable `portalCd`)

---

## 🎨 Visual FX & Atmosphere

### Lighting System
- **Directional sun**: Follows player; casts dynamic shadows
- **Player lantern**: Point light around you (~10 units radius)
- **Torch flames**: Flickering static torches in rooms (6+ have lighting cones)
- **Emissive crystals**: Wall-mounted glowing crystals (teal or amber)
- **Lava glow**: Orange light radiating from magma pits

### Post-Processing
- **Unreal Bloom**: Soft bloom on emissive materials
- **Fog**: Dark volumetric fog that fades out toward camera
- **Vignette**: Radial darkening at screen edges
- **Damage flash**: Red radial overlay when hit

### Particle Effects
| Event | FX Description |
|-------|----------------|
| Enemy death | Color-matched confetti burst; audio SFX |
| Level-up | Golden particle shower (+30 units) |
| Portal use | Blue/green swirl particles at spawn point |
| Player swing | Translucent blue arc appears on strike |
| Damage numbers | Floating text above hit location |

---

## 🎮 Controls Reference

| Key | Action |
|-----|--------|
| **W** / Arrow Up | Move forward (relative to facing) |
| **S** / Arrow Down | Move backward |
| **A** / Arrow Left | Turn left and move |
| **D** / Arrow Right | Turn right and move |
| **SPACE** | Strike with sword |
| **SHIFT** or **Right-click** | Dash (short burst) |
| **ESC** | Pause/Resume game |
| **` (backtick)** | Toggle camera debug panel (**developer tool**) |

### Debug Camera (`F4` or `` ` ``)
When enabled:
- Three sliders control X/Y/Z camera position
- "Follow player" checkbox re-attaches camera to player
- "RESET" returns camera to normal offset mode

---

## 🏆 Game Progression & Stats

### Per-Run Statistics (Displayed on Death/Victory)
| Stat | Description |
|------|-------------|
| **Kills** | Total enemies defeated in this run |
| **Gold** | Gold coins collected |
| **Shards** | Gems found |
| **Secrets** | Vault entries unlocked via gates |
| **Level** | Player's level at death (carried across runs) |
| **Time** | Elapsed time in seconds |

### Victory Conditions
- Defeat the Abyssal Colossus boss in Depth 3
- Game triggers victory screen with full stats
- "Delve Anew" resets to main menu for another run

### Death & Respawn
- Fall into lava: Instant death, respawn at current floor spawn
- HP reaches 0: Screen fades; show death summary
- **Run reset**: Stats carry over (level), but gold/gems/kills reset each time
- "Rise Again" returns to main menu

---

## 🔊 Audio System

Built-in procedural audio with a tiny synthesizer. All sounds are generated at runtime.

### SFX Categories
- **Combat**: Swing, hit, kill, player damage
- **Collectibles**: Coins (high chime), gems (ascending chimes), potions (healing trill)
- **Portal effects**: Gate activation, stair-port transition
- **Boss events**: Roar when enraged
- **Environmental**: Lava bubbling, chest opening

### Music System
- Ambient drone pad with three oscillators (A–C# minor)
- Bass line cycling through notes
- Occasional lead melody on higher frequencies
- Soft noise textures for atmosphere
- Starts automatically when entering a depth
- Can be resumed from pause

---

## 🎨 Visual Art Style

### Material Palette
| Element | Color Notes |
|---------|-------------|
| **Stone walls** | Dark purplish-gray with variation (dark/medium/light tints) |
| **Floor tiles** | Deep blue-black alternating pattern; some mossy or cracked |
| **Lava** | Dark red/orange with glowing emissive surface |
| **Crystal growths** | Teal or amber cubes mounted on walls |
| **Torch flames** | Orange-yellow, flickering scale |

### Character Design
All characters built from voxels (boxes) for stylistic consistency:
- **Player**: Adventurer in blue tunic with gold sword, brown skin, green cape
- **Slime**: Bouncy green blob with eyes on top
- **Bat**: Dark purple bat shape with glowing eyes and wings
- **Golem**: Stone construct with glowing crystal core
- **Wisp**: Glowing violet sphere with rotating ring halo
- **Boss**: Massive golem (2.3x scale) with pulsing magenta core

---

## 🖼️ HUD Elements

### Main HUD (Top-Left)
- **HP bar**: Orange/red gradient; text shows "X / Y"
- **Level bar**: Teal progress bar toward next level
- **Gold counter**: Yellow coin icon + number
- **Gem counter**: Teal gem icon + number
- **Kills counter**: Skull icon + number
- **Depth indicator**: Current depth / 3

### Boss Health Bar (Top-Center)
- Appears only when boss is alive
- Pink/gradient bar with "ABYSSAL COLOSSUS" label

### Minimap (Top-Right)
- Shows explored area with:
  - Dark purple = unexplored walls
  - Cyan dots = teleport gates
  - Green dot = downward portal, red = upward
  - Red circles = enemies (boss is larger)
  - Yellow/green/red dots = loot types
  - White dot + line = player position and facing

### Toast Notifications (Center-top)
- Appear when leveling up, opening vaults, enrage events
- Fade out after ~2.6 seconds

---

## 🧩 Technical Notes

### Performance Optimizations
- **Instanced meshes**: Static world geometry uses single mesh with per-instance transforms
- **Object pooling**: Particles and damage numbers reuse objects from a pool
- **Dynamic objects**: Only current floor updates every frame (others hidden)
- **Shadow cascades**: Soft shadows for realistic lighting

### Three.js Features Used
- Perspective camera with isometric offset
- Hemisphere light + directional sun for ambient+shadow lighting
- Fog for depth atmospheric effect
- UnrealBloomPass for glowing effects
- Point/Directional/Hemisphere lights combined

---

## 🎯 Design Philosophy Summary

> *"Three floating terraces drift in the void — each one grown fresh by the old machines."*

VOXELDEPTHS emphasizes:
1. **Verticality**: Use stair-ports to explore depths and secrets
2. **Environmental interaction**: Cross lava, ride platforms, solve gate puzzles
3. **Procedural discovery**: Every run has new room layouts, enemy placements, and loot
4. **Voxel aesthetic**: All geometry is boxes; clean, minimal art style
5. **Atmospheric roguelite**: Short runs, permadeath, persistent level progression
6. **Audio atmosphere**: Procedural synth music creates immersion

---

## 📜 Legend

- `[]` = brackets for inline code
- `/` = forward slash (division)
- `\n` = newline character (in strings)
- `Math.floor()`, `Math.sin()`, etc. = JavaScript built-in functions
- `THREE.` prefix = Three.js module namespace

---

*VOXELDEPTHS — A voxel isometric roguelite*

**Built with love for the art of exploration, survival, and discovery.**
P.H.C 2026
