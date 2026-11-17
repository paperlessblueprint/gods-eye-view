# Deployment — God's Eye View (paperlessblueprint fork)

Fork of the public repo `bilawalsidhu/gods-eye-view` (MIT). `origin` = `paperlessblueprint/gods-eye-view`,
`upstream` = the original. Pull upstream changes with `git fetch upstream && git merge upstream/main`.

## Cloud-agent
- Path: `/home/agent/projects/godseyeview`
- Preview: https://godseyeview.dev.getmysitelink.com (owner login required), proxied with
  `sudo preview enable godseyeview --port 5180`
- Serves the **built** app with `vite preview` on `127.0.0.1:5180` (started by `~/.local/bin/godseyeview-preview`;
  env: `HOST`, `GEV_ALLOWED_HOSTS=godseyeview.dev.getmysitelink.com`, `GEV_RATELIMIT_GOOGLE_PER_MIN=60`).
  **Not** `npm run dev`: the dev server loads modules from `/node_modules/...` URLs, which the preview proxy blocks
  (503), so the page hangs on "Initializing photorealistic world…".
- After pulling changes: `npm ci && npm run build`, then kill the `vite preview` process (cron restarts it within a minute).
- Kept alive by agent's crontab (every minute + `@reboot`); the script exits if port 5180 is already listening and
  builds `dist/` first if it is missing. Log: `~/logs/godseyeview-preview.log`.
- Linger is not enabled for `agent`, so a systemd user service would not survive logout — hence the cron watchdog.
- Database: none. Secrets: none — the app runs keyless (Esri imagery, OpenSky, Launch Library). No `.allsecrets` store,
  no `.cloud-keys`. The in-app POWER UP key panel is disabled in `vite preview` mode: put keys in the gitignored `.env` on
  cloud-agent and rebuild (browser keys are baked in at build time). Use only restricted/test keys.
- `npm ci` leaves esbuild/puppeteer/vue-demi install scripts unapproved (npm 11 default); not needed to run the app.

## Production
None. This is a local/self-hosted app; there is no production target and nothing registered in `prod`.
