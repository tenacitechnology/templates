# Tenaci Templates / Portfolio

Source of truth for client showcase sites (private repo: `tenacitechnology/templates`).
Each folder is self-contained (`index.html` + `assets/`) and deploys standalone.

## Sites
- `j4v-top-coach/` — Live: https://tenaci-tech.com/portfolio/j4v-top-coach/

## Conventions
- One folder per site, deployed to `/var/www/portfolio/<name>/` on the tenaci-tech.com VPS
- nginx pattern: one `location` alias block per site in `/etc/nginx/sites-enabled/tenaci-tech.com`
- Deploy via SFTP; always back up the nginx config before editing, `nginx -t` before reload
