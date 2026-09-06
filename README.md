# stupidOS

> **Make phones stupid again. Give old hardware another life.**

stupidOS is a minimal, open-source operating system for old Android phones. It reuses the
hardware enablement that already works on the device (through the phone's Android vendor
stack, kept isolated as a hardware backend) and puts a small, readable Linux userspace
with a **text-first UI** on top.

The engineering focus is narrow on purpose: **reliable calls, SMS, and hotspot** — the
things that make a phone a phone — done well, on hardware people already own.

> **Reuse the hardware work. Own the software. Keep it small enough to read.**

Today the target is old hardware; tomorrow it may include commodity phones too.

---

## Why stupidOS?

Older phones still have perfectly good radios, Wi-Fi, displays, speakers, microphones,
batteries, processors, and storage. They become unpleasant only because modern software
gets heavier every year.

stupidOS takes the opposite approach:

> **Keep the hardware. Remove the unnecessary software.**

Two purposes:

1. **Make the phone stupid again.**
2. **Give old hardware another useful life.**

---

## What stupidOS does

In rough priority order:

1. Run on hardware you already own, using it **minimally and efficiently**.
2. **Calls and SMS** — clear audio, the best connection possible. *(where we spend our
   engineering power)*
3. **Wi-Fi hotspot** — reliable connection sharing. *(also a focus)*
4. **Contacts** — create and manage, stored locally.
5. A few **utility apps** to bring people onto the platform.
6. Long term: a **shared economy** where many phones pool their resources.

Everything outside this stays as small as possible.

---

## Approach: reuse Android's hardware work, own the software

Building a phone OS directly on mainline Linux hits a wall: on most of these SoCs the
mainline kernel cannot bring up the **modem or call audio** — no open replacement exists
for the vendor's proprietary radio stack. That gap is the wall.

So stupidOS does **not** rebuild hardware enablement from scratch, and it is **not** a
stripped-down Android either. Instead it reuses the device's proven Android hardware layer
as an **isolated backend**, and builds a clean, minimal Linux system on top. See
[`fork.md`](fork.md) for the full rationale.

### The reuse vs. write-clean split

```
stupidOS layer   WRITE CLEAN   text UI, dialer/SMS logic, contacts, hotspot control,
                               orchestration, the backend interface        (ours, readable)
open plumbing    REUSE (open)  ofono, PulseAudio/PipeWire, BlueZ,
                               wpa_supplicant, hostapd + dnsmasq, TLS, libc (mature, don't rewrite)
vendor bridge    REUSE (blob)  Android HAL + libhybris, RIL, audio HAL     (sealed, isolated)
kernel           REUSE         vendor Android kernel (GPL source)
firmware         REUSE (blob)  modem / GPU / Wi-Fi firmware
```

The layer that makes it *stupidOS* — the UI, phone logic, and orchestration — is code we
write, read, and own. The modem/call path is a quarantined vendor blob behind a single
interface. Only the radio-facing daemons that must always listen run in the background;
nothing else.

### Pluggable backend, with an open fallback

The telephony/audio interface has two implementations behind it: a **vendor-blob** backend
(works where mainline cannot) and an **open** backend used as a clean fallback where it
actually works. The right one is chosen per device.

Device support is **narrow and curated** — a small set of devices where calls genuinely
work, not a promise of broad compatibility.

### Blobs are install-time only

stupidOS does **not** redistribute proprietary blobs. They are extracted from the user's
own device (or stock image) at install time. If extraction fails, install fails. The image
we distribute is the open parts only.

---

## App model: web-style stupidOS apps

An app is a **tiny local service with a light (text/web) frontend** — no Android runtime,
no APKs. This keeps apps small, sandboxable, and on-demand, and it lets the large
web-developer ecosystem build for stupidOS. A future **decentralized app store** would
distribute these small bundles.

---

## What is deliberately NOT in scope

- Android app compatibility / APKs / an Android runtime
- App store accounts, cloud accounts, telemetry, advertising
- Social media, messaging ecosystems, games, multimedia ecosystems
- A desktop environment
- Heavy background services

A capability is not "done" until it is reliable. We do not chase breadth while a core
capability is still flaky.

---

## Reference device

The initial candidate is the **Google Pixel 4a (`sunfish`, Snapdragon 730G / SM7150)** —
old hardware already owned. The final reference device is being selected on one criterion:
a **proven two-way call-audio path**, since "best possible call" has to start from a
known-good baseline. Additional devices are added only as narrow, curated ports.

---

## Development philosophy

- **Small is a feature.** Every dependency justifies its existence.
- **Reuse proven components** at the component level; do not reinvent working kernel,
  radio, networking, or audio stacks without a strong reason.
- **Readable and owned.** The system should be small enough to read and understand; we do
  not want to depend fully on tooling to comprehend our own OS.
- **Reliability over features.** A phone that does a few things reliably beats one that
  does forty things badly.
- **No feature creep.** If a feature does not serve the core purpose, it belongs in an RFC,
  not the core.
- **No cloud, accounts, telemetry, or ads in the core OS.**

---

## Status

**Project stage: design.** The architecture is decided (reuse Android's hardware layer as
an isolated backend; minimal Linux userspace + text UI; pluggable backend with an open
fallback; install-time-only blobs; web-style apps). The current open item is selecting the
reference device with a proven call path.

---

## Contributing

The project is intentionally starting small. Early useful contributions:

- Device research (proven call-audio ports, blob-extraction stories)
- Radio/modem and audio integration behind the backend interface
- Networking and hotspot
- The text UI and core phone logic
- Documentation and testing on old phones

Before adding a feature, ask:

> **Does this help make the phone a reliable, minimal phone?**

If not, it probably belongs in an RFC rather than the core.

---

## License

License: **TBD** — an open-source license will be selected before the first public release.
