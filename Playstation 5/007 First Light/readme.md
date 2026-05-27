# 007 First Light Game Preservation Analysis

## Overview

This repository documents a long-term technical preservation research project focused on modern video games, console ecosystems, launcher dependencies, DRM behavior, and offline survivability.

The primary goal is to determine:

```text
Can modern games still be installed, launched, and played if internet connectivity or platform infrastructure disappears?
```

This project focuses on evidence-based technical analysis rather than speculation, outrage, or opinion-based commentary.

All findings are based on:

* direct gameplay testing
* offline boot/install testing
* packet captures
* DNS analysis
* retry/timeout behavior
* platform dependency analysis
* reproducible network isolation testing

---

# Project Goals

This project investigates:

* offline installation behavior
* offline boot capability
* launcher dependency
* platform dependency
* account dependency
* cloud dependency
* telemetry behavior
* CDN dependency
* entitlement validation
* retry/timeout handling
* long-term preservation survivability

The research aims to answer questions such as:

* Can a physical disc still function years later?
* What happens if authentication servers disappear?
* Does a game actually require internet or simply prefer it?
* Which dependencies are platform-level versus game-level?
* Are modern games preservation-safe?
* Which companies implement graceful offline fallback behavior?
* Which systems degrade versus fully fail?

---

# Scope

This project is not limited to a single game.

Current testing focuses on:

* PlayStation 5
* physical disc media
* modern AAA titles

Future testing will expand to:

* PlayStation 4
* Xbox Series X/S
* Xbox One
* Nintendo Switch
* PC storefronts
* Steam
* Epic Games Store
* EA App
* Ubisoft Connect
* Battle.net
* DRM middleware
* launcher ecosystems

Additional games and platforms will continuously be added over time.

---

# Methodology

The project uses controlled and reproducible testing methodologies.

Testing scenarios include:

* fully online baseline startup
* fully offline startup
* offline installation
* post-activation offline startup
* internet isolation
* DNS-only environments
* gateway-only connectivity
* future-date manipulation
* reconnect testing
* retry/timeout observation
* packet capture analysis

---

# Network Analysis

The repository contains:

* packet captures (`.pcap`)
* DNS observations
* endpoint analysis
* retry behavior analysis
* CDN observations
* PlayStation platform communication analysis
* telemetry observations

Packet captures are analyzed using:

* `tcpdump`
* Wireshark
* DNS analysis
* TCP retransmission analysis
* TLS/SNI analysis where possible

Important:

```text
TLS payloads are encrypted.
This project does not claim decrypted application behavior unless directly observable.
```

---

# Evidence Standards

This repository follows strict evidence standards.

## Confirmed Behavior

Only directly observable behavior is treated as confirmed.

Examples:

* DNS queries
* connection attempts
* retry behavior
* startup duration
* install progression
* successful/failed boot behavior

---

## Reasonable Interpretation

Interpretation is only made when strongly supported by observed evidence.

Interpretations are clearly labeled and separated from confirmed facts.

---

## No Unsupported Claims

The project intentionally avoids unsupported claims such as:

* "always-online DRM"
* "telemetry-required gameplay"
* "mandatory account enforcement"

unless directly proven by evidence.

---

# Preservation Philosophy

This project focuses on:

* technical preservation
* ownership implications
* survivability of physical media
* long-term accessibility
* infrastructure dependency mapping

The objective is not to attack developers or publishers.

Instead, the goal is to document:

* how modern games behave
* what infrastructure they depend on
* what happens when services disappear
* whether graceful offline fallback exists

---

# Current Findings

Initial findings from 007 First Light testing indicate:

* the game eventually becomes playable offline
* startup behavior is heavily internet-first
* retry/timeout behavior is aggressive
* PlayStation platform infrastructure is heavily contacted during startup
* offline fallback behavior exists
* installation from disc eventually succeeds offline after extended delay

Importantly:

```text
The game did NOT exhibit immediate hard online lockout behavior in current testing.
```

However:

```text
The offline experience was severely degraded with startup/install delays exceeding 15 minutes.
```

---

# Repository Structure

Example layout:

```text
Captures/
Reports/
Markdown/
Screenshots/
Analysis/
Tests/
Tools/
Documentation/
```

---

# Future Research Areas

Planned future work includes:

* differential patch analysis
* platform-to-platform comparisons
* launcher survivability analysis
* CDN dependency mapping
* token expiration testing
* server shutdown simulation
* endpoint blocking analysis
* long-term offline survivability scoring

---

# Disclaimer

This repository is intended for:

* technical research
* preservation analysis
* interoperability analysis
* educational purposes

No attempt is made to bypass DRM, reverse engineer proprietary code, or interfere with online services.

All testing is performed on legally obtained hardware and software.

---

# Guiding Principle

```text
If a game can only survive while external infrastructure exists,
then the infrastructure itself becomes part of the game’s preservation boundary.
```
