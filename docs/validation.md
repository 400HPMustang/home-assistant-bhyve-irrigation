# Validation Notes

The files in this repository were checked for:

- Valid YAML parsing
- Removal of the original location-specific weather entity
- Removal of personal mobile notification service names
- Removal of the original B-hyve device and zone entity IDs
- Consistent generic helper, script, valve, history, battery, and fault names

The package has not been loaded into a live Home Assistant instance in this sanitized form. Home Assistant's configuration check and a controlled zone-by-zone test are still required.

The core package was reconstructed from the preserved dashboard, preserved catch-up package, prior schedule/runtime decisions, and referenced scripts/entities. The catch-up behavior and dashboard structure were directly preserved and sanitized; the reconstructed core should be reviewed especially carefully before unattended use.
