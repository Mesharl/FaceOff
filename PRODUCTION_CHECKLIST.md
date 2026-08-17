# Production Checklist

## Infrastructure
- [ ] Deploy backend with PostgreSQL + Redis
- [ ] Set all production environment variables (see README / .env.example)
- [ ] Configure TLS (wss/https only)
- [ ] Set CORS_ORIGIN to your domains
- [ ] Enable DATABASE_SSL=true
- [ ] Confirm ALLOW_UNVERIFIED_PURCHASES=false

## Google Play & Monetization
- [ ] Create `remove_ads` in Play Console
- [ ] Create `question_pack_1` in Play Console
- [ ] Test purchase verification with internal testing
- [ ] Test restore purchases after reinstall
- [ ] Verify rewarded ads only credit through AdMob SSV
- [ ] Point AdMob SSV to https://YOUR_API_DOMAIN/api/admob/ssv

## Android release
- [ ] Replace test AdMob IDs with real ones via env / buildConfig
- [ ] Set FACEOFF_SERVER_URL to production https URL
- [ ] Generate signed release AAB
- [ ] Privacy policy URL + Data safety form
- [ ] Content rating questionnaire
- [ ] Store listing: screenshots, icon, feature graphic

## Gameplay hardening
- [ ] Test reconnect during every phase
- [ ] Test host disconnect and migration
- [ ] Test duplicate buzz/guess/reward events
- [ ] Test simultaneous wagers
- [ ] Test modified client attempting to award coins
- [ ] Test multiple backend instances with Redis
