# Agent instructions

This repository contains the Shore Jetson Nano case and its existing FreeCAD
assembly. Communicate with the owner in Traditional Chinese. Keep changes small
and preserve their drawings.

## Inspect before editing

- Confirm the checkout, `git status --short --branch`, and relevant diff. Preserve
  unrelated edits and untracked files; do not reset, clean, or overwrite them.
- For Blender work, read `docs/blender-handoff.md` first. It records the current
  source fingerprint, material requirements, assembly context, and first deliverable.
- `Nano_Case_v05_DRAFT.FCStd` is the authoritative CAD source. Its existing
  `Reference` group is the assembly; do not create another assembly FCStd.

## Protect the CAD source

- Rendering work reads the FCStd without saving changes to it. Hash it before
  and after work; compare with the handoff fingerprint while that baseline applies.
- Use a temporary copy outside the repository when opening/recomputing/exporting
  with FreeCAD. A successful recompute does not authorize saving over the source.
- Do not alter sketches, dimensions, fillets, placements, or hardware to improve
  a render. Preserve `Sketch014`, `Pocket007`, and the existing base-plate pose.
- Do not create or overwrite STL exports for rendering. The tracked
  `nano_case_with_screen.stl` is a separate owner-provided print export; use the
  FCStd assembly for the Blender scene.
- Keep automatic backups and STEP out of Git according to `.gitignore`.

## Blender workflow

- Prefer Python scripts and small configuration files that can rebuild the scene.
  Preserve part names, grouping, and world positions through the import.
- Do not render every intermediate PartDesign feature or apply parent transforms
  twice. Confirm scale and several assembly landmarks before working on materials.
- Large base plate: metal, black matte. Printed enclosure: milky/off-white plastic;
  the actual polymer has not been specified. Existing FreeCAD display colors are
  not material specifications.
- The current Blender deliverable is a **60-second promotional animation**.
  Read `docs/promo-brief.md`: polished engineering assembly first, then a beach
  setting with tripod and camping power, then a simulated surfer and tracking
  picture-in-picture using the real Shore Jetson App UI references.
- During the engineering assembly, present the **screen above the Jetson Nano**.
  Flip a Blender parent or choose the camera accordingly; keep source CAD unchanged.
- Cartoon styling is welcome; never use block/voxel people. Read
  `docs/promo-assets.md` and continue choosing free assets with the owner.
- The owner selected **Remastered Cameras by bunny man** on Sketchfab for the
  camera model; preserve that selection. Its exact URL, CC BY 4.0 attribution,
  and not-yet-downloaded status are recorded in `docs/promo-assets.md`.
- The owner confirmed Google AI Plus as an available auxiliary resource. Check
  actual access/remaining credits before using it; do not buy credits or upgrade.
- The new promotional animation is explicitly requested on 2026-11-19, superseding
  the earlier static-only/no-animation stage. The old MP4 remains historical and
  must not supply outdated geometry or orientation.
- Do not treat visual animation or a good render as proof of physical fit, cable
  clearance, strength, or verified gimbal motion.

## Completion

Run relevant checks, inspect the actual images and full video, and report the commands
used, outputs, source hash preservation, and remaining limitations. Git commits
and pushes follow the owner's authorization for the current task.
