# Home Assistant B-hyve Irrigation

A reusable Home Assistant irrigation package built around the Orbit B-hyve custom integration.

The configuration provides:

- Fully sequential watering so two zones are never intentionally started at once
- Separate lawn and plant cycles
- A master enable and manual lockout
- Weather-based rain skipping
- A configurable hot-day plant boost
- Same-day automatic catch-up for plant zones after a weather skip
- Manual lawn catch-up by design
- A manual mist zone
- Desktop and mobile dashboard layouts
- Runtime, schedule, battery, fault, and recent-run dashboard sections

## Important

This repository is an example configuration, not a drop-in universal irrigation controller. Replace all placeholder entities and review every runtime and threshold before enabling automatic watering.

The default rain thresholds use inches and the default temperature thresholds use Fahrenheit. Change the units and values if your Home Assistant weather provider uses different units.

## Requirements

- Home Assistant with YAML packages enabled
- Orbit B-hyve custom integration by `sebr/bhyve-home-assistant`
- B-hyve zone entities exposed as `valve.*`
- A Home Assistant `weather.*` entity supporting hourly and daily forecasts
- Optional: `card-mod` for the dashboard's rounded-card styling

## Repository layout

```text
.
├── configuration.yaml.example
├── packages
│   ├── irrigation_core.yaml
│   └── irrigation_catchup.yaml
├── dashboards
│   └── irrigation_dashboard.yaml
└── docs
    ├── entity-mapping.md
    ├── mobile-notifications.md
    ├── sanitization.md
    └── validation.md
```

## Installation

### 1. Install and configure B-hyve

Install the Orbit B-hyve integration, configure it in Home Assistant, and confirm that each watering zone appears as a `valve` entity.

Disable B-hyve app schedules and Smart Watering for any zones Home Assistant will control. This configuration is intended to be the scheduler of record.

### 2. Enable packages

Copy the `packages` folder into `/config/packages` and add this to `configuration.yaml`:

```yaml
homeassistant:
  packages: !include_dir_named packages
```

Merge it under an existing `homeassistant:` key rather than creating a second one.

### 3. Replace placeholder entities

At minimum, replace these placeholders in both package and dashboard files:

```text
weather.home
valve.lawn_zone_1
valve.lawn_zone_2
valve.lawn_zone_3
valve.plant_zone_1
valve.plant_zone_2
valve.plant_zone_3
valve.plant_zone_4
valve.manual_mist_zone
```

See [`docs/entity-mapping.md`](docs/entity-mapping.md) for every optional history, battery, and fault placeholder used by the dashboard.

### 4. Review defaults

The included defaults are:

| Item | Default |
|---|---:|
| Lawn schedule | Monday / Wednesday / Saturday at 5:00 AM |
| Plant schedule | Daily at 6:15 AM |
| Hot-day check | Daily at 6:30 PM |
| Lawn zone runtimes | 30 / 22 / 18 minutes |
| Plant zone runtimes | 10 / 12 / 12 / 7 minutes |
| Manual mist | 3 minutes |
| Rain skip threshold | 0.13 in remaining today |
| Plant catch-up threshold | 0.25 in remaining today |
| Hot day threshold | 85 °F |
| Very hot day threshold | 90 °F |

Change these values for the landscape, sprinkler hardware, soil, weather provider, and local watering restrictions.

### 5. Validate and restart

Run Home Assistant's configuration check before restarting. After restart, confirm that the helpers, scripts, template sensors, and automations were created.

### 6. Import the dashboard

Create a new dashboard or view, open the raw configuration editor, and paste the contents of `dashboards/irrigation_dashboard.yaml`.

The file contains one complete `views:` block. When pasting into an existing dashboard, copy only the view item under `views:` as appropriate.

## Behavior

### Automatic lawn cycle

The lawn cycle runs Monday, Wednesday, and Saturday. It refreshes the weather forecast before watering and then runs the three lawn zones sequentially.

### Automatic plant cycle

The plant cycle runs daily and sequentially waters the four plant zones.

### Weather skip

The weather update automation retrieves hourly and daily forecast data, calculates the forecast high and remaining precipitation for the current day, and sets the weather skip flag when the configured rain threshold is met.

### Plant catch-up

When the scheduled plant cycle is blocked by weather, the catch-up package records the skip. Between 10:00 AM and 6:00 PM it checks every 30 minutes. If the weather gate clears and remaining forecast rain drops below the catch-up threshold, it runs the plant cycle once.

### Lawn catch-up

Lawn catch-up is deliberately manual. The dashboard reports that the lawn cycle was skipped, but it does not automatically run a long lawn cycle later in the day.

### Manual watering

The manual quick-run buttons call `script.irrigation_run_zone_ignore_weather`. These runs ignore the weather skip but still honor the master enable and manual lockout.

### Manual mist

Turning on `input_boolean.irrigation_manual_mist_enable` runs the mist zone for the configured runtime and then turns the helper back off.

## Notifications

The sanitized package uses a built-in persistent notification through `script.irrigation_notify`, so it contains no personal mobile-device service name. See [`docs/mobile-notifications.md`](docs/mobile-notifications.md) to swap that one script to a mobile notification service.

## Safety notes

- Test each individual zone before enabling schedules.
- Confirm the zone entity mapping carefully.
- Keep the manual lockout on while editing or testing.
- The `Stop All` script should be tested before unattended use.
- Home Assistant, the B-hyve cloud service, Wi-Fi, and the controllers must all be available for cloud-controlled watering.

## License

MIT

## References

- Home Assistant configuration packages: https://www.home-assistant.io/docs/configuration/packages/
- Home Assistant weather forecasts: https://www.home-assistant.io/actions/weather.get_forecasts/
- Orbit B-hyve custom integration: https://github.com/sebr/bhyve-home-assistant
