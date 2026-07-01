# Hardware

This folder contains the HandUMI printable hardware.

HandUMI has two hardware configurations that share the same core printed frame:

- Data collector: worn on the hand to record wrist-view video, gripper width,
  and tracker pose.
- Deployment unit: mounted on a robot end-effector with matching camera
  viewpoint and gripper-tip geometry.

## STL Layout

All printable files live in `stl/`:

```text
stl/
|-- common/         # Parts shared by both hands
|-- left_handumi/   # Left-hand HandUMI parts
|-- right_handumi/  # Right-hand HandUMI parts
`-- gripper_tip/    # Robot-specific detachable tips
```

## Modular Gripper Tip

HandUMI is modular: the body, camera mount, servo, and tracking mount stay the
same, while the detachable gripper tip changes to match the target robot
gripper.

Current target tip filenames:

- `arx-gripper-tip.stl`
- `trossen-gripper-tip.stl`
- `trlc-dk1-gripper-tip.stl`

Place all robot-specific tips directly in `stl/gripper_tip/`.

## Print Guide

- PLA is the default material for early builds.
- Print the common body parts from `stl/common/`.
- Print the hand-specific parts from `stl/left_handumi/` or
  `stl/right_handumi/`.
- Print the deployment gripper tip that matches your robot from
  `stl/gripper_tip/`.

Detailed orientation, support, layer height, and infill settings will be added
with the STL release.

## Assembly

HandUMI assembly is organized around three modules:

1. Finger-worn body and thumb/index cradle.
2. Camera and tracking mounts.
3. Detachable robot-specific gripper tip.

For deployment on a new arm, keep the main body unchanged and swap only the
gripper tip.
