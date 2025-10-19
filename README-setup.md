
# Savannah Daily Briefing — Alexa Flash Briefing Starter

This folder contains everything you need to set up your **Savannah Daily Briefing** as an Alexa Flash Briefing source.

## What’s inside
- `savannah-daily-briefing-feed.json` — Flash Briefing JSON feed template. Point Alexa to the hosted URL of this file.
- `icon_108.png` and `icon_512.png` — required square icons for the skill.
- This README with setup steps and tips.

## Step 1 — Host the feed JSON over HTTPS
You need a publicly reachable HTTPS URL for the JSON file and for your daily MP3s.

Recommended hosting options:
- **Amazon S3 + CloudFront** — most reliable at scale.
- **GitHub Pages** — quick and free. Commit this JSON to a repo and enable Pages.
- **Static web host** like Netlify or Vercel — drop the file into a public site.

When hosted, your feed URL will look like:
```
https://YOUR_HOST/savannah-daily-briefing-feed.json
```

Update the `audioUrl` daily to point to your new MP3, for example:
```
https://YOUR_HOST/savannah-briefings/2025-10-20.mp3
```

## Step 2 — Prepare audio files
Alexa Flash Briefing accepts standard MP3 files over HTTPS. Keep each daily file under ~10 minutes.

Suggested file path pattern:
```
/savannah-briefings/YYYY-MM-DD.mp3
```

Include your subtle sonic signature at the start, then the narrated briefing.

## Step 3 — Create the Flash Briefing in Amazon
1. Go to **developer.amazon.com** and sign in.
2. Click **Developer Console** → **Create Skill**.
3. Name: **Savannah Daily Briefing**
4. Choose **Flash Briefing**.
5. In **Add New Feed**, configure:
   - **Feed name**: Savannah Daily Briefing
   - **Preamble**: leave empty
   - **Content type**: **Audio**
   - **Update frequency**: **Daily**
   - **Genre**: **News**
   - **Content language**: **English (United States)**
   - **Timezone**: **America/New_York**
   - **Feed URL**: the HTTPS URL where you host `savannah-daily-briefing-feed.json`
6. Save.

## Step 4 — Test on your own Alexa
1. Open the **Alexa** app → **More** → **Settings** → **News**.
2. Add your **Savannah Daily Briefing** source to Flash Briefing.
3. Move it near the top if you want it first.
4. Say: **"Alexa, what's my news?"**

## Daily update workflow
- Upload the new MP3 to your host, e.g. `/savannah-briefings/2025-10-20.mp3`.
- Update the `audioUrl` in `savannah-daily-briefing-feed.json` and optionally bump `lastUpdatedDate` and the entry's `updateDate` to the current date at 06:00:00Z.
- If you want multiple days in the feed, add another object to `entries` with a new `uid` per day. Alexa will play the most recent item.

### Minimal entry example
```json
{
  "uid": "2025-10-20",
  "updateDate": "2025-10-20T06:00:00.0Z",
  "titleText": "Savannah Daily Briefing",
  "mainText": "Audio briefing by Caroline for 2025-10-20",
  "redirectionUrl": "https://example.com/savannah",
  "audioUrl": "https://YOUR_HOST/savannah-briefings/2025-10-20.mp3"
}
```

## Notes
- Ensure HTTPS with valid TLS. Self-signed certs will fail.
- MP3 should be quickly streamable. Avoid extremely large bitrates.
- If using GitHub Pages, the audio files themselves may exceed the repo size; consider S3 for MP3 hosting and keep the JSON on GitHub Pages.

---

**Support checklist**
- [ ] Public HTTPS URL for JSON feed
- [ ] Public HTTPS URL for today's MP3
- [ ] Alexa Flash Briefing skill configured
- [ ] Source added in Alexa app and moved to top of news
