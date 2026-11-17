# Deployment — God's Eye View (paperlessblueprint fork)

Fork of the public repo `bilawalsidhu/gods-eye-view` (MIT). `origin` = `paperlessblueprint/gods-eye-view`,
`upstream` = the original. Pull upstream changes with `git fetch upstream && git merge upstream/main`.

## Cloud-agent
- Path: `/home/agent/projects/godseyeview`
- Preview: https://godseyeview.dev.getmysitelink.com (owner login required), proxied with
  `sudo preview enable godseyeview --port 5180`
- Runs the Vite dev server (`npm run dev`) on `127.0.0.1:5180`, started by `~/.local/bin/godseyeview-preview`
  (env: `HOST`, `PORT=5180`, `GEV_ALLOWED_HOSTS=godseyeview.dev.getmysitelink.com`, `GEV_RATELIMIT_GOOGLE_PER_MIN=60`).
- Kept alive by agent's crontab (every minute + `@reboot`); the script exits if port 5180 is already listening.
  Log: `~/logs/godseyeview-preview.log`. Restart: `pkill -f "projects/godseyeview.*vite"` (cron restarts it within a minute).
- Linger is not enabled for `agent`, so a systemd user service would not survive logout — hence the cron watchdog.
- Database: none. Secrets: none — the app runs keyless (Esri imagery, OpenSky, Launch Library). No `.allsecrets` store,
  no `.cloud-keys`. Keys added through the in-app POWER UP panel are written to the gitignored `.env` on cloud-agent;
  use only restricted/test keys there.
- `npm ci` leaves esbuild/puppeteer/vue-demi install scripts unapproved (npm 11 default); not needed to run the app.

## Production
None. This is a local/self-hosted app; there is no production target and nothing registered in `prod`.
