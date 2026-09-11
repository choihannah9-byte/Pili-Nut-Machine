# Pili Cracker — Interactive Design Models

Interactive 3D models for the TUP BSME capstone: an integrated grading and
dual counter-rotating roller pili nut shell cracker.

Included models (switch with the bar at the top of the app):
- **Rev C Assembly (current)** — full machine with buffer-bin architecture,
  single-lane queue feed, containment, and flow-path overlay.
- **Nip Simulator** — set the gap and nut grade, feed nuts, watch outcomes
  (whole kernel / crushed / uncracked).
- **Rev B Assembly (superseded)** — kept for design-history comparison.

> These models are illustrative concept models built to Rev B/C proportions.
> They are NOT fabrication references — the dimensioned PNC CAD drawing set is.

## Run it — three options

### Option 1 — StackBlitz (no install, in the browser)
1. Put this folder in a GitHub repo (see Option 2, steps 1–3).
2. Open: `https://stackblitz.com/github/YOUR_USERNAME/YOUR_REPO`
3. It installs and runs automatically. Edits are live.

(Alternative without GitHub: go to https://vite.new/react, then drag-drop the
files from this folder into the StackBlitz file panel, replacing the starter's
`src/` and `package.json`, and let it reinstall.)

### Option 2 — GitHub (storage + versioning)
1. Create a new repository at github.com (private is fine).
2. "Add file → Upload files" and drag the entire contents of this folder
   (keep the `src/` structure).
3. Commit. The repo now archives the source; open it in StackBlitz per
   Option 1, or clone it locally per Option 3.

CodeSandbox works the same way: `https://codesandbox.io/s/github/YOUR_USERNAME/YOUR_REPO`

### Option 3 — Run locally
Requires Node.js 18+ (nodejs.org).
```
npm install
npm run dev
```
Open the printed local URL (usually http://localhost:5173).

## Notes
- `three` is pinned to **0.128.0** to match the API version the models were
  written against. Upgrading to newer three.js will mostly work (only core
  APIs are used) but re-test the serrated-roller extrusions and tube
  geometry if you bump it.
- Each component is self-contained (scene, controls, UI in one file), so you
  can copy any single `.jsx` into another React project along with the
  `three` dependency.
- A zero-setup distribution copy also exists:
  `Pili_Assembly_Model_RevC_Standalone.html` (single file, opens in any
  browser) — use that for demos; use THIS project for editing.


---

## Rev C1 (2026-08) — panel-review corrections

- **Sync chain geometry fixed.** Rev C drew the serpentine crossing run through
  both roller shafts (11.1 mm from each shaft centre vs a 15 mm shaft radius).
  C1 computes the exact internal tangent between the 16T pitch circles
  (sin psi = 2r/d): the crossing now clears both shafts by 17.6 mm at every gap,
  and the repositioned idlers hold >= 90 deg wrap across the full 160-177 mm
  centre-distance range. Verification plot: docs/chain-geometry-qa.png.
- **Nip simulator reconciled.** The recommended gap window is now DERIVED live
  from the thesis's two gap inequalities (gap >= max in-grade kernel + 0.5;
  gap <= min in-grade width - 1.1 mm fracture deformation per Gallegos 2013),
  each fed nut is sampled from its grade's size range, and outcomes use the
  same inequalities - the sim can no longer contradict its own gap table.
  60,000-trial Monte Carlo: zero failures inside the derived windows.
- **Model accuracy:** toothed 16T sprockets (pitch dia 65.1), M24 screws drawn to
  size with correct nuts/collars, side plates widened so the carriage slot and
  screw bosses land on plate material, motor/pedestal interference removed,
  grade bins raised clear of the funnel rim, feed channel re-aimed at the nip,
  guard edge frames, pillow-block bolts, motor terminal box.
- **docs/WHITEBOARD-TASK-0-torque.md** - the torque/draw-in re-derivation brief,
  now the top item of the whiteboard list. Note: the briefing's draw-in figures
  (alpha ~ 23 deg, mu >= 0.42-0.43) were CONFIRMED for the worst in-grade case;
  the peak-torque figure (~58 N.m) remains unreproduced and must be re-derived
  from the rig's force-deformation curve.

## Rev C2 (2026-08) — user-review pass

- Tap-to-identify now sees through the translucent guards (sprockets behind the
  chain guard are reachable) and works on all grouped parts.
- Grade bins rebuilt as open-top buffer bins with guide rails, lifting slide
  gates and a NOTCHED interlock bar: tap a bin (or API.setGrade) and the bar
  slides its single cutout to that gate - only the selected gate lifts.
- Handwheel is interactive: drag up/down to change the roller gap; the
  adjustable roller/carriage/collar/sprocket assembly translates together, the
  fixed roller stays put, the chain re-tensions (quantized 0.4 mm rebuilds).
- Hopper on a bolted 4-leg stand + support collar (no longer floating).
- Decks: discharge-end walls with a central opening; enclosed U-channel
  transition troughs replace the flat grade chutes.
- Feed queue redrawn as a true V-profile with a translucent cover and three
  queued nuts seated seam-down.

### C2b — support-structure correction
- The four sorter posts at (±160, ±60) and the C2 hopper stand legs both passed
  through the oscillating decks. Replaced by a straddle GANTRY: four corner
  columns fully outside the deck/drive envelope (x -210 & +215, z ±170, on the
  base rails), two sorter support rails carrying the deck frame at its z-edges,
  two top rails, hopper cross beams bridging ABOVE the top-deck skirt crest
  (~y865), and cradle beams seating the cone. Clearances audited against deck
  oscillation, chain guard, take-off chain, eccentric, troughs, bins and hood.

---

## Rev D (2026-09-10) — layout, drivetrain and process rebuild

Standalone: `standalone/Pili Cracker Rev D - Interactive 3D Model.html` (self-contained,
three r128 from cdnjs). Rev D was built standalone-first; `src/PiliAssemblyModelRevC.jsx`
is now superseded and has NOT been ported — treat the Rev D HTML as the editable source.

Design reference: Chapters 1–3 Working Draft Rev 2 + the 8 Sept 2026 design-verification
note (crack orientation, separation order).

- **Linear gravity cascade.** Hopper and three decks at +x, bins/funnel/V-channel
  feeding the nip in the middle, two-stage screen box and trays at −x. Every hand-off is
  a supported chute or trough; no gantry straddles the decks. Frame is 40×40 tube:
  base ring, six cross rails, columns only outside the oscillating envelopes, a cracker
  table the 10 mm side plates bolt to, and a motor/reducer pedestal at shaft height.
- **Drivetrain made explicit.** Motor → input coupling → 40:1 self-locking worm reducer →
  output coupling → fixed-roller (main) shaft. Rear extension of that shaft carries, in
  order: screen eccentric, 16T sync sprocket, 16T take-off sprocket, coupling. Adjustable
  roller is chain-synchronised only (serpentine, Rev C1 internal-tangent geometry kept:
  17.5 mm shaft clearance at every gap). Take-off chain → 11T jackshaft (16:11 step-up) →
  eccentric → rod → deck frame on four rocker links. Screen box floats on four springs
  and is stroked by a rocker lever driven from the main-shaft eccentric (no bent rods).
  Chains are drawn with moving link stripes. The blower has its own motor.
- **Nut orientation per the verification note.** In the V-channel the nut lies tip-to-tail
  with one ridge in the groove; at the nip it is tip-first (long axis vertical), ridge
  toward the adjustable roller, convex face toward the fixed roller — groove bisector in
  the plane of the roller centres. Roller surface: 60 shallow grip serrations.
- **Separation per the verification note.** Catch funnel → coarse ≈24 mm screen (tilted
  18° to the FRONT; retained halves/uncracked nuts drop into a sorting tray at the
  operator's position) → 8 mm screen (dust to a static front-pull drawer) → cross-flow
  air at the lip (chips to the chip tray, kernels drop to the kernel tray, adjustable
  divider). This is NOT yet in the Rev 2/Rev 3 text (§3.2, §3.3 item 6) — team decision.
- **Decks.** Top 31 mm and middle 25 mm perforated; bottom deck drawn SOLID (collecting
  deck) per the Rev D brief. Rev 2 §3.2 says 20 mm apertures + fines drawer; set
  `BOTTOM_DECK_PERFORATED = true` in the script to restore that. Deck discharge openings
  are offset in z (+88/0/−88) with plan deflectors so the three troughs never cross.
- **Live process simulation.** Nuts spawn at the hopper, grade through the decks (with a
  small mis-grade chance near boundaries), accumulate in the bins, release through the
  selected gate only, queue in the lane, are drawn tip-first through the nip, judged by the
  same two gap inequalities as the nip simulator (whole / crushed / uncracked), and their
  fragments follow the separation path to the trays. Panel shows counts everywhere;
  "Return uncracked nuts" performs the manual repass. Playback speed 0.25–4×.
- **Envelope.** ≈1.13 × 0.55 × 1.35 m including the drive overhang. The 950 × 450 × 700 mm
  in §3.2/§3.8 cannot hold this stack; replace it from CAD.
- Kept from C2: tap-a-bin interlock animation, drag-handwheel gap change (now with a
  scale pointer on the carriage), explode view, subsystem toggles, part inspector.
