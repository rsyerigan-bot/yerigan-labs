Problem

Each service was independently exposed using host ports.

This worked for a small lab but would not scale.

Decision

Introduce a reverse proxy before deploying additional services.

Rationale

A reverse proxy:

Reduces attack surface.
Simplifies user experience.
Centralizes routing.
Enables future HTTPS.
Provides infrastructure abstraction.
Lessons

This lab reinforced an important engineering principle:

Introduce abstraction before complexity.

Although only four services currently exist, implementing the reverse proxy now prevents future architectural debt.

Future Work
HTTPS
Internal DNS using Pi-hole
Authentication
Home Assistant
Vaultwarden
Nextcloud