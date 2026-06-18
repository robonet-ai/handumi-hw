# HandUMI

A hand-worn, open-source variant of the [Universal Manipulation Interface (UMI)](https://umi-gripper.github.io/) for collecting **bimanual** manipulation data without a robot in the loop. HandUMI mounts on the operator's **thumb and index finger** and opens or closes with a natural pinch. No handheld gripper to carry, no exoskeleton to strap on. Pose comes from a **PICO 4 Ultra headset and two wrist trackers**, gripper width comes from a **servo encoder**, and an **interchangeable fingertip** lets the data deploy on any robot arm with a **parallel-jaw gripper**.

<p align="center">
  <img src="media/HandUMI.jpg" alt="HandUMI worn on the hand, with PICO tracker and wrist-camera mounts" width="720">
</p>

## What it is

HandUMI keeps UMI's core idea, *collect manipulation demonstrations without a robot and deploy on one*, but replaces the handheld gripper with a finger-worn device and offloads pose tracking to a VR stack. The operator pinches, grasps, and releases as they would barehanded, with both hands. Each demonstration records the three things a UMI-trained policy expects at inference time:

- **SE(3) wrist pose** from the PICO 4 Ultra headset plus two wrist trackers (inside-out tracking, no SLAM post-processing).
- **Exact gripper width** from a Feetech servo and its encoder, read directly off the mechanism.
- **Wrist-view video** from a small camera mounted on the device.

<p align="center">
  <img src="media/HandUMI-demo.gif" alt="HandUMI demo, worn on the hand and opening/closing by pinch" width="640">
</p>

Two configurations share the same 3D-printed frame:

- **Data collector**: worn on the hand, records wrist-view video, gripper-width telemetry, and tracker pose.
- **Deployment unit**: mounts on a robot end-effector for policy rollout, with the same camera viewpoint and fingertip geometry.

## Direct gripper-width sensing

Most UMI-style rigs estimate gripper aperture indirectly, from a QR fiducial or by segmenting the fingers in the image. HandUMI measures it directly: a **Feetech servo with its encoder** sits in the mechanism, so the recorded width is the true mechanical aperture, frame for frame. Because the servo reports load as well as position, the same channel exposes how hard the fingers are squeezing, which matters for dexterous and soft-object manipulation where contact force, not just open/closed, drives the task.

## Motion tracking via PICO 4 Ultra + wrist trackers

Pose is captured with a **PICO 4 Ultra** headset and **two wrist trackers**, worn like lightweight bracelets. The headset provides the world frame through inside-out tracking; the wrist trackers give each hand its own SE(3) trajectory. This replaces the camera VIO-SLAM pipeline used by the original UMI: there is no offline reconstruction step, both hands are tracked at once for bimanual demonstrations, and because pose no longer comes from the camera, the wrist camera only has to record video and can be a cheap off-the-shelf part.

The captured trajectories are recorded relative to the gripper's current end-effector pose, then retargeted to whatever arm you want to deploy on.

## Interchangeable fingertip

To target a different robot you only swap the **detachable gripper tip** (the orange fingertip in the renders), not the rest of the device. The body, camera mount, servo, and trackers stay the same; you print the fingertip whose geometry matches your arm's parallel jaw and HandUMI data transfers without reworking anything else. The same demonstrations can therefore drive any of these arms:

- **[Agilex Piper](https://global.agilex.ai/products/piper)**
- **[UR](https://www.universal-robots.com/)** (UR3 / UR5 / UR5e with a two-finger gripper)
- **[Trossen Robotics](https://www.trossenrobotics.com/)** (WidowX / ViperX)
- **[ARX](https://www.arx-x.com/)**
- **[Open Arm](https://github.com/enactic/openarm)**
- **TRLC-DK1** (The Robot Learning Company)

Any other arm with a comparable parallel-jaw gripper (UR with WSG-50, Franka with the Panda gripper, xArm with a two-finger gripper) is supported by printing a matching tip. This keeps the device robot-agnostic while still producing data that lands exactly on the deployment hardware.

## Wrist-view camera

The wrist-view camera supplies the visual observation the policy sees at train and deploy time. Because pose is handled by the PICO stack, the camera no longer has to support VIO-SLAM, so it does not need to be a GoPro or 360° body. A cheap **wide-angle UVC USB module** is enough: the reference part is a 32×32 mm 1080p UVC camera with a 130° lens and an M12 mount (the same module class used by the [SO-ARM100](https://github.com/TheRobotStudio/SO-ARM100) wrist-cam adapters). A wider lens, roughly **140–170° FOV**, is preferable for more coverage of the workspace.

The frame uses a swappable camera mount, so it can also carry a GoPro Hero or Insta360 X4 body when a specific viewpoint or 360° capture is wanted.

## Hand-fit design via 3D scan

The finger cradle geometry was designed around a 3D scan of my own hand taken with **[Hunyuan 3D](https://3d.hunyuanglobal.com/)**, Tencent's image-to-3D model. One photo of the hand produces a watertight mesh, which is imported into CAD as the reference surface for the thumb and index rings. The same workflow can be used to refit HandUMI to a different operator.

<p align="center">
  <img src="media/3d_hunyuang_hand.png" alt="Hunyuan 3D scan of the operator's hand, used as the CAD reference surface" width="540">
</p>

## What HandUMI replaces

The goal is to collect manipulation data without a robot in the loop. Classical leader / follower teleop rigs like the one below (two arms, a support frame, synchronized cameras, cabling) are expensive, immobile, and tied to a single embodiment, and a bimanual rig needs a second pair of arms just to act as the leader. HandUMI removes that rig: the operator wears it and performs the task directly, with both hands, anywhere.

<p align="center">
  <img src="media/Follower_Leader_setup.png" alt="Classical bimanual leader / follower teleop rig, the kind of setup HandUMI removes" width="720">
</p>

## Why UMI-style data collection

- **Hardware-agnostic policies.** Actions are recorded as trajectories relative to the gripper's current end-effector pose, so a policy trained on HandUMI data can be retargeted to different arms (UR5, Franka, PiPER, ARX, Trossen, 6-DoF or 7-DoF) without retraining.
- **No robot during collection.** Demonstrations are recorded anywhere, without controllers, teach pendants, or a lab. A robot arm is only needed at deployment.
- **Bimanual by default.** Two wrist trackers capture both hands at once, so coordinated two-hand tasks are demonstrated naturally rather than puppeteered.
- **In-the-wild coverage.** Data can be gathered across homes, offices, kitchens, and outdoor scenes.
- **Throughput.** On the original UMI cup-arrangement benchmark, the handheld gripper collected demonstrations about 3× faster than SpaceMouse teleoperation, reaching roughly 48% of the speed of a bare human hand.
- **Feasible on complex tasks.** Dynamic, bimanual, precise, and long-horizon tasks (tossing, cloth folding, multi-step kitchen work) that are hard to teleoperate reliably can be demonstrated directly.
- **Consistent viewpoint.** The wrist-mounted camera sits in the same position during training and deployment, which keeps the observation distribution stable.

### How it compares

| Method | Robot-agnostic | Interface | Gripper width | Complex tasks | In-the-wild |
|---|:---:|---|:---:|:---:|:---:|
| SpaceMouse / VR controller | ❌ | Cartesian EE | n/a | ⚠️ | ❌ |
| Puppeteering ([ALOHA](https://arxiv.org/abs/2304.13705)) | ❌ | Leader / follower, articular | joint readout | ✅ | ❌ |
| [GELLO](https://arxiv.org/abs/2309.13037) | ❌ | Kinematically isomorphic leader | joint readout | ✅ | ❌ |
| UMI | ✅ | Relative EE trajectories | QR / vision estimate | ✅ | ✅ |
| **HandUMI** | **✅** | **Relative EE trajectories** | **servo encoder (direct)** | **✅** | **✅** |

## Target robots

HandUMI is designed for robot arms with a **parallel-jaw gripper** whose finger geometry matches the swappable deployment fingertip (see [Interchangeable fingertip](#interchangeable-fingertip) for the supported arms: Agilex Piper, UR, Trossen Robotics, ARX, Open Arm, TRLC-DK1, and others). A common reference target is the **[Agilex Piper](https://global.agilex.ai/products/piper)**, a 6-DoF arm with 1.5 kg payload, Python API, and ROS 1 / ROS 2 support. Hosting the deployment unit on any of these arms only requires printing the matching fingertip and an adapter plate.

## Bill of Materials

Prices in USD. A printable PDF is in [`docs/BoM HandUMI.pdf`](docs/BoM%20HandUMI.pdf). The kit has two layers: the per-hand HandUMI gripper, built from cheap off-the-shelf parts, and a shared tracking layer (PICO 4 Ultra + two wrist trackers) reused across every demonstration.

### One pair of HandUMI data collectors (gripper layer)

| Part | Qty | Price | Shipping |
|---|---:|---:|---:|
| PLA filament, 1 kg | 1 | 18.99 | 0.00 |
| Wide-angle UVC USB camera (130°, 32×32 mm) | 2 | 37.98 | 16.60 |
| Feetech servo + encoder | 2 | 27.98 | 0.00 |
| Microcontroller (servo readout / trigger) | 2 | 21.98 | 0.00 |
| Rolling bearing MR83ZZ | 1 | 8.99 | 15.49 |
| Rolling bearing MR63ZZ | 4 | 13.19 | 15.44 |
| Linear bearing LM4 | 4 | 7.49 | 15.55 |
| Axle Ø4 mm | 2 | 10.79 | 16.85 |
| M3 screws and nuts (M3×14 mm and M3×12 mm) | 6 + 8 | 23.97 | 18.35 |
| Hook-and-loop fastener | 2 | 9.99 | 16.52 |

### One pair of HandUMI deployment units (gripper layer)

| Part | Qty | Price | Shipping |
|---|---:|---:|---:|
| PLA filament, 1 kg | 1 | 18.99 | 0.00 |
| Wide-angle UVC USB camera (130°, 32×32 mm) | 2 | 37.98 | 16.60 |
| USB-C to USB-A cable | 2 | 31.98 | 15.86 |

### Tracking layer (shared, bought once)

| Part | Qty |
|---|---:|
| PICO 4 Ultra headset | 1 |
| Wrist tracker | 2 |

The tracking layer is a one-time purchase reused across both hands and every session, so it is not duplicated per collector. A GoPro or Insta360 body can replace the UVC module when a specific viewpoint or 360° capture is wanted, at higher cost.

## Status

Open-source release in progress. CAD, print files, the servo/encoder firmware, and the PICO tracking + retargeting scripts will follow in this repo.

## Related work

- [Universal Manipulation Interface (UMI)](https://umi-gripper.github.io/), Chi et al., RSS 2024
- [DexUMI](https://dex-umi.github.io/), wearable hand exoskeleton variant
- [Generalist AI](https://generalistai.com/), GEN-0 and GEN-1 foundation models trained on wearable data hands
