# Face Off!

**Multiplayer Family Feud-style party game for Android** — real friends, live buzzer races, persistent wallets, and production-ready monetization (AdMob + Google Play Billing).

This is the cleaned, production-oriented version of the Face Off codebase (v2.0).

## Features that make money

- **AdMob** — Banner + Interstitial + Rewarded ads (server-side verification / SSV so clients cannot fake rewards)
- **In-app purchases** — `remove_ads` and `question_pack_1` (verified server-side against Google Play)
- **Persistent coin wallets** + secure wager settlement (authoritative backend)
- Private rooms + public matchmaking, host migration, reconnection
- Sound effects + theme music, Compose UI, modern Android (minSdk 24, target 35)

## Project structure

```
family-feud-app/
├── android/          # Kotlin + Jetpack Compose client
├── backend/          # Node.js + Socket.IO + PostgreSQL + Redis authoritative server
├── docker-compose.yml
├── PRODUCTION_CHECKLIST.md
└── BRAND_AND_PRODUCT_VISION.md
```

## Quick start (local)

### Backend
```bash
cd backend
cp .env.example .env          # edit secrets
npm install
# Optional: docker-compose up -d  (Postgres + Redis)
npm run dev
```

### Android
1. Open the `android/` folder in Android Studio.
2. Set environment variables or edit `app/build.gradle.kts` for local server URL.
3. Run on emulator (uses `http://10.0.2.2:3000` by default) or device.

## Environment variables (deployment)

### Backend (required for production)

| Variable | Purpose |
|----------|---------|
| `NODE_ENV` | `production` |
| `PORT` | Usually `3000` (or platform default) |
| `JWT_SECRET` | Long random secret for auth tokens |
| `CORS_ORIGIN` | Your allowed origin(s), comma-separated |
| `DATABASE_URL` | `postgresql://user:pass@host:5432/faceoff` |
| `DATABASE_SSL` | `true` on most cloud providers |
| `REDIS_URL` | Optional but recommended for multi-instance: `redis://...` |
| `GOOGLE_PLAY_PACKAGE_NAME` | `com.familyfeud.app` (must match your app) |
| `GOOGLE_PLAY_SERVICE_ACCOUNT_JSON` | Full JSON of a Play Console service account that has **Android Publisher** permission |
| `ALLOW_UNVERIFIED_PURCHASES` | **Always `false` in production** |

AdMob SSV callback must be publicly reachable at:
`https://YOUR_API_DOMAIN/api/admob/ssv`

### Android build-time (set as env vars or in CI)

| Variable | Purpose |
|----------|---------|
| `FACEOFF_SERVER_URL` | Production backend, e.g. `https://api.yourdomain.com` (must be `wss`/`https`) |
| `ADMOB_APP_ID` | Your real AdMob App ID (`ca-app-pub-xxxx~yyyy`) |
| `ADMOB_BANNER_ID` | Banner unit ID |
| `ADMOB_INTERSTITIAL_ID` | Interstitial unit ID |
| `ADMOB_REWARDED_ID` | Rewarded unit ID |

Debug builds fall back to Google’s test ad IDs.

## Google Play Store checklist (money path)

1. Create app in Play Console with package `com.familyfeud.app` (or change package everywhere).
2. Create in-app products: `remove_ads` (managed product) and any question packs.
3. Link a service account → download JSON → put into `GOOGLE_PLAY_SERVICE_ACCOUNT_JSON`.
4. Set up AdMob app + ad units; enable Server-Side Verification and point to your `/api/admob/ssv`.
5. Generate a signed release AAB (`./gradlew bundleRelease`).
6. Internal testing → closed → production.
7. Privacy policy, data safety form, content rating, store listing (icon, feature graphic, screenshots).
8. Follow `PRODUCTION_CHECKLIST.md` before going live.

## Security notes

- Never commit real `.env`, service-account JSON, or keystores.
- All coin awards and purchases are verified on the server.
- Clients cannot grant themselves rewards.

## License / notes

Questions bank is intentionally left as-is (not replaced with a regional set). Brand assets are placeholders — replace launcher icon and feature graphic before store submission.

See `BRAND_AND_PRODUCT_VISION.md` and `PRODUCTION_CHECKLIST.md` for the full product vision and launch checklist.
