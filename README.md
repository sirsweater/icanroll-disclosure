# ICanRoll — Prior Art Disclosure

**Physical Dice Rolling System with Authenticated Remote Photographic Delivery**

Published for defensive purposes — March 2026  
Gregory Noel Kruger — Denton, Texas  
Hackaday profile: https://hackaday.io/sirsweater

> This document establishes a public prior art record as of its publication
> date. Patent applications covering features intentionally omitted from
> this disclosure may be pending. All rights reserved.

---

## The Problem

Existing remote dice and gaming systems require users to trust either a
regulatory body's certification of a random number generator, a
cryptographic proof they cannot personally verify, or a third-party
auditor's attestation.

This system eliminates the need for trusted intermediaries by making the
physical outcome directly observable by the end user. A photograph of a
real die face requires no mathematical literacy to verify.

> **The user is their own auditor. Everyone understands a dice roll
> more than an algorithm.**

The system is a general-purpose physical dice rolling utility applicable
to any context where verifiable physical dice outcomes are preferred over
software-generated randomness — tabletop games, board games, casino-style
games, research, education, or any wagering context.

---

## What It Is

A motorized physical dice roller connected to the internet. A motor shakes
real dice in a container. A camera photographs the result. The photograph
is delivered to remote users over the internet via an authenticated session.
The photo is the proof — no RNG, no algorithm, no intermediary required.

### System Architecture

Three-tier design:

- **Client** — browser-based interface, any device
- **Central API** — session management, authentication, worker routing
- **Worker Node** — single-board computer controlling physical hardware:
  motor, camera, and LED illumination per dice container

### One Complete Roll

1. User configures dice type and named players in the web interface
2. User clicks Roll — a session token is created binding all data to them
3. API routes the request to the worker node matching the dice configuration
4. If hardware is in use the request queues automatically
5. Worker motor agitates the container until dice randomize and settle
6. LED illuminates, camera captures a still photograph
7. Photo is returned to the API, associated with the session token,
   and delivered to the user's browser
8. User sees and verifies the physical result themselves

### Dice Configuration

The hardware accepts any physical dice. No hardware modification is required
to change dice type or quantity. The software reflects whatever is loaded.
Reference implementation uses standard polyhedral dice: D4, D6, D8, D10,
D12, D20, D100.

---

## Key Features

- **Authenticated photo delivery** — photos accessible only via valid
  session token, not publicly accessible
- **Named player sessions** — multi-participant support with round tracking
- **Scratch / void system** — rolls marked void are preserved with reason,
  never silently deleted
- **PDF export** — complete session record with embedded photographs,
  round structure, timestamps, and void stamps
- **Session expiry** — idle timeout and hard maximum duration server-side
- **Media grace period** — photos remain accessible after session end for
  PDF download
- **Badge encoding system** — portable 16-bit achievement codes using
  XOR/multiply/salt/checksum scheme, no server-side storage required
- **Worker heartbeat registration** — worker nodes self-register with the
  API every 30 seconds; live hardware registry maintained automatically
- **Up to 8 physical dice containers per worker node**
- **Per-container independent hardware tuning** — shake speed, duration,
  and settle time configurable per container to suit different dice types

---

## Claims of Novelty

The following combinations are believed to be novel as of March 2026:

- Physical motorized dice actuation combined with photographic capture
  delivered as an authenticated record to remote users — as a unified system
- User as their own auditor — no mathematical literacy, cryptographic
  knowledge, or trusted third party required to verify the result
- Configuration-aware worker routing — API matches requests to worker nodes
  by the physical dice configuration currently loaded
- Per-container hardware queue management via threading lock — sequential
  roll processing without user retry
- Session token ownership model — all roll data bound to the initiating
  user's token; accessible only to them
- Multi-modal verification — still photograph now, video of full actuation
  event as a designed future capability
- Session-linked exportable audit trail — PDF with embedded photos and
  complete roll record
- Dice-agnostic physical hardware — any configuration, no hardware
  modification required
- Worker self-registration via 30-second heartbeat — live registry of all
  available physical hardware without manual configuration
- LED-synchronized camera capture — dedicated illumination per container
  with configurable timing for consistent exposure
- Up to 8 independent containers per worker node with fully independent
  hardware assignments

---

## Prior Art Reviewed

The following patents were reviewed and found not to conflict. Each is
anchored to casino wagering infrastructure not present in this system.

| Patent | Description | Conflict |
|---|---|---|
| US20230306815A1 | Casino player performance comparison | None |
| US11887441B2 | Hybrid craps table with electronic displays | None |
| US8758109 | Casino slow-motion camera replay at local stations | None |
| US10535230 | Cryptographic provably fair RNG scheme | None |
| US20200302744A1 | GPS-based betting on casino player performance | None |
| Roulette aggregation sensing patent | Camera detects roulette outcomes for set-based wagering | None |

None of the above claim the combination of physical dice actuation,
camera-based photographic delivery, authenticated remote session
management, and user-verifiable physical outcomes.

---

## Intentional Omissions

**The following feature is intentionally omitted pending provisional
patent filing:**

Automated image-based extraction of dice face values from photographs
and/or video frames — including any combination of computer vision,
machine learning, or frame analysis used to suggest or confirm the
numerical outcome of a physical dice roll. This feature and any
combination of it with the pipeline described herein is not disclosed
and is subject to a pending patent application.

The inventor was aware of and had conceived this capability at the time
of this publication and elected to withhold it for patent protection.

---

## Publication Record

- **GitHub:** https://github.com/sirsweater/icanroll-disclosure — March 2026
- **Hackaday.io profile:** https://hackaday.io/sirsweater
- **Hackaday.io project:** [will add after creator approval]
- **Archive.org snapshot of GitHub:** [will add after archiving]
- **Archive.org snapshot of Hackaday:** [will add after Hackaday publishes]

---

*ICANROLL PRIOR ART DISCLOSURE — Gregory Noel Kruger — March 2026*  
*All rights reserved.*
