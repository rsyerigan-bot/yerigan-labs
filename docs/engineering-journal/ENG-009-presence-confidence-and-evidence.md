# ENG-009 — Presence Confidence and Evidence

## Context

LAB-009 explored identity, presence, and context within Home Assistant.

The initial goal was to determine whether a person was home using more than a single device state.

## Design

The first implementation separated raw evidence from the final presence inference.

Evidence sources:

- GPS-based Person state
- iPhone-reported Wi-Fi SSID

Derived states:

- Confirmed Home
- Likely Home
- Conflicting Evidence
- Away

GPS was treated as the primary location signal.

Wi-Fi association was treated as corroborating evidence rather than an equally authoritative signal.

## Key Finding

The phone-reported SSID was not operationally independent.

When Wi-Fi was disabled on the iPhone, the phone could no longer reach the local-only Home Assistant server over cellular.

As a result, Home Assistant retained the previous SSID value rather than immediately receiving the disconnected state.

This created stale telemetry.

## Architectural Decision

Phone-side SSID remains useful as positive corroborating evidence but should not be trusted as the authoritative network-presence source.

The permanent design will use network-side observation from the router or access point after migration to the new home network.

## Lessons Learned

- Raw signals are evidence rather than truth.
- Missing evidence is different from contradictory evidence.
- Evidence freshness matters.
- A valid-looking sensor value may still be stale.
- Two signals that appear independent in a diagram may share hidden dependencies.
- Failure testing is necessary to reveal real architecture.
- Inference should remain separate from raw telemetry.

## Future Improvements

- Add router-side device presence.
- Explicitly represent unavailable evidence.
- Track evidence freshness.
- Test every input combination.
- Add additional evidence sources such as Bluetooth, motion, and local AI detection.
- Define safe behavior for security-sensitive automations.
