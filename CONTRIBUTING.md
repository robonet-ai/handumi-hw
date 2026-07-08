# Contributing to HandUMI

Thank you for helping improve HandUMI. This project combines mechanical design,
electronics, data-collection software, and documentation, so clear contribution
details matter. Please use these guidelines when opening issues, proposing
hardware changes, or submitting pull requests.

## Ways to Contribute

- Report build, assembly, fit, printability, or documentation problems.
- Improve documentation, diagrams, photos, assembly notes, and print settings.
- Add or improve specific gripper tips, camera mounts or hand controllers.
- Improve the bill of materials with equivalent parts, better sourcing, or
  corrected prices.
- Contribute software once the `software/` package is published.
- Share validation results from real builds, prints, and data-collection runs.

## Before You Start

For small fixes, you can open a pull request directly. For larger changes,
please open an issue first so the design intent, compatibility constraints, and
validation plan can be discussed before significant CAD or software work.

Large changes include:

- New gripper-tip families.
- Changes to shared left/right HandUMI body geometry.
- Servo, camera, tracker, or bearing changes.
- BOM substitutions that affect mounting, wiring, power, or mechanical
  clearance.
- Software interfaces for recording, calibration, synchronization, or data
  formats.

## Repository Layout

```text
bom/       Bill of materials and sourcing notes
hardware/  Printable STL files and editable STEP sources
media/     Images and GIFs used by the documentation
software/  HandUMI software, currently under development
```

Hardware source files are organized in parallel:

```text
hardware/STL/   Printable exports
hardware/STEP/  Editable CAD sources
```

When contributing a hardware change, include both the editable source and the
printable export when applicable.

## Hardware Contributions

### CAD and Export Requirements

- Keep editable CAD changes in `hardware/STEP/`.
- Keep printable exports in `hardware/STL/`.
- Export STLs in the same orientation and scale expected for printing.
- Preserve the existing folder structure for left-hand, right-hand, and
  gripper-tip parts.
- Use descriptive filenames that match the existing naming style.
- Avoid changing shared body geometry when a robot-specific gripper tip is
  sufficient.

For new gripper tips, add files under:

```text
hardware/STEP/gripper_tips/<Robot-or-Gripper-Name>/
hardware/STL/gripper_tips/<Robot-or-Gripper-Name>/
```

### Design Expectations

Hardware changes should document:

- Target robot, gripper, or hand-fit problem being solved.
- Critical dimensions and assumptions, such as jaw width, mounting spacing,
  camera clearance, servo travel, or fingertip contact geometry.
- Material and print settings used during validation.
- Whether the change was printed, assembled, and tested on hardware.
- Known limitations or fit issues.

For hand-fit changes, explain whether the part is operator-specific or intended
to replace the default geometry.

### Mechanical Safety

HandUMI is a wearable mechanism. Contributions should avoid sharp edges,
pinch hazards, excessive servo force, insecure tracker mounts, and cable routing
that can snag during use. If a change affects force transmission, finger
clearance, or wrist-mounted hardware, describe how it was checked.

## BOM Contributions

The bill of materials lives in `bom/README.md`.

When updating the BOM:

- Keep quantities, unit prices, links, and total prices internally consistent.
- Prefer stable product links where possible.
- Mention regional availability if a part is only available in some markets.
- Describe compatibility when proposing substitutes.
- Include any mechanical, electrical, firmware, or mounting implications.

Prices change over time, so price-only updates are welcome when they improve the
accuracy of the reference estimate.

## Software Contributions

The `software/` directory is currently a placeholder. Until a software stack is
published, please discuss software contributions in an issue before opening a
large pull request. The software will be released in the next few days. 

Future software contributions should include:

- Setup and run instructions.
- Hardware, operating system, and device assumptions.
- Calibration or synchronization requirements.
- Data format details.
- Tests or reproducible validation steps.

Do not commit generated datasets, recordings, local credentials, device-specific
configuration, or large binary outputs unless they are intentionally part of the
repository.

## Documentation Contributions

Documentation changes are valuable. Please keep writing practical and buildable:

- Prefer exact part names, dimensions, file paths, and settings.
- Add photos or diagrams when they clarify assembly or print orientation.
- Update links when files move.
- Keep claims tied to tested behavior or clearly mark them as future work.

Media used by the README should live in `media/`.

## Pull Request Checklist

Before opening a pull request, check the relevant items:

- The change has a clear title and description.
- Related issues are linked.
- CAD source files and exported STLs are both included when applicable.
- BOM totals are recalculated if prices or quantities changed.
- Documentation is updated for new parts, folders, or workflows.
- Print, assembly, software, or validation steps are described.
- Limitations and untested assumptions are called out.
- The branch does not include unrelated formatting or generated files.

## Issue Guidelines

When reporting a problem, include as much relevant detail as possible:

- Which file, part, or assembly step is affected.
- Printer, material, slicer settings, and scale for print issues.
- Photos or screenshots when they help explain the issue.
- Robot/gripper model and measured dimensions for gripper-tip problems.
- Operating system, device model, logs, and reproduction steps for software
  issues.

## Licensing

By contributing to this repository, you agree that your contribution will be
licensed under the repository's Apache License 2.0 license.
