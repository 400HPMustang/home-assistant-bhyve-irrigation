# Mobile Notifications

The sanitized package routes irrigation messages through one script:

```yaml
script.irrigation_notify
```

By default, that script creates a Home Assistant persistent notification. To use the Home Assistant Companion App instead, replace only the script body in `packages/irrigation_core.yaml`.

Example:

```yaml
  irrigation_notify:
    alias: Irrigation - Notification
    fields:
      title:
        description: Notification title
      message:
        description: Notification message
    sequence:
      - action: notify.mobile_app_your_phone
        data:
          title: "{{ title | default('Irrigation') }}"
          message: "{{ message | default('Irrigation update') }}"
          data:
            push:
              interruption-level: time-sensitive
            tag: irrigation_status
            group: irrigation
    mode: queued
    max: 20
```

Replace `notify.mobile_app_your_phone` with the actual notify action shown in Home Assistant Developer Tools.
