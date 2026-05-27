# Game Preservation Analysis Repository

## Overview

This repository contains long-term technical preservation research into modern video games, console ecosystems, launcher dependencies, DRM behavior, online service reliance, and offline survivability.

The purpose of this project is to document, analyze, and preserve evidence regarding how modern games behave when internet connectivity, authentication infrastructure, storefront ecosystems, or publisher-controlled services become unavailable.

This repository focuses on:

* evidence-based analysis
* reproducible testing
* packet-level network inspection
* offline survivability testing
* platform dependency mapping
* preservation impact analysis

The project intentionally avoids speculation and unsupported claims.

All findings are derived from directly observable technical behavior.

---

# Project Goals

Modern games increasingly depend on:

* online authentication
* launcher ecosystems
* cloud infrastructure
* CDN distribution
* telemetry systems
* entitlement validation
* live-service architecture
* platform APIs
* account ecosystems

This project attempts to answer questions such as:

```text id="pbmy42"
Can a game still install years later if platform services disappear?
Can a physical disc function fully offline?
What infrastructure is actually required?
What behavior is platform-level versus game-level?
Does a game degrade gracefully offline or hard-fail?
```

---

# Research Areas

The repository investigates:

* offline installation behavior
* offline startup behavior
* retry/timeout handling
* entitlement dependency
* launcher dependency
* telemetry infrastructure
* CDN dependency
* account dependency
* remote configuration systems
* platform service reliance
* patch dependency
* long-term preservation survivability

---

# Platforms

Current and future testing targets include:

## Consoles

* PlayStation 5
* PlayStation 4
* Xbox Series X/S
* Xbox One
* Nintendo Switch
* Future console platforms

## PC Ecosystems

* Steam
* Epic Games Store
* EA App
* Ubisoft Connect
* Battle.net
* Rockstar Launcher
* Microsoft Store/Xbox PC

---

# Methodology

Testing is performed using controlled and reproducible scenarios.

Typical test workflows include:

* fully online baseline startup
* fully offline startup
* offline installation testing
* reconnect testing
* DNS-only environments
* gateway-only environments
* internet isolation
* future-date manipulation
* retry/timeout observation
* packet capture analysis
* endpoint blocking
* differential boot analysis

---

# Network Analysis

This repository heavily uses network traffic analysis to determine:

* what systems games contact
* when they contact them
* what happens when services are unavailable
* how retry logic behaves
* whether fallback behavior exists

Tools commonly used:

* tcpdump
* Wireshark
* DNS analysis
* TLS/SNI analysis
* retransmission analysis
* timing analysis

Important:

```text id="vlywhk"
Encrypted TLS payloads are not treated as known application behavior unless directly observable.
```

The project distinguishes between:

* confirmed technical observations
* reasonable evidence-based interpretation
* unsupported speculation

---

# Evidence Standards

## Confirmed Behavior

Only directly observable behavior is considered confirmed.

Examples:

* DNS queries
* TCP connection attempts
* startup timing
* installation progression
* successful/failed handshakes
* retry behavior
* offline fallback behavior

---

## Interpretation

Interpretation is only made when supported by evidence from:

* packet captures
* startup behavior
* installation behavior
* retry timing
* observed platform communication

Interpretation is clearly separated from confirmed facts.

---

## No Unsupported Claims

This repository intentionally avoids unsupported conclusions such as:

* “always-online DRM”
* “telemetry-required gameplay”
* “forced cloud gaming”
* “mandatory account lockout”

unless directly demonstrated by evidence.

---

# Preservation Philosophy

Modern games are increasingly dependent on infrastructure outside the physical media itself.

This project attempts to document where the actual preservation boundary exists.

Examples:

| Traditional Preservation | Modern Preservation                               |
| ------------------------ | ------------------------------------------------- |
| Preserve disc/cartridge  | Preserve servers + APIs + platform infrastructure |
| Offline ownership        | Infrastructure-assisted ownership                 |
| Self-contained software  | Distributed service ecosystems                    |

The repository aims to document:

* what survives offline
* what degrades offline
* what permanently fails offline
* what depends on disappearing infrastructure

---

# Repository Structure

Example structure:

```text id="3dy5ja"
Captures/
├── Online/
├── Offline/
├── Install/
├── Differential/

Reports/
Markdown/
Screenshots/
Analysis/
Documentation/
Tools/
Notes/
```

---

# Current Focus

The initial phase of this project currently focuses on:

* PlayStation 5
* physical media
* offline installation behavior
* startup dependency analysis
* packet-level preservation evidence

---

# Long-Term Goals

Future goals include:

* cross-platform comparisons
* preservation scoring
* launcher survivability analysis
* dependency cataloging
* CDN mapping
* platform behavior analysis
* sunset behavior tracking
* server shutdown simulation
* historical preservation snapshots

---

# Neutrality Statement

This repository is not intended as:

* anti-DRM activism
* publisher harassment
* piracy advocacy
* outrage content

The objective is technical documentation and preservation analysis.

The repository documents:

* what games do
* what systems they contact
* what happens when services disappear
* whether meaningful offline survivability exists

---

# Legal / Ethical Scope

All testing is performed using:

* legally obtained hardware
* legally obtained software
* personal network infrastructure

No attempt is made to:

* bypass DRM
* reverse engineer proprietary game code
* attack services
* interfere with online infrastructure

---

# Guiding Principle

```text id="94k9de"
If a game depends on external infrastructure to remain usable,
then that infrastructure becomes part of the preservation boundary.
```
