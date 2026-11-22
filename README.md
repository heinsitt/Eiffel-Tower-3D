<div align="center">

# LA TOUR DE 300 MÈTRES (1887–1889)

### Interactive 3D Archival Exhibition & Physically Grounded Construction Reconstruction

**Technical Specification, Architecture Reference & Exhibition Guide**

---

| Document Property | Specification |
| :--- | :--- |
| **Application Name** | `eiffel-tower-3d-experience` — *La Tour de 300 Mètres (1887–1889 Archival Exhibition)* |
| **Version** | `1.0.0` |
| **Rendering Engine** | Three.js (ES Modules, WebGL2 / WebGL, ACES Filmic Tone Mapping) |
| **Build Toolchain** | Vite `^8.3.0` · TypeScript `^7.0.2` · Node.js / Bun |
| **Spatial Scale** | `1 unit = 1 meter` (`-11.55 m` pneumatic caissons to `+312.275 m` flagpole tip; `125 m` base) |
| **Exhibition Languages** | Bilingual — English (`en`) & Français (`fr`) |
| **UI Design System** | Pure Achromatic Museum Exhibition (`chroma = 0`) + On-Demand Engineering Debug Drawer |

</div>

---

## Table of Contents

1. [Executive Overview](#1-executive-overview)
2. [Key Experience & Engineering Highlights](#2-key-experience--engineering-highlights)
3. [Chronological Exhibition Stops & Archival Calibration](#3-chronological-exhibition-stops--archival-calibration)
4. [System Architecture & Repository Structure](#4-system-architecture--repository-structure)
5. [Procedural Geometry & Construction Kinematics](#5-procedural-geometry--construction-kinematics)
6. [Optical PnP Portals & Camera Choreography](#6-optical-pnp-portals--camera-choreography)
7. [Lighting, Materials & 1889 Paris Maquette](#7-lighting-materials--1889-paris-maquette)
8. [Performance & Early-Boot Worker Pipeline](#8-performance--early-boot-worker-pipeline)
9. [Installation, Scripts & Runtime Configuration](#9-installation-scripts--runtime-configuration)
10. [Archival Sources & Public-Domain Attributions](#10-archival-sources--public-domain-attributions)

---

## 1. Executive Overview

**La Tour de 300 Mètres (1887–1889)** is an interactive, real-time 3D web exhibition reconstructing the design, twenty-six-month assembly, and 1889 *Exposition Universelle* inauguration of Gustave Eiffel’s iron tower in Paris.

Rather than presenting a static landmark mesh, the application pairs a **five-zone procedural 3D ironwork model** (`159` discrete structural and temporary falsework pieces; `3,176,054` triangles at completion) with **eleven perspective-calibrated historical photographs and elevation plates** (`1884–1889`) sourced from public-domain museum collections. Every construction stage is driven by physically grounded 19th-century erection kinematics—from compressed-air pneumatic caissons and `800`-ton hydraulic alignment jacks to *boîtes à sable* leveling cylinders, climbing creeper cranes, and top-down timber falsework dismantling.

> **Core Architectural Guarantee:** The scene strictly enforces physical PBR lighting (`MeshStandardMaterial` / `MeshPhysicalMaterial`, zero emissive surfaces, zero additive blending), calibrated sub-`5.3 px` PnP camera registration, and a pure achromatic museum UI with zero engineering clutter in visitor mode.

---

## 2. Key Experience & Engineering Highlights

| Pillar | Capability Description |
| :--- | :--- |
| **12-Stop Chronological Loop** | Guides visitors from Gustave Eiffel’s 1:1,000 elevation drawing (`Stop 1`), through ten historical construction and 1889 Exposition glass plates (`Stops 2–11`), to an interactive 3D showcase (`Stop 12`) with five craftsmanship camera presets. |
| **Zero-Jump Optical Portals** | Aligns the 3D WebGL camera directly to solved 3D pinhole + rising-front `viewOffset` frustums (`0.000 px` onset/lock continuity), allowing seamless cross-dissolves and 50/50 wipe comparisons between 1889 photography and 3D geometry. |
| **Drag-to-Orbit 3D Release** | Dragging more than `6 px` on any locked archival plate smoothly dissolves the historical overlay into an unobstructed, obstacle-constrained 3D orbit without focal jumps. |
| **Physically Grounded Kinematics** | Implements nine authentic 1887–1889 mechanical assembly behaviors with damped crane pendulum sway, live 3D hoist cables, progressive bay erection, and `0.000 m` origin/floor plunge invariants. |
| **Monochrome Paris 1889 Context** | Reconstructs the Champ-de-Mars, Palais du Trocadéro, Pont d’Iéna, Seine, and Haussmannian surroundings as a collodion photograph & pen-and-ink architectural maquette with distance mist. |
| **Bilingual Exhibition Copy** | Complete museum placards, titles, elevation summaries, and UI chrome authored in both English (`en`) and French (`fr`), switchable live via header controls or URL query parameters. |

---

## 3. Chronological Exhibition Stops & Archival Calibration

The exhibition follows a continuous timeline mapped to construction progress **`step ∈ [0.00, 11.00]`**, anchoring each archival plate to its exact solved camera pose and structural state in `data/catalog/photo_cameras_3d.json`.

| Stop | ID / Plate | Historical Date | Title (EN) | Calibrated Step | PnP RMSE | Structural Milestone |
| :---: | :---: | :--- | :--- | :---: | :---: | :--- |
| **01** | `#55` | `1884–1889` | Designing the 300-Meter Tower | `11.00` | `0.90 px` | Eiffel’s 1:1,000 architectural elevation (`125 m` base, `312 m` tip) |
| **02** | `#13` | `21 April 1887` | Compressed-Air Foundation Caissons | `0.55` | `1.37 px` | West Pier excavation & pneumatic caissons `5 m` below the Seine bed |
| **03** | `#09` | `18 July 1887` | Base Girders and Cast-Iron Shoes | `1.50` | `1.39 px` | Cast-iron sabots, `800`-ton jack recesses, and inclined starter struts (`12 m`) |
| **04** | `#51` | `14 January 1888` | Central Timber Scaffolds at the First Platform | `3.55` | `1.90 px` | `48 m` bolted central timber falsework towers & *boîtes à sable* cylinders |
| **05** | `#23` | `26 March 1888` | Joining the First-Floor Girders Across the Four Piers | `3.60` | `1.14 px` | `58 m` horizontal first-floor belt uniting all four piers (`-75.4%` rising-front shift) |
| **06** | `#14` | `15 May 1888` | Second-Stage Piers and Climbing Creeper Cranes | `4.85` | `1.91 px` | Creeper cranes ascending elevator rails to `89 m` prior to decorative arches |
| **07** | `#01` | `21 August 1888` | Completion of the Second Platform and Lower Arches | `5.66` | `3.68 px` | `116 m` 2nd platform & `127 m` panel complete; `74 m` arches closed; scaffolds cleared |
| **08** | `#46` | `23 November 1888` | Upper Shaft Above the Intermediate Platform | `6.52` | `5.21 px` | Merged column passing `196 m` transfer deck to `216 m`; 3 central arcade bays |
| **09** | `#18` | `May 1889` | Completed Tower from the Champ-de-Mars | `11.00` | `2.85 px` | Inaugurated 31 Mar / opened 15 May 1889; 9-bay gallery, `300 m` beacon, `312 m` mast |
| **10** | `#36` | `Summer 1889` | Lower Arch and Central Dome View | `11.00` | `1.20 px` | `74 m` arch intrados (`39 m` crown), 72-scientist frieze, Saint-Vidal & Central Dome |
| **11** | `#38` | `Summer 1889` | Fountain Basin and Pier Entrances | `11.00` | `1.88 px` | Château-Landon limestone plinths, ticket kiosks, and Saint-Vidal sculptural basin |
| **12** | `3D` | `1889 in 3D` | Complete 1889 Tower in 3D | `11.00` | `—` | Unobstructed 3D hero showcase with 5 preset angles (`0`, `I`, `II`, `III`, `IV`) |

### Stop 12 Hero Showcase Presets

1. **`0` — Full 300 m Elevation:** Three-quarter overview from `[240, 52, 440]` targeting `[0, 134, 0]` (`FOV 42°`, slow turntable enabled).
2. **`I` — Plinths and Cast-Iron Sabots:** Close inspection of Château-Landon stone plinths and `54.6°` cast-iron shoes (`FOV 36°`).
3. **`II` — 74 m Arch and First Platform:** Focus on the decorative lattice arch, gilded frieze of 72 French scientists, and `58 m` glazed gallery (`FOV 36°`).
4. **`III` — Second Platform at 116 m:** Convergence of the four inclined piers, observation deck, *Le Figaro* kiosk, and Édoux lift transfer (`FOV 38°`).
5. **`IV` — Summit Campanile at 300 m:** `276 m` third platform, Eiffel’s private study, campanile arches, Sautter-Lemonnier *Phare électrique*, and Tricolore (`FOV 35°`).

---

## 4. System Architecture & Repository Structure

### Runtime Boot & Render Flow

```text
index.html
  └─► startup_orchestrator.js (t ~ 0 ms, zero heavy deps)
        ├── Worker A1 (pbr_worker.js: floor_a) ──┐
        ├── Worker A2 (pbr_worker.js: floor_b) ──┼─► Zero-copy Transferable PBR Buffers
        ├── Worker B  (pbr_worker.js: tower)   ──┘
        ├── Fetch: data/catalog/photo_cameras_3d.json
        └── Preload & decode: Stop 01 Elevation Plate (#55)
  └─► app.js (Main Exhibition Runtime)
        ├── WebGLRenderer (ACESFilmicToneMapping, sRGB, PCFSoftShadowMap)
        ├── world/atmosphere.js (Real Physical Lights + Mood PMREM + Sky Equirect)
        ├── world/paris_1889.js (Monochrome Pen-and-Ink Paris + Distance Mist)
        ├── model/tower_controller.js (5 Zones + BuildKinematicsEngine)
        ├── perf/fps_and_drawcalls/ (Batching, Shader Unification, Matrix Freeze)
        ├── portals/photo_portals.js + camera/ (PnP Alignment & Helical Flights)
        └── ui/ + story_and_archive/ (Landing Intro, Header, Walkthrough, Placard, Debug)
```

### Directory Layout

```text
├── index.html                        # Entry shell, importmap, modulepreloads, boot veil
├── style.css                         # Exhibition layout & typography stylesheet
├── app.js                            # Main WebGL runtime, simulation loop & __MUSEUM_APP__ API
├── startup_orchestrator.js           # Parallel early-boot worker, JSON & image decode pipeline
├── pbr_worker.js                     # Off-thread procedural PBR texture synthesizer
├── vite.config.ts                    # Vite MPA config, importmap hoisting & static asset bundling
├── data/
│   └── catalog/photo_cameras_3d.json # 11 solved PnP cameras, viewOffsets & landmark RMSE data
├── model/
│   ├── tower_controller.js           # Zone loader, step sync, ambient summit motion & metrics
│   ├── build_kinematics.js           # 9-family 19th-century construction & rigging kinematics
│   ├── materials.js                  # Procedural puddled-iron, bronze, glass & timber PBR banks
│   ├── floor_textures.js             # Oak decks, stone parvis, gravel & Seine surface PBR maps
│   ├── walkthrough_pacing.js         # Step dwell & easing curves for automated playback
│   └── modules/                      # Procedural 3D structural zone builders (Zones 1–5)
├── camera/
│   ├── index.js                      # Camera subsystem barrel exports
│   ├── optical_camera_rig.js         # 0.000-px PnP projection, rising-front & Hermite blending
│   ├── walkthrough_camera_choreography.js # Helical 4-pier obstacle-avoiding flight paths
│   ├── hero_finale_rig.js            # Stop 12 responsive framing & turntable sway
│   └── viewport_constraints.js       # 6-layer tower-focused orbit & ground-clearance clamps
├── portals/
│   └── photo_portals.js              # Spatial glass-plate overlays, wipe curtain & LRU cache
├── world/
│   ├── atmosphere.js                 # Day/Dusk/Night physical lighting, PMREM & emissive audit
│   ├── paris_1889.js                 # Surrounding 1889 Paris orchestrator & mesh merging
│   ├── distance_fog.js               # Hybrid world-radial + camera-range mist & ink fade
│   ├── sketch_materials.js           # 45° copperplate cross-hatching & botanical tree builders
│   ├── w_parvis_seine.js             # Parvis, caisson pits, Seine river & Pont d'Iéna
│   ├── w_trocadero.js                # Chaillot slope & Palais du Trocadéro
│   └── w_champ_de_mars.js            # Exposition palaces, Fontaine Coutan, Dôme Central & city
├── perf/fps_and_drawcalls/
│   ├── index.js                      # Stage 4 FPS & draw-call optimization entry point
│   ├── mesh_batch_optimizer.js       # Intra-piece & settled-zone mesh batching
│   ├── kinematics_and_shader_optimizer.js # Shader unification, non-enumerable userData & prewarm
│   └── shadow_and_matrix_optimizer.js # Subterranean/distance shadow culling & static matrix freeze
├── story/
│   ├── chapters_1889.js              # French curated archival entries & timeline constants
│   └── chapters_1889_en.js           # English curated archival entries
├── story_and_archive/
│   ├── index.js                      # Story & archive barrel exports
│   ├── walkthrough_data.js           # 12-stop bilingual narrative data & 5 Hero Finale presets
│   ├── museum_placard.js             # Negative-space-aware museum story card
│   └── story_archive.css             # Story placard & archive styling
├── ui/
│   ├── theme_and_debug_index.js      # UI mount orchestrator & legacy chrome quarantine
│   ├── palette_tokens.js             # Pure achromatic (chroma=0) tokens & DOM compliance auditors
│   ├── landing_intro.js              # Post-load full-3D landing veil ('Explore' / 'Explorer')
│   ├── museum_header.js              # Top-right EN/FR toggle & Pure View mode
│   ├── walkthrough_controller.js     # Unified 12-stop playback bar & virtual deterministic clock
│   └── debug_drawer.js               # 5-panel engineering inspector (?debug=1 / Ctrl+Shift+D)
├── references/                       # High-resolution public-domain plates & construction photos
├── thumbnails/                       # 320 px fast-boot archival preview thumbnails
└── vendor/three/                     # Vendored Three.js core & OrbitControls ES modules
```

---

## 5. Procedural Geometry & Construction Kinematics

### Structural Zones (`model/modules/`)

The Tower is synthesized entirely in code across five modular zones totaling **159 tracked pieces** (`148` permanent pieces and `11` temporary scaffolds, derricks, and creeper cranes):

| Zone Slot | Module | Elevation Range (Y) | Primary Architectural & Engineering Components |
| :---: | :--- | :---: | :--- |
| **`zone1`** | `zone1_foundations_piers1.js` | `-11.55 m` to `+57.63 m` | Pneumatic caissons, Château-Landon masonry massifs, `54.6°` cast-iron *sabots*, starter *amorces*, and lower inclined pier latticework |
| **`zone2`** | `zone2_arches_floor1.js` | `+1.95 m` to `+69.20 m` | First-floor box girders (`58 m`), consoles, `74 m` decorative Sauvestre arches (`39 m` crown), frieze of 72 French scientists, 9-bay *Galerie Vitrée*, and 4 restaurant pavilions |
| **`zone3`** | `zone3_middle_floor2_merger.js` | `+57.20 m` to `+197.02 m` | Stage-2 inclined piers, `116 m` second platform & *Le Figaro* kiosk, pier convergence merger (`z3_merger_intermediaire.js`), and `196 m` Édoux intermediate transfer deck |
| **`zone4`** | `zone4_upper_shaft_summit.js` | `+178.98 m` to `+312.275 m` | Upper vertical shaft panels, `276 m` third platform, Gustave Eiffel’s study, open campanile, `300 m` Sautter-Lemonnier Fresnel lighthouse, Robinson anemometer, and Tricolore mast |
| **`zone5`** | `zone5_elevators_cranes_falsework.js` | `-11.55 m` to `+295.00 m` | Roux-Combaluzier, Otis, and Édoux hydraulic lift carriages, `48 m` bolted timber falsework towers, guyed gin-pole derricks, and rail-mounted climbing creeper cranes |

### 9 Mechanical Assembly Families (`model/build_kinematics.js`)

| Mechanism Family | Historical Engineering Behavior |
| :--- | :--- |
| `caisson_pneumatic_sink` | Sheet-iron foundation caissons sinking smoothly into excavation pits under compressed air |
| `masonry_course_stack` | Stepped hydraulic-concrete and dressed Château-Landon ashlar masonry courses |
| `inclined_shoe_hydraulic_seat` | Cast-steel counter-shoes seated along `54.6°` batter with `800`-ton hydraulic jack alignment |
| `battered_pier_derrick_hoist` | Pier trusses swung in on guyed *bigues* with damped pendulum oscillation and gusset seating |
| `platform_girder_sandbox_settle` | `58 m` & `116 m` box girders hoisted onto falsework *boîtes à sable*, then bleeding dry sand to lock |
| `arch_cantilever_swing` / `crown_key` | `74 m` arches cantilevering from pier haunches over timber *cintres* until crane-lowered crown key closure |
| `shaft_creeper_crane_hoist` | Upper shaft panels hoisted by internal climbing cranes with live boom slewing and 3D steel cables |
| `progressive_bay_erection` | 1889 glazed galleries, pavilions, staircases, and hydraulic machinery erected progressively upward |
| `falsework_staged_dismantle` | Pine scaffolds built bottom-up tier-by-tier and dismantled top-down as lowered timber bundles |

---

## 6. Optical PnP Portals & Camera Choreography

- **PnP Projection Fidelity (`camera/optical_camera_rig.js`):** Each historical plate in `data/catalog/photo_cameras_3d.json` specifies a world-space camera `position_xyz_m`, `quaternion_xyzw`, vertical `fov_y_deg`, and a Three.js `setViewOffset` rectangle supporting extreme 19th-century rising-front architectural bellows shifts (up to `-75.4%` vertical shift on Photo `#23`).
- **Helical Obstacle-Avoiding Flight (`camera/walkthrough_camera_choreography.js`):** Transitions between stops compute exterior `CatmullRomCurve3` trajectories that clear all four inclined piers and the central shaft while keeping the camera’s focal gaze anchored toward the Tower—preventing disorienting sky-away rotations during `180°` cross-axis flights.
- **Mid-Flight Interruption & Drag Release:** User clicks or scrubs mid-flight blend smoothly via `C¹` Hermite velocity-matched curves. Intentional canvas drags (`> 6 px`) release the locked glass plate via a frozen-aspect cross-dissolve into constrained 3D orbit (`camera/viewport_constraints.js`).

---

## 7. Lighting, Materials & 1889 Paris Maquette

- **100% Real Physical Light Rig (`world/atmosphere.js`):**
  - **Three Archival Moods:** `day` (luminous collodion glass-plate daylight), `dusk` (warm albumen twilight), and `night` (silver-gelatin nocturnal illumination).
  - **Light Sources:** Key `DirectionalLight` with `PCFSoftShadowMap` (`4096×4096` GPU / `2048×2048` software WebGL), collodion/limestone `HemisphereLight`, counter-rim & fill `DirectionalLight`s, four arch/parvis gasolier `PointLight`s, four 1st-floor lantern `PointLight`s, two 2nd-floor `PointLight`s, and the sweeping summit *Phare électrique* `SpotLight`.
  - **Sky & Reflections:** Tone-mapped `EquirectangularReflectionMapping` `DataTexture` sky synced `1:1` to `scene.fog` and updated via `PMREMGenerator` into `scene.environment`.
  - **Zero-Emissive Invariant:** Verified via `auditZeroEmissive(scene)`—all materials maintain `emissive = 0x000000`, `emissiveIntensity = 0.0`, and `emissiveMap = null`.
- **Monochrome 1889 Paris World (`world/paris_1889.js`):**
  - Renders the Seine, Pont d’Iéna, Trocadéro, Galerie des Machines, Dôme Central, Fontaine Coutan, and Haussmann blocks using `45°` shadow-gated copperplate cross-hatching, `EdgesGeometry` pen-and-ink outlines, multi-branched botanical etching trees, and hybrid radial + camera-range mist (`165..960 m` world radial; `430..1340 m` camera range).

---

## 8. Performance & Early-Boot Worker Pipeline

| Optimization Layer | Mechanism & Impact |
| :--- | :--- |
| **3-Worker Parallel Boot** | `startup_orchestrator.js` spawns three module Web Workers (`floor_a`, `floor_b`, `tower_and_world`) at `t ~ 0 ms` to synthesize procedural PBR albedo/roughness/normal buffers off-thread and transfer them zero-copy via `Transferable` `ArrayBuffer`s. |
| **Non-Enumerable PBR Bank** | `makeHeavyMaterialUserDataNonEnumerable()` prevents Three.js `Material.clone()` from `JSON.stringify`-serializing large procedural texture metadata during runtime clipping clones. |
| **Shader Unification & Prewarm** | `unifySceneShaderPrograms()` consolidates ~60 WebGL programs down to ~16; an offscreen `4×4` 1-triangle prewarmer compiles shaders in idle time post-Frame 1 to prevent mid-flight stutters. |
| **Mesh & Zone Batching** | `optimizeTowerMeshesAndZones()` and `optimizeParisWorldMeshes()` merge intra-piece static meshes and build Level-2 settled-zone batches (`zone1..zone5`). |
| **Shadow & Matrix Policies** | Culls deep subterranean caisson shadow casters (`y <= -6.6 m`) and distant Paris casters (`> 195 m`), freezes static world matrices, and refreshes shadow maps only when `step` deltas exceed thresholds. |
| **Software WebGL Adaptation** | Automatically detects SwiftShader / llvmpipe via `WEBGL_debug_renderer_info`, caps pixel ratio to `1.0`, lowers shadow map resolution to `2048`, and throttles idle ambient frames. |

---

## 9. Installation, Scripts & Runtime Configuration

### Prerequisites & Local Setup

- **Node.js** `>= 20` (or **Bun** `>= 1.1`)
- Modern WebGL2-capable browser

```bash
# Install dependencies
npm install

# Start the development server on http://localhost:3000
npm run dev
```

### NPM Scripts (`package.json`)

| Command | Action |
| :--- | :--- |
| `npm run dev` | Launches Vite development server on port `3000` (`--host=0.0.0.0`) |
| `npm run build` | Builds production bundle into `dist/` and copies `data/`, `references/`, and `thumbnails/` |
| `npm run preview` | Serves the production `dist/` build locally on port `3000` |
| `npm run lint` | Runs TypeScript static checks (`tsc --noEmit`) |

### URL Query Parameters

Deep-link directly into specific stops, languages, or engineering modes:

| Parameter | Values | Behavior |
| :--- | :--- | :--- |
| `?lang=` | `en` \| `fr` | Sets initial exhibition language (default: `en`) |
| `?intro=` | `0` \| `false` | Bypasses the full-3D landing veil and opens directly into the walkthrough |
| `?stop=` | `1`–`12` or `stopId` | Boots directly at the specified chronological stop |
| `?photo=` | `55`, `13`, `09`, … | Boots locked onto a specific archival plate ID |
| `?autoplay=` | `1` \| `true` | Begins automated walkthrough playback immediately after boot |
| `?debug=` | `1` \| `true` | Mounts the 5-panel Engineering Debug Drawer on the right viewport edge |
| `?deterministic=` | `1` \| `true` | Skips landing mode for scripted frame capture |

### Keyboard Controls

| Key / Shortcut | Context | Action |
| :--- | :--- | :--- |
| `Enter` / `Space` | Landing Screen | Dismisses landing veil and enters Stop 01 |
| `Space` | Exhibition Walkthrough | Play / Pause chronological progression |
| `ArrowRight` / `ArrowLeft` | Exhibition Walkthrough | Step forward / backward (pauses playback) |
| `V` | Active Plate Stop (`01–11`) | Toggle between Archival Glass Plate and unobstructed 3D Reveal |
| `Home` / `End` | Focused Timeline Rail | Jump to Stop 01 or Stop 12 |
| `Ctrl+Shift+D` / `Cmd+Shift+D` | Global | Mount / unmount the Engineering Debug Drawer |

### Programmatic Runtime API (`window.__MUSEUM_APP__`)

For headless verification, camera inspection, and scripted tours, `app.js` exposes `window.__MUSEUM_APP__`:

- **Timeline & Stops:** `setStep(0..11)`, `getStep()`, `goToStop(indexOrId, opts)`, `startExplore()`, `enterLandingMode()`
- **Portals & Blend:** `enterPortal(photoId, opts)`, `exitPortal(opts)`, `setBlend(0..1)`, `setWipe(0..1)`, `verifyAllPortalsProjection()`
- **Atmosphere & Render Modes:** `setMood('day' | 'dusk' | 'night')`, `setRenderMode('pbr' | 'ink' | 'xray' | 'silhouette')`, `setLanguage('en' | 'fr')`
- **Audits & Telemetry:** `getMetrics()`, `auditZeroEmissive()`, `auditWorldSketchAndFog()`, `auditVisitorCompliance()`, `getKinematicsTelemetry()`

---

## 10. Archival Sources & Public-Domain Attributions

All archival plates and construction photographs included under `references/` and `thumbnails/` are public-domain (`PD` / `CC0`) holdings:

| Plate ID | Photographer / Author | Date | Archive / Collection | License |
| :---: | :--- | :---: | :--- | :---: |
| `#55` | Gustave Eiffel (*Planche I*, 1900 monograph) | `1884–1889` | Wikimedia Commons | Public Domain |
| `#13` | Louis-Émile Durandelle | `21 April 1887` | J. Paul Getty Museum (`106P1J`) | `CC0` |
| `#09` | Louis-Émile Durandelle | `18 July 1887` | J. Paul Getty Museum (`106P1T`) | `CC0` |
| `#51` | Louis-Émile Durandelle | `14 January 1888` | Bibliothèque nationale de France (Gallica) | Public Domain |
| `#23` | Louis-Émile Durandelle | `26 March 1888` | J. Paul Getty Museum (`106P1A`) | `CC0` |
| `#14` | Louis-Émile Durandelle | `15 May 1888` | J. Paul Getty Museum (`106P1P`) | `CC0` |
| `#01` | Louis-Émile Durandelle | `21 August 1888` | Bibliothèque nationale de France (Gallica) | Public Domain |
| `#46` | Louis-Émile Durandelle | `23 November 1888` | National Gallery of Art Commons | Public Domain |
| `#18` | Neurdein Frères | `May 1889` | Museum of Photographic Arts (MoPA) Commons | Public Domain |
| `#36` | Unknown (1889 Exposition) | `Summer 1889` | Library of Congress (`LCCN 92519644`) | Public Domain |
| `#38` | Unknown (Fontaine Saint-Vidal) | `Summer 1889` | Rijksmuseum (`RP-F-F16649`) | `CC0` |
