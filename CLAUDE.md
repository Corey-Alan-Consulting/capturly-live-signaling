# capturly-live-signaling

Capturly Live's WebSocket signaling server: a fork of Steve Seguin's
websocket server variants for VDO.Ninja-style apps, deployed as a container
(`Dockerfile`) behind `live.capturly.app`.

<!-- SYNCED FROM framework/agents/base.md -->
<!-- END SYNC: framework/agents/base.md -->

## Layout

- `server.js` — basic fan-out server (generic, personal use only).
- `vdoninja.js` — the VDO.Ninja-optimized router (`npm start`).
- `vdoninja_advanced.js` — multi-room, stream-ownership aware router (AGPLv3).
- npm, no lint or test scripts; `npm start`, `npm run start:basic`, `npm run start:advanced`.

## Rules of this repo

- Upstream fork: keep changes to deployment and configuration; routing changes go
  upstream first. Licence terms (AGPLv3 for the advanced server) stay intact.
- TLS comes from `SERVER_CERT`/`SERVER_KEY` (or `CERT_PATH`/`KEY_PATH`); never
  commit certificate material.
