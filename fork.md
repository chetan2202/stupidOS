# stupidOS — New Design Approach

## The idea

stupidOS will start from the open-source Android Open Source Project (AOSP) rather than building a phone OS directly on top of a bare Linux system.

The reason is simple: **the hardware already works with Android.**

Instead of spending years recreating the hardware-enablement work that Android and device vendors have already done, stupidOS will use AOSP as a working foundation and progressively remove everything that is unnecessary.

> **Use Android's hardware knowledge. Remove Android's complexity. Build stupidOS.**

## What we keep

AOSP and the device/vendor stack provide a large amount of proven hardware integration:

* Boot and hardware initialization
* Linux kernel integration
* Display and touchscreen
* Audio
* Wi-Fi and Bluetooth
* Sensors
* Power management
* Storage
* Camera
* Cellular/modem integration
* Device-specific hardware support

Where proprietary firmware is unavoidable, stupidOS may continue to use that firmware. Using required hardware firmware does not mean using Android as the operating system.

## What we remove

stupidOS will aggressively remove components that exist primarily to support the Android ecosystem:

* Google services
* Play Store
* Google accounts
* Advertising and telemetry
* Unnecessary background services
* Android ecosystem dependencies
* Complex UI and system applications
* Features that do not serve the core purpose of the device

The result should be a small, understandable system rather than a complete Android distribution.

## Evolution

The project can evolve gradually:

```text
AOSP
  ↓
AOSP + stupidOS changes
  ↓
Remove Google and unnecessary Android components
  ↓
Replace Android services with stupidOS services
  ↓
Replace components with native Linux/open implementations
  ↓
Minimal independent operating system
```

This avoids making hardware support the first problem to solve.

## Core principle

stupidOS is **not an Android fork whose goal is to remain Android forever**.

AOSP is the starting point and the reference implementation for the hardware.

The long-term goal remains:

> **Own the operating system while reusing proven hardware-enablement work wherever necessary.**

This approach allows stupidOS to become simpler and more independent over time without requiring us to reinvent the entire smartphone hardware stack on day one.

## First target

The Google Pixel 4a (`sunfish`) remains the initial development device.

The first milestone is not to reinvent the phone stack.

It is simply:

**Boot → hardware works → remove unnecessary Android → introduce stupidOS.**

From there, the system can be simplified one subsystem at a time.
