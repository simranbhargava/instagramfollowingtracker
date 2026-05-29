# Instagram Unfollow Checker

A browser console script that tells you who isn't following you back on Instagram, and who you haven't followed back.

No app installs, no third-party services, no login required — it runs directly in your browser using your existing Instagram session.

---

## How it works

The script calls Instagram's internal API (the same one the app uses) to fetch your full followers and following lists, then compares them to find the difference. Random delays are added between paginated requests to avoid triggering rate limits.

---

## Usage

**1. Go to [instagram.com](https://www.instagram.com) and log in.**

**2. Open the browser console.**

- **Chrome / Edge**: `F12` → Console tab
- **Firefox**: `F12` → Console tab
- **Safari**: `Cmd + Option + C` (you may need to enable developer tools in Settings first)

**3. Paste the script into the console.**

Replace `your_username` on the last line with your actual Instagram username, then press Enter.

```js
checkUnfollowers("your_username");
```

**4. Wait for it to finish.**

Progress is logged as it runs. Large accounts (1000+ followers) may take a minute or two.

**5. Read the output.**

Two arrays are printed in the console:

- `notFollowingMeBack` — accounts you follow that don't follow you back
- `iDontFollowBack` — accounts that follow you that you don't follow back

---

## Output example

```
Looking up @your_username...
Fetching followers...
[followers] loaded 50 so far — waiting 1120ms
[followers] loaded 100 so far — waiting 943ms
...
────────────────────────────────────────
✓ 834 followers · 610 following
→ Not following you back (42): ['example_user', 'another_user', ...]
→ You don't follow back (266): ['someone_else', ...]
```

---

## Limitations

- **Point-in-time only** — the script shows the current state. It can't tell you *when* someone unfollowed you or who unfollowed you since last time you ran it. To track changes over time, copy the output and compare it with a future run manually.
- **Public accounts only** — private accounts you don't follow back will show up in the list, but you won't be able to view their profiles without a follow request.
- **Rate limits** — Instagram may temporarily limit requests if you run the script repeatedly in a short window. If you see errors, wait a few minutes and try again.
- **API changes** — this script uses Instagram's internal API, which can change without notice. If it stops working, the API endpoint or App ID may have changed.

---

## Privacy & security

- The script runs entirely in your browser. No data is sent anywhere except to Instagram's own servers.
- It uses your existing logged-in session (via `credentials: "include"`). No passwords or tokens are stored or transmitted elsewhere.
- Nothing is written to disk or saved after you close the tab.

---

## Troubleshooting

**"User not found" error**
Make sure you typed your username correctly and that you're logged into that account.

**Empty arrays / wrong counts**
Instagram's API occasionally returns incomplete data for very large accounts. If the follower/following counts look off, try running the script again.

**Script stops mid-run**
Instagram may have rate-limited the session. Wait a few minutes, refresh the page, and run the script again.

**Console says "allow pasting"**
Some browsers block pasting into the console as a security measure. Type `allow pasting` and press Enter first (Chrome), then paste the script.

---

## License

MIT — do whatever you want with it.
