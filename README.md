# Blueprint - Frigate Telegram Notification
An Home Assistant Blueprint to receive Snapshots and Clips from Frigate

Add this url to your Blueprints projects:
https://gist.github.com/freemandigger/6507b65820e88a716eaee01eef28f116

The same blueprint is kept in this repo: [frigate_telegram_notification.yaml](frigate_telegram_notification.yaml)

Fork of [NdR91/Blueprint_Frigate-Telegram-Notification](https://github.com/NdR91/Blueprint_Frigate-Telegram-Notification).

## Changes in this fork

- **Clips are no longer cut off at the detection moment.** The clip is requested 30 s after the `end` event,
  so Frigate has time to write the last recording segment to its database. Requested right away,
  `/api/events/<id>/clip.mp4` silently returns only the segments already stored, which often ends near the detection.
- **5 s of padding** around the clip (`clip.mp4?padding=5`).
- **`mode: parallel`** instead of `single`, so a long event (e.g. a parked car) no longer blocks notifications
  for other objects. Each object is notified once: on `new`, or on the first update where it enters one of the
  selected zones.
- Uses `chat_id` and the current Frigate integration notification URLs.

Recommended Frigate setting so the padded tail is actually kept (Frigate 0.14+):

```yaml
record:
  alerts:
    retain:
      mode: all   # active_objects drops segments once the object has left or stopped
```
