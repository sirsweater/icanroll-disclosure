# ICanRoll — Prior Art Disclosure

PRIOR ART DISCLOSURE
Physical Dice Rolling System with Authenticated Remote Photographic Delivery
Published for defensive purposes — March 2026
Gregory Noel Kruger, Denton, Texas

This document establishes a public prior art record as of its publication
date. Patent applications covering features intentionally omitted from this
disclosure may be pending. All rights reserved.

======================================================================
1. Title & Inventors
======================================================================

1.1 Title
Physical Dice Rolling System with Authenticated Remote Photographic Delivery

1.2 Inventor
Gregory Noel Kruger
Denton, Texas, United States
Date of first working prototype: March 2026
Date first disclosed outside inventor: March 2026 — demonstrated to a
group of tabletop gaming associates for testing purposes

======================================================================
2. Problem Statement
======================================================================

Existing remote dice and gaming systems require users to trust either a
regulatory body's certification of a random number generator, a
cryptographic proof they cannot personally verify, or a third-party
auditor's attestation. These systems place the burden of verification on
intermediaries rather than on the user themselves.

This system eliminates the requirement for trusted intermediaries by making
the physical outcome directly observable by the end user. A photograph or
video of a physical die face requires no mathematical literacy,
cryptographic knowledge, or specialist expertise to verify. The user is
their own auditor.

"People only trust cryptographic RNG because someone tells them it is
verifiable. This system allows people to be their own auditor. Everyone
understands a dice roll more than an algorithm."

The system is designed as a general-purpose physical dice rolling utility
applicable to any context requiring verifiable physical dice outcomes. Use
cases include but are not limited to: tabletop role-playing games, board
games, casino-style games, educational settings, research applications, and
any wagering or gaming context where physical dice are preferred over
software-generated randomness.

======================================================================
3. What the System Does
======================================================================

3.1 User Experience — One Complete Roll

A user accesses the system through a web-based interface on any device with
a browser. Before initiating a session, the user configures the dice type
or combination to be used, reflecting the physical dice currently loaded in
the hardware device. If the application requires it, the user may also
establish named participants for the session.

The user initiates a roll by activating a control element in the web
interface. The system establishes a session token at this point, which
persists for the duration of the session and binds all subsequent roll data
to that user. The request is transmitted to the central API along with the
user's dice configuration.

The API identifies the appropriate worker node based on the physical dice
configuration currently loaded in each available device. If the target
hardware is in use, the request is queued until the device is available.
Once a compatible worker node is free, the API forwards the roll request.

The worker node physically actuates the dice — a motor agitates the
container holding the physical dice until they have been sufficiently
randomized and come to rest. The camera then captures a still photograph
and, in supported configurations, a video recording of the full actuation
event from motor start through dice settling.

The worker node transmits the captured media and associated metadata to the
central API. The API associates this data with the user's session token and
returns the result to the user's browser. The user sees a photograph of the
final dice position — the physical, real-world result of the roll — which
they may inspect and verify themselves without any intermediary
interpretation.

3.2 Session Operator Model

The current implementation designates a single session operator who
initiates rolls on behalf of all participants. The system is designed to
support a transfer mechanism allowing operator control to be passed to
another participant within the same session. All participants in a session
have access to the photographic record of each roll regardless of who holds
operator control. The architecture is designed to support independent
per-player roll initiation as a future capability.

3.3 Dice Configuration

The physical apparatus accepts any standard dice configuration. The
hardware imposes no limitation on die type, quantity, or combination. The
current reference implementation uses D4, D6, D8, D10, D12, D20, and D100.
Future configurations may include non-standard dice, custom-faced dice, or
any physical randomization object that fits within the container.

3.4 Verification Modalities

- Still photograph: automatically captured and delivered upon completion of
  every roll. Shows the final resting position of all dice.
- Video recording: captures the full actuation event from motor initiation
  through dice settling. Available as a requestable artifact. Provides
  continuous visual evidence of the randomization process.

======================================================================
4. Hardware Components
======================================================================

- Container: physical dice container (reference implementation: tin can)
- Motor driver: TB6612FNG dual H-bridge motor driver per can, connected via
  GPIO pins (AIN1, AIN2, PWM) for direction and speed control
- Motor: DC motor per can. Speed, shake duration, shake count, and settle
  time are independently configurable per can
- LED illumination: PCA9685 16-channel I2C LED controller with one
  dedicated channel per can. LED activates before camera capture with
  configurable flash_delay for consistent exposure
- Single-board computer: Raspberry Pi. Each worker node supports up to 8
  independent can slots
- Camera: USB webcam per can. Still JPEG captured using fswebcam after
  motor stops and dice settle
- Physical dice: any standard polyhedral dice. Mixed configurations
  supported (e.g. 2x D6, or D20 + D6 in same can)
- Network interface: wired ethernet (primary) or wireless

The hardware design is intentionally modular. No custom silicon or
proprietary hardware is required.

======================================================================
5. Software Pipeline
======================================================================

5.1 Three-Tier Architecture

- Tier 1 — Client: Browser-based frontend, any device
- Tier 2 — Central API: Session management, authentication, worker routing,
  hardware queue management
- Tier 3 — Worker Node: Raspberry Pi controlling GPIO motor driver, camera
  capture, and LED illumination per can

5.2 Roll Sequence

1.  User initiates roll via web interface
2.  Session token created or validated; all roll data bound to token
3.  Client transmits roll request and dice configuration to central API
4.  API consults live worker registry (populated by 30-second heartbeats)
    to identify matching worker node and can
5.  Threading lock prevents concurrent access to same physical can
6.  Worker receives roll request when can is available
7.  Motor runs for configured shake cycles via TB6612FNG GPIO driver
8.  Settle period allows dice to come to rest
9.  PCA9685 LED activates; flash_delay elapses; fswebcam captures JPEG
10. LED deactivates; photo encoded as base64 and returned to API
11. API stores photo, associates with session token, returns authenticated
    media URL to client
12. Client fetches photo with Bearer token and displays in browser
13. Roll appended to session log with timestamp, round, player, dice config

5.3 Session Token Ownership Model

All roll data is cryptographically bound to the session token. The API
enforces that data is accessible only to the token holder. Isolation is
enforced by design. Absolute security is not claimed.

======================================================================
6. Session Management
======================================================================

- Multi-participant session support with named players
- Single operator model with session transfer mechanism
- Round tracking — rolls tagged to round number, navigable history
- Scratch/void system — rolls marked void with reason, preserved not deleted
- PDF export — embedded photographs, round structure, timestamps, void stamps
- Session expiry — idle timeout and hard maximum duration
- Media grace period — URLs valid after session end for PDF download

======================================================================
7. Authentication & Security
======================================================================

Session tokens are HMAC-SHA256 signed with server-side secret and carry
expiry timestamps. All endpoints require Bearer token. Media URLs are
authenticated — not publicly accessible. After grace period, media is
purged from server.

======================================================================
8. Badge & Reward System
======================================================================

16-bit bitfield encoding scheme. XOR scrambling with fixed key,
multiplicative hashing with large prime, application-specific salt,
checksum verification. 29-character human-readable alphabet excluding
ambiguous characters (0, O, 1, I, L). Format: XXXX-XXXX. Supports up to
16 badge types in a single portable string requiring no server-side storage.

======================================================================
9. Claims of Novelty
======================================================================

The following combinations are believed to be novel as of March 2026:

- Physical actuation of real dice by motorized mechanism, combined with
  photographic and video capture, delivered as authenticated record to
  remote users over the internet — as a unified system
- User as their own auditor — photographic record requires no mathematical
  literacy, cryptographic knowledge, or trusted third party to verify
- Configuration-aware worker routing — API matches requests to worker nodes
  by physical dice configuration
- Hardware queue management — threading lock per can, sequential processing
- Session token ownership — all roll data cryptographically bound to user
- Multi-modal verification — still photograph and video, both authenticated
- Session operator transfer mechanism — control passable between participants
- Session-linked exportable audit trail — PDF with embedded photos and
  complete roll record
- Dice-agnostic physical hardware — any dice configuration, no hardware
  modification required
- Per-can independent hardware tuning — motor speed, shake count, settle
  time, LED channel all independently configurable per can
- Worker self-registration via heartbeat — live registry of available
  hardware without manual configuration
- LED-synchronized camera capture — dedicated LED per can with configurable
  timing for consistent exposure
- Up to 8 independent physical cans per worker node

======================================================================
10. Prior Art Acknowledged
======================================================================

US20230306815A1 — Casino player performance comparison. No camera, no
physical dice, no remote delivery. No conflict.

US11887441B2 (Interblock) — Hybrid craps table. Camera-based recognition
in dependent claims only, all requiring casino wagering infrastructure not
present in this system. No conflict.

US8758109 (CFPH) — Casino slow-motion camera replay at local stations.
No internet delivery, no remote actuation. No conflict.

US10535230 — Cryptographic provably fair RNG. No physical dice, no camera,
no hardware. Verification is cryptographic; this system's is photographic.
No conflict.

US20200302744A1 — GPS-based betting on casino player performance. No
physical dice, no camera, no remote rolling. No conflict.

Roulette aggregation sensing patent — Camera detects roulette outcomes for
set-based wagering. Casino context, no physical dice shaker, no internet
delivery. No conflict.

======================================================================
11. Intentional Omissions
======================================================================

THE FOLLOWING FEATURE IS INTENTIONALLY OMITTED AND WITHHELD PENDING
PROVISIONAL PATENT FILING:

Automated image-based extraction of dice face values from captured
photographs and/or video frames, including any combination of computer
vision, machine learning, frame selection algorithms, or image analysis
techniques used to suggest or confirm the numerical outcome of a physical
dice roll. This feature — and any combination of it with the physical
pipeline described herein — is not disclosed and is subject to a pending
or forthcoming patent application.

The existence of this omission is disclosed here to establish that the
inventor was aware of and had conceived of this capability at the time of
this publication, and elected to withhold it for patent protection purposes.

======================================================================
12. Publication Record
======================================================================

