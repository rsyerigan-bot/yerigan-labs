Lessons:

Validate every layer independently.
Never assume the configuration file is the active source of truth.
Read logs.
Follow evidence.
Keep narrowing possibilities.

Problem

Home Assistant returned HTTP 400 through Caddy despite correct reverse proxy configuration.

Investigation
Verified firewall
Verified Docker networking
Verified Caddy upstream connectivity
Validated YAML
Confirmed mounted configuration
Reviewed logs
Identified persistent .storage configuration
Root Cause

Home Assistant had migrated HTTP configuration into persistent storage after first-run setup. Subsequent YAML changes were ignored.

Engineering Takeaway

Always identify the application's active source of truth before modifying configuration.

That last sentence is one I think you'll remember for years.