# HandUMI

A finger-worn, open-source variant of the [Universal Manipulation Interface (UMI)](https://umi-gripper.github.io/) for collecting manipulation data without a robot. HandUMI mounts on the operator's **thumb and index finger**, and opens or closes with natural pinch motion. No handheld gripper to carry, no exoskeleton to strap on. Policies trained on HandUMI data deploy on robot arms with a **parallel-jaw gripper**, with the [Agilex Piper](https://global.agilex.ai/products/piper) as the reference target.

<p align="center">
  <img src="media/HandUMI.png" alt="HandUMI with GoPro and Insta360 X4 configurations" width="720">
</p>

## What it is

HandUMI keeps UMI's core idea, *collect manipulation demonstrations without a robot and deploy on one*, but replaces the handheld gripper with a finger-worn device. The operator pinches, grasps, and releases as they would barehanded. The mechanism records the same SE(3) wrist pose, gripper width, and wrist-view video that a UMI-trained policy expects at inference time.

<p align="center">
  <img src="media/HandUMI-demo.gif" alt="HandUMI demo, worn on the hand and opening/closing by pinch" width="640">
</p>

Two configurations share the same 3D-printed frame:

- **Data collector**: worn on the hand, records video and IMU from a 360° or action camera.
- **Deployment unit**: mounts on a robot end-effector for policy rollout, with the same camera viewpoint.

## Camera compatibility

HandUMI supports the two cameras most commonly used for UMI-style data collection:

- **GoPro Hero 9 / 10 / 11 / 12**, as used by the original Stanford UMI (fisheye plus wrist-mounted monocular VIO-SLAM).
- **Insta360 X4**, a 360° dual-lens camera, as used by [Generalist AI](https://generalistai.com/) for the wearable data hands behind GEN-0 and GEN-1. The spherical field of view removes the need for side mirrors.

The frame includes swappable mounts for both.

## Hand-fit design via 3D scan

The finger cradle geometry was designed around a 3D scan of my own hand taken with **[Hunyuan 3D](https://3d.hunyuanglobal.com/)**, Tencent's image-to-3D model. One photo of the hand produces a watertight mesh, which is imported into CAD as the reference surface for the thumb and index rings. The same workflow can be used to refit HandUMI to a different operator.

<p align="center">
  <img src="media/3d_hunyuang_hand.png" alt="Hunyuan 3D scan of the operator's hand, used as the CAD reference surface" width="540">
</p>

## What HandUMI replaces

The goal is to collect manipulation data without a robot in the loop. Classical leader / follower teleop rigs like the one below (two arms, a support frame, synchronized cameras, cabling) are expensive, immobile, and tied to a single embodiment. HandUMI removes that rig. The operator wears it and performs the task directly.

<p align="center">
  <img src="media/Follower_Leader_setup.png" alt="Classical bimanual leader / follower teleop rig, the kind of setup HandUMI removes" width="720">
</p>

## Why UMI-style data collection

- **Hardware-agnostic policies.** Actions are recorded as trajectories relative to the gripper's current end-effector pose, so a policy trained on HandUMI data can be deployed on different arms (UR5, Franka, PiPER, 6-DoF or 7-DoF) without retraining.
- **No robot during collection.** Demonstrations are recorded anywhere, without controllers, teach pendants, or a lab.
- **In-the-wild coverage.** Data can be gathered across homes, offices, kitchens, and outdoor scenes.
- **Throughput.** On the original UMI cup-arrangement benchmark, the handheld gripper collected demonstrations about 3× faster than SpaceMouse teleoperation, reaching roughly 48% of the speed of a bare human hand.
- **Feasible on complex tasks.** Dynamic, bimanual, precise, and long-horizon tasks (tossing, cloth folding, multi-step kitchen work) that are hard to teleoperate reliably can be demonstrated directly.
- **Consistent viewpoint.** The wrist-mounted camera sits in the same position during training and deployment, which keeps the observation distribution stable.

### How it compares

| Method | Robot-agnostic | Interface | Complex tasks | In-the-wild | Approx. cost |
|---|:---:|---|:---:|:---:|---|
| SpaceMouse / VR controller | ❌ | Cartesian EE | ⚠️ | ❌ | Low |
| Puppeteering ([ALOHA](https://arxiv.org/abs/2304.13705)) | ❌ | Leader / follower, articular | ✅ | ❌ | High |
| [GELLO](https://arxiv.org/abs/2309.13037) | ❌ | Kinematically isomorphic leader | ✅ | ❌ | ~$300 |
| **UMI / HandUMI** | **✅** | **Relative EE trajectories** | **✅** | **✅** | **~$370** |

## Target robots

HandUMI is designed for robot arms with a **parallel-jaw gripper** whose finger geometry matches the deployment unit. The reference target is the **[Agilex Piper](https://global.agilex.ai/products/piper)**, a $1,999, 6-DoF arm with 1.5 kg payload, Python API, and ROS 1 / ROS 2 support. Any arm with a comparable parallel gripper (UR5 with WSG-50, Franka with Panda gripper, xArm with a two-finger gripper) can host the deployment unit with an adapter plate.

## Bill of Materials

Prices in USD, sourced from Amazon. A printable PDF is in [`docs/BoM HandUMI.pdf`](docs/BoM%20HandUMI.pdf).

### One pair of HandUMI data collectors

| Part | Qty | Price | Shipping |
|---|---:|---:|---:|
| PLA filament, 1 kg | 1 | 18.99 | 0.00 |
| Insta360 X4 camera | 2 | 799.98 | 0.00 |
| Rolling bearing MR83ZZ | 1 | 8.99 | 15.49 |
| Rolling bearing MR63ZZ | 4 | 13.19 | 15.44 |
| Linear bearing LM4 | 4 | 7.49 | 15.55 |
| Axle Ø4 mm | 2 | 10.79 | 16.85 |
| M3 screws and nuts (M3×14 mm and M3×12 mm) | 6 + 8 | 23.97 | 18.35 |
| Hook-and-loop fastener | 2 | 9.99 | 16.52 |
| Bluetooth remote (dual-camera start / stop) | 1 | 26.69 | 15.69 |

### One pair of HandUMI deployment units

| Part | Qty | Price | Shipping |
|---|---:|---:|---:|
| PLA filament, 1 kg | 1 | 18.99 | 0.00 |
| Insta360 X4 camera | 2 | 799.98 | 0.00 |
| USB-C to USB-A cable | 2 | 31.98 | 15.86 |

### Cost summary

| Category | Parts (USD) | Shipping (USD) | Total (USD) |
|---|---:|---:|---:|
| Data collectors | 920.08 | 113.89 | 1,033.97 |
| Deployment units | 850.95 | 15.86 | 866.81 |
| **Combined** | **1,771.03** | **129.75** | **1,900.78** |

Swapping the Insta360 X4 pair for two GoPro Hero 9 or 10 bodies lowers the bill to the original UMI budget.

## Status

Open-source release in progress. CAD, print files, and firmware for the Bluetooth trigger will follow in this repo.

## Related work

- [Universal Manipulation Interface (UMI)](https://umi-gripper.github.io/), Chi et al., RSS 2024
- [DexUMI](https://dex-umi.github.io/), wearable hand exoskeleton variant
- [Generalist AI](https://generalistai.com/), GEN-0 and GEN-1 foundation models trained on wearable data hands
