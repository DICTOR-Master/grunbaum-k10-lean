# Grünbaum's zone question at k = 10 (Lean 4 + Mathlib)

Author: DICTO.

## The theorems

B. Grünbaum (*Math. Intelligencer* 32(4), 2010) asked: "Given an even integer k ≥ 4, is there a rhombic monohedral
parallelohedron such that every zone has exactly k faces?" The cases k = 4, 6 are classical, k = 8 is his, and he wrote
that "no information is available for any k ≥ 10". The Challenge proves an example for k = 10:

1. **`grunbaum_k10`**: there is a polyhedron `P` of spherical type (a compact solid, the closure of its interior, whose
   frontier is the union of its 30 distinct parallelogram faces and is homeomorphic to the sphere; faces meet in a
   common side, a common corner or not at all; adjacent faces are not coplanar), whose 30 faces are congruent rhombi
   with diagonals in the golden ratio, in which every zone has exactly 10 faces, and whose translates by a lattice
   cover space with pairwise disjoint interiors, face to face.
2. **`grunbaum_k10_parallelohedron`**: the same in Grünbaum's own terms (some set of translates tiles space).
3. **`cube_everyZoneHas`**, **`rhombicDodecahedron_everyZoneHas`**: calibration of the zone definition, which gives 4
   for the cube and 6 for the rhombic dodecahedron.

The witness is DICTO's Grünbaum–Escher Triacontahedron **GET B**: the rhombic triacontahedron less four oblate golden
rhombohedra, a union of 16 golden rhombohedra. It is not part of the statement.

- `Challenge.lean`: the definitions and the four statements, with every proof `sorry`. It imports only Mathlib.
- `Solution.lean`: the four proofs, from the library.
- `GET/`: the library (23 modules, namespace `GET`): exact arithmetic in ℤ[φ] with a verified embedding into ℝ; zone
  certificates with a generic soundness lemma; the cells, the lattice, separation of the cells and of the neighbouring
  translates; volume = covolume (20 + 32φ) and Mathlib's `ZSpan` fundamental domains for the cover; the frontier as
  the union of the 30 faces; faces meeting properly; the sphere by radial projection from an interior point.
  `GET/Statement.lean` repeats the Challenge's definitions word for word. The exact data and certificates are written
  by Python generators; every one of them is re-decided by the kernel (`decide +kernel`).

Toolchain `leanprover/lean4:v4.35.0-rc2`, Mathlib `v4.35.0-rc2`. The four theorems depend only on the axioms
`propext`, `Classical.choice` and `Quot.sound`. Neither `sorry` nor `native_decide` is used. Every Lean file uses the
module system.

## Earlier solids of this family

The rhombic triacontahedron with four corners indented was shown on the web as a space-filler before this work,
without proof, without zone sizes and without reference to Grünbaum's question: by D. Hall (Sketchfab, June 2016; the same
solid as G. Inchbald's dimpled rhombic triacontahedron, by 2023), and by J. Kostick (vZome, April 2022), whose
concavo-convex triacon array shows GET A, the sibling of GET B. GET B itself was not found in the sources checked.
`formalization.yaml` lists every source checked.

## How it was made

DICTO set the question, found and named the solids, and chose the witness. A Claude Code agent (Claude Opus, Anthropic)
wrote the definitions, the statements, the generators and the proofs, and ran the checks. `formalization.yaml`
(section `automation`) records this in full.

## Checks

`scripts/check-lean-sources.py` and `scripts/verify-comparator.sh` come from Palomar's entry template (PalomarRegistry).
They check the source rules and run `lake comparator` the way Palomar does.

## How to cite and reuse

### How to cite

- **The paper.** DICTO, *Ten Faces in Every Zone: Grünbaum's Question at k = 10 — The DICTO Grünbaum–Escher
  Triacontahedron (GET A and GET B)* (paper #115), Zenodo, 2026. DICTO DOI:
  [10.5281/zenodo.24389006](https://doi.org/10.5281/zenodo.24389006) (all versions).
- **The Palomar entry.** Submitted to Palomar (registration pending).
- **The repository commit.** github.com/DICTOR-Master/grunbaum-k10-lean, commit
  `a6b16c25d80fa90ea28259bad6a53d7e81d01779` (the commit submitted to Palomar). Later commits change only metadata and
  documentation, not the Lean files.

`CITATION.cff` holds the same data in machine-readable form (GitHub shows it under "Cite this repository").

### What the library offers

The four theorems of the Challenge are listed above (namespace `GrunbaumK10`). Their definitions (`Parallelogram`,
`IsSphericalPolyhedron`, `IsRhombicMonohedral`, `zone`, `EveryZoneHas`, `lattice`, `TilesByTranslates`,
`IsParallelohedron`, `FaceToFace`) are repeated word for word in `GET/Statement.lean`. The library `GET/` (namespace `GET`)
contains:

- The witness. `PB` (GET B as a union of 16 golden rhombohedra, `GET/Tiling.lean`), its faces `FB` (`GET/Faces.lean`)
  and lattice basis `bB` (`GET/Tiling.lean`), with `PB_spherical`, `PB_tiles` and `FB_faceToFace` (`GET/Main.lean`),
  `FB_zones` and `FB_golden` (`GET/Faces.lean`), `PB_cover` (`GET/Cover.lean`) and `PB_disjoint` (`GET/Tiling.lean`).
- Exact arithmetic in ℤ[φ]. `ZPhi` with `+`, `-`, `*` and the evaluation `ZPhi.toReal`, which commutes with them
  (`ZPhi.toReal_add`, `ZPhi.toReal_mul`, …); the exact sign test `ZPhi.pos`, with `ZPhi.pos_sound`
  (`GET/ZPhi.lean`) and its converse `pos_complete` (`GET/Solid.lean`).
- Zone certificates. `XPar` (a parallelogram with ℤ[φ] data), `ZoneClass`, `ZoneCert` and `everyZoneHas_of_cert`
  (`GET/Zones.lean`): `EveryZoneHas` from a certificate decided by the kernel.
- General lemmas. `segment_eq_segment_iff` (two segments are equal exactly when their endpoints agree, `GET/Zones.lean`);
  `volume_vadd_parallelepiped` (`GET/CellVolume.lean`); `sphere_of_radial` (the frontier of a compact set, seen from an
  interior point from which every ray meets the frontier at most once, is homeomorphic to the unit sphere,
  `GET/Sphere.lean`).

### How to import it

The library builds with `leanprover/lean4:v4.35.0-rc2` and Mathlib `v4.35.0-rc2`; a project that requires it should use
the same toolchain and Mathlib. In `lakefile.toml`:

```toml
[[require]]
name = "GrunbaumK10"
git = "https://github.com/DICTOR-Master/grunbaum-k10-lean.git"
rev = "a6b16c25d80fa90ea28259bad6a53d7e81d01779"
```

Then import the modules you need, for example:

```lean
import GET.Main
import GET.Zones
```

### Licence

The Lean code: Apache-2.0 (see `LICENSE`). Copyright 2026 DICTO. The paper #115: CC BY-SA 4.0.

## Other work by DICTO

- ORCID (all works): https://orcid.org/0009-0001-2620-2036
- Website: https://dictospheres.com

## Paper

The statement formalized here is Theorem 1 of DICTO, *Ten Faces in Every Zone: Grünbaum's Question at k = 10* (paper #115), DICTO DOI 10.5281/zenodo.24389006.
