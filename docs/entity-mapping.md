# Entity Mapping

Replace the placeholders below with entities from the target Home Assistant installation.

## Required entities

| Placeholder | Purpose |
|---|---|
| `weather.home` | Weather entity used for hourly and daily forecasts |
| `valve.lawn_zone_1` | First lawn watering zone |
| `valve.lawn_zone_2` | Second lawn watering zone |
| `valve.lawn_zone_3` | Third lawn watering zone |
| `valve.plant_zone_1` | First plant, bed, or micro-sprayer zone |
| `valve.plant_zone_2` | Second plant, bed, or micro-sprayer zone |
| `valve.plant_zone_3` | Third plant, bed, or micro-sprayer zone |
| `valve.plant_zone_4` | Fourth plant, bed, or micro-sprayer zone |
| `valve.manual_mist_zone` | Manual-only mist zone |

## Optional dashboard history sensors

The B-hyve integration may create watering-history sensors using names derived from each zone and device. Replace these placeholders or remove the rows from the dashboard.

```text
sensor.lawn_zone_1_history
sensor.lawn_zone_2_history
sensor.lawn_zone_3_history
sensor.plant_zone_1_history
sensor.plant_zone_2_history
sensor.plant_zone_3_history
sensor.plant_zone_4_history
sensor.manual_mist_zone_history
```

## Optional dashboard battery sensors

Replace these with the controller or hose-timer battery sensors created by the integration, or remove the battery card.

```text
sensor.irrigation_controller_1_battery
sensor.irrigation_controller_2_battery
sensor.irrigation_controller_3_battery
sensor.irrigation_controller_4_battery
```

## Optional dashboard fault sensors

Replace these with the relevant B-hyve station or controller fault sensors, or remove the fault card.

```text
binary_sensor.irrigation_controller_1_fault
binary_sensor.irrigation_controller_2_fault
binary_sensor.irrigation_controller_3_fault
binary_sensor.irrigation_controller_4_fault
```

## Search command

After editing, search the repository for remaining placeholders:

```bash
grep -RniE 'weather\.home|valve\.(lawn|plant|manual_mist)_zone|irrigation_controller_[1-4]' .
```
