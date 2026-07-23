# Sanitization Summary

The shareable files use generic placeholders rather than installation-specific identifiers.

Removed or generalized:

- Location-specific weather entity
- Original B-hyve controller and zone names
- Original lawn, bed, garden, pot, and mist zone names
- Personal mobile notification action
- Controller-specific history, battery, and fault entity names
- Any user name or postal-code-derived identifier

Preserved:

- Sequential watering design
- Master enable, lockout, and weather gates
- Lawn, plant, heat-boost, and manual-mist workflows
- Original default schedule pattern and runtime values
- Same-day automatic plant catch-up behavior
- Manual-only lawn catch-up decision
- Desktop and mobile dashboard structure

The original core package was not available as a preserved standalone file. The core in this repository was reconstructed from the preserved dashboard and catch-up package plus the previously established schedules, runtimes, zone order, and weather logic.
