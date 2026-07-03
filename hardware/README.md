# Hardware

This folder contains the HandUMI printable hardware.

HandUMI has two hardware configurations that share the same core printed frame:

- Data collector: worn on the hand to record wrist-view video, gripper width,
  and tracker pose.
- Deployment unit: mounted on a robot end-effector with matching camera
  viewpoint and gripper-tip geometry.

## Folder Layout

Printable files live in `STL/`, and editable CAD sources for every part live
in `STEP/`:

```text
STL/
|-- left_handumi/   # Left-hand HandUMI parts
|-- right_handumi/  # Right-hand HandUMI parts
`-- gripper_tips/   # Robot-specific detachable tips, one folder per robot
    |-- AgileX-Piper/
    |-- ARX-X5-2023/
    |-- Dream-Gripper/
    |-- Trossen-WidowXAI/
    `-- UMI-Gripper/

STEP/
|-- left_handumi/   # STEP sources for the left-hand parts
|-- right_handumi/  # STEP sources for the right-hand parts
`-- gripper_tips/   # STEP sources for the tips, for custom adjustments
    |-- AgileX-Piper/
    |-- ARX-X5-2023/
    |-- Dream-Gripper/
    |-- Trossen-WidowXAI/
    `-- UMI-Gripper/
```

## Modular Gripper Tip

HandUMI is modular: the body, camera mount, servo, and tracking mount stay the
same, while the detachable gripper tip changes to match the target robot
gripper.

Currently supported grippers:

- AgileX Piper (`STL/gripper_tips/AgileX-Piper/`)
- ARX X5 2023 (`STL/gripper_tips/ARX-X5-2023/`)
- Dream Gripper TRLC (`STL/gripper_tips/Dream-Gripper/`)
- Trossen WidowX AI (`STL/gripper_tips/Trossen-WidowXAI/`)
- UMI Gripper (`STL/gripper_tips/UMI-Gripper/`)

Any robot with a comparable parallel-jaw gripper can be supported by designing
and printing a matching tip. Start from the STEP sources in
`STEP/gripper_tips/` if you need to adjust a tip or adapt it to a new gripper.
STEP sources for the full left/right bodies are also available in
`STEP/left_handumi/` and `STEP/right_handumi/` for custom modifications
(e.g. refitting the finger cradles to another operator's hand).

## Print Guide

- PLA is the default material for early builds.
- For a left HandUMI, print the complete `STL/left_handumi/` folder.
- For a right HandUMI, print the complete `STL/right_handumi/` folder.
- Depending on your robotic arm with a parallel gripper, print the
  corresponding gripper tips from `STL/gripper_tips/`. If you want to make
  adjustments, modify the STEP sources in `STEP/gripper_tips/` and export your
  own STLs.

Detailed orientation, support, layer height, and infill settings will be added
in a future update.

## Assembly

HandUMI assembly is organized around three modules:

1. Finger-worn body and thumb/index-middle finger cradle.
2. Camera and tracking mounts.
3. Detachable robot-specific gripper tip.

For deployment on a new arm, keep the main body unchanged and swap only the
gripper tip.
