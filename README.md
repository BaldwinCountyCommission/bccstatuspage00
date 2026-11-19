# Baldwin County Service Status

The status page (`index.html`) reads **`status.json`** from this repository every minute and shows it in the baldwincountyal.gov theme. To change what the public sees, edit `status.json` and commit. No rebuild is needed.

Visitors see changes within about **5 minutes**, which is how long GitHub caches raw files.

## Updating status from the GitHub website

1. Open `status.json` and click the pencil icon (**Edit this file**).
2. Change the `status` of the affected service(s), and add an incident if it helps (see below).
3. Set `updated` to the current time. Write times either in UTC (`2026-11-19T14:45:00Z`) or in Central time with its offset (`2026-11-19T08:45:00-06:00`, or `-05:00` during daylight saving). The page shows them in Central time.
4. Click **Commit changes**.

If the file has a JSON error (a missing comma or quote, for example), the page says "We couldn't load the latest status right now." Paste the file into a JSON validator to find the problem.

## status.json format

```json
{
  "updated": "2026-11-19T14:45:00Z",
  "notice": "Optional message shown at the top of the page. Leave as \"\" for none.",
  "services": [
    { "group": "Online Services", "name": "Online Payments", "description": "Taxes, fees and bills paid online",
      "status": "operational", "note": "Optional note shown under this service." }
  ],
  "incidents": [],
  "maintenance": []
}
```

### Service `status` values

| Value | Shown as |
|---|---|
| `operational` | Operational (green) |
| `degraded` | Slower than usual (amber) |
| `partial_outage` | Partial outage (orange) |
| `major_outage` | Outage (red) |
| `maintenance` | Maintenance (navy) |

Services are grouped under their `group` name, in the order they appear in the file. The headline at the top of the page reflects the worst status on the page.

### Incidents

Add one object per incident. While `resolved` is empty, it shows under **Current Incidents**. Once `resolved` has a time, it moves to **Past Incidents** for 14 days.

```json
{
  "title": "Online payments failing",
  "impact": "partial_outage",
  "status": "identified",
  "affected": ["Online Payments"],
  "started": "2026-11-19T14:10:00Z",
  "resolved": "",
  "updates": [
    { "time": "2026-11-19T14:30:00Z", "status": "identified", "message": "Our payment provider is having an outage. We're working with them on a fix." },
    { "time": "2026-11-19T14:12:00Z", "status": "investigating", "message": "We're looking into reports that online payments aren't going through." }
  ]
}
```

- `impact`: one of the service status values above.
- `status`: `investigating`, `identified`, `monitoring` or `resolved`.
- `updates`: any order. The page shows newest first.

### Scheduled maintenance

Shown until its `end` time passes.

```json
{
  "title": "Permit portal upgrade",
  "affected": ["Permits & Inspections Portal"],
  "start": "2026-11-22T12:00:00Z",
  "end": "2026-11-22T16:00:00Z",
  "message": "The permit portal will be offline while we install an upgrade."
}
```

## Hosting the page

`index.html` can be hosted anywhere. It always loads `status.json` from this repository. Host it **somewhere other than the county web servers** (GitHub Pages works: Settings → Pages → deploy from `main`), so the status page stays up when the main site is down.

The repository must stay **public**: the page reads the file without signing in.
