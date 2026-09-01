# Journey Keepalive

Pings [`https://journey-xvdw.onrender.com`](https://journey-xvdw.onrender.com) every 12 minutes so the
Render free-tier service doesn't spin down from inactivity, and publishes a small status page over
GitHub Pages showing the result of the latest ping.

## How it works

- [`.github/workflows/keepalive.yml`](.github/workflows/keepalive.yml) runs on a GitHub Actions
  schedule (`*/12 * * * *`), issues an HTTP GET to the target URL, and commits the result
  (timestamp, HTTP status, latency) to [`status.json`](status.json).
- [`index.html`](index.html) is a static page (served by GitHub Pages) that reads `status.json`
  and displays the last ping time, HTTP status, and latency. It refreshes every 60 seconds.

This does **not** depend on anyone having the page open — the ping happens server-side on
GitHub's schedule regardless.

## Limits to know about

- GitHub Actions scheduled workflows can run a few minutes late under high platform load; a
  12-minute cadence is not millisecond-precise.
- GitHub disables a repository's scheduled workflows after **60 days with no repository
  activity**. Because this workflow commits `status.json` on every run, the repository stays
  active and the schedule keeps running indefinitely.
- You can also trigger a ping manually from the **Actions** tab (`Run workflow`).

## Changing the target URL

Edit `TARGET_URL` in `.github/workflows/keepalive.yml` (two places) and in `status.json`'s
initial `url` field if you want the placeholder to match before the first run.
