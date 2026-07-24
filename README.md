# Fennec: Autonomous Quadruped (silicon → autonomy, built solo)

A four-legged robot I designed and brought up end-to-end: custom power electronics, from-scratch real-time firmware, a servo-bus driver, a ROS 2 control contract, and LIDAR-inertial SLAM: all integrated into one machine that stands, balances, and maps. Built on the open-source Nova-SM3 platform, heavily redesigned.

**Why it matters:** most robotics projects stop at one layer. Fennec is the whole stack: I own it from the PCB copper to the autonomy node, which is exactly the systems-integration work that makes hardware actually ship.

## Highlights
- **200 Hz hard-real-time control** on a Teensy 4.1 (IMXRT1062 Cortex-M7): p99 scheduling jitter **1 µs**: 100× under my <100 µs acceptance gate. Software watchdog (SCB_AIRCR reset on hang) + exec-time/latency histograms streamed over USB-CDC.
- **Feetech STS3215 servo-bus driver, from scratch**: half-duplex protocol (PING / READ_DATA / SYNC_WRITE), 74HC125 tri-state OE timing verified on a logic analyzer, categorized error counters, per-joint slew-rate limiting, round-robin telemetry.
- **Custom multi-rail power PCB** (KiCad): battery → regulated buck rails, INA226 per-rail current/voltage telemetry, MOSFET reverse-polarity protection, a hardware safety/E-stop chain. DRC/ERC clean, fabricated.
- **22-topic ROS 2 contract** (micro-ROS / XRCE-DDS) from the Teensy up to an NVIDIA Jetson Orin Nano (embedded Linux), with a latching E-stop + battery-low safety FSM (50 ms debounce, explicit `/safety_clear` reset).
- **LIDAR-inertial SLAM** (Unitree L2 + POINT-LIO) on the Jetson. Found and fixed an IMU-bridge bug (double `getImuData()` queue drain) in `unilidar_sdk2` and shipped a **public fork** that restored the 250 Hz IMU stream.

## Research angle
SLAM on a **legged** platform is harder than on wheels: the body sways during locomotion, so the perception stack has to compensate for that ego-motion instead of assuming a smooth base. Getting stable mapping while the robot walks is a genuinely open problem at the undergrad level: and the through-line for manipulation + mobility + perception→action work.

## Stack
`C / C++` · `Teensy 4.x (Cortex-M7)` · `RTOS` · `ROS 2 (Humble)` · `micro-ROS / XRCE-DDS` · `NVIDIA Jetson Orin Nano` · `KiCad` · `INA226 / MOSFET power design` · `POINT-LIO SLAM` · `Python` · `PlatformIO`

## Architecture (one glance)
```
Battery ─► [Custom Power PCB: buck rails, INA226 telemetry, MOSFET protect, E-stop chain]
                 │
         Teensy 4.1 (200 Hz ISR loop, safety FSM, watchdog)
                 │  half-duplex bus (74HC125)          │ micro-ROS / XRCE-DDS
         STS3215 servos ×N                       Jetson Orin Nano (ROS 2, POINT-LIO SLAM ◄─ Unitree L2 LIDAR)
```

## Links
- **Public fix:** `unilidar_sdk2` fork: `fix/imu-bridge-double-call`: [github.com/Ace2932/unilidar_sdk2](https://github.com/Ace2932/unilidar_sdk2)
- **Demo video:** _coming: 30–60s clip_

---

*Aiden Fox · Computer Engineering & CS @ USC · embedded · robotics · real-time systems.*
