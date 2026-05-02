# Home Assistant Ventilation Reminder

Sends a push notification when opening a window would meaningfully lower indoor humidity. Uses psychrometric math to give accurate time estimates rather than just comparing raw humidity percentages.

> **Disclaimer:** This automation was developed through iterative AI prompting rather than rigorous engineering. The logic is sound in principle but I can't fully vouch for it in all conditions — treat it as a starting point and expect some tuning.

## How it works

Cold outdoor air holds less moisture than its RH% suggests. The automation converts indoor/outdoor readings to absolute humidity, calculates the lowest RH% achievable by ventilating (the "theoretical floor"), then uses an exponential decay model to estimate how many minutes it would take to reach your target.

Notification looks like:
> 💨 68% → 52%
> ~7 min → 50% • Outside 12.3°C

With action buttons: **Window opened** / **Too cold**

## Files

| File | Purpose |
|------|---------|
| `automation.yaml` | Main automation — triggers, conditions, notification |
| `binary_sensor_template.yaml` | Template sensor that calculates whether ventilating is worthwhile |
| `helpers.yaml` | `input_number` helpers for storing session humidity values |

## Setup

1. Replace all `YOUR_*` placeholders with your own sensor entity IDs
2. Replace `notify.YOUR_NOTIFY_TARGET` with your notification service
3. Replace `YOUR_PRESENCE_SENSOR` with your presence/person sensor
4. Add `helpers.yaml` contents under `input_number:` in your `configuration.yaml`
5. Add `binary_sensor_template.yaml` contents under `template:` > `binary_sensor:`
6. Reload HA or restart

## Requirements

- Indoor and outdoor temperature + humidity sensors (e.g. Ruuvi tags)
- A presence sensor for the room
- Mobile app for push notifications with actionable alerts

## Tuning

All parameters are at the top of each file under `TUNABLE PARAMETERS`. The defaults are calibrated for a typical European apartment.
