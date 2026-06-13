# Instagram Unfollow Checker

A browser console script that tells you who isn't following you back on Instagram, who you haven't followed back, and **who has unfollowed you since your last run**.

No app installs, no third-party services, no login required — it runs directly in your browser using your existing Instagram session.

---

## How it works

The script calls Instagram's internal API (the same one the app uses) to fetch your full followers and following lists, then compares them to find the difference. Random delays are added between paginated requests to avoid triggering rate limits.

To track unfollowers over time, the script saves a snapshot of your followers to `localStorage` after each run. On the next run, it compares the current follower list against that snapshot to detect anyone who dropped off.

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
username = "your_username";
```

**4. Wait for it to finish.**

Progress is logged as it runs. Large accounts (1000+ followers) may take a minute or two.

**5. Read the output.**

Three arrays are printed in the console:

- `PeopleNotFollowingMeBack` — accounts you follow that don't follow you back
- `PeopleIDontFollowBack` — accounts that follow you that you don't follow back
- `PeopleWhoUnfollowedMe` — accounts that were in your followers snapshot but have since unfollowed you

> **Note:** `PeopleWhoUnfollowedMe` will be empty on your first run — there's no previous snapshot to compare against yet. Run the script again later to start seeing unfollowers.

---

## Output example

```
--------------------------------------------------
Fetched 834 followers and 610 following.
Snapshot from: 2025-01-10T14:23:00.000Z
Previous follower count: 851
Snapshot saved. Run again later to track unfollowers.
--------------------------------------------------
{
  PeopleNotFollowingMeBack: ['example_user', 'another_user', ...],
  PeopleIDontFollowBack: ['someone_else', ...],
  PeopleWhoUnfollowedMe: ['user_who_left', ...]
}
```

---

## Snapshot & unfollower tracking

The script uses your browser's `localStorage` (on instagram.com) to persist your follower list between sessions. Each run:

1. Loads the previous snapshot (if one exists) and diffs it against your current followers.
2. Saves the current follower list as the new snapshot for next time.

The snapshot is stored under the key `ig_tracker_followers_snapshot`. To reset it and start fresh, run this in the console:

```js
localStorage.removeItem("ig_tracker_followers_snapshot");
```

---

## Limitations

- **Snapshot required for unfollower detection** — the script can't tell you who unfollowed you on the very first run. You need at least two runs (with time in between) to detect changes.
- **Public accounts only** — private accounts you don't follow back will show up in the list, but you won't be able to view their profiles without a follow request.
- **Rate limits** — Instagram may temporarily limit requests if you run the script repeatedly in a short window. If you see errors, wait a few minutes and try again.
- **API changes** — this script uses Instagram's internal API, which can change without notice. If it stops working, the API endpoint or App ID may have changed.

---

## Privacy & security

- The script runs entirely in your browser. No data is sent anywhere except to Instagram's own servers.
- It uses your existing logged-in session (via `credentials: "include"`). No passwords or tokens are stored or transmitted elsewhere.
- The follower snapshot is saved to `localStorage` in your browser — it stays on your device and is never sent anywhere.

---

## Troubleshooting

**"User not found" error**
Make sure you typed your username correctly and that you're logged into that account.

**Empty arrays / wrong counts**
Instagram's API occasionally returns incomplete data for very large accounts. If the follower/following counts look off, try running the script again.

**`PeopleWhoUnfollowedMe` is always empty**
This is expected on your first run. A snapshot is saved after each run — come back later and run it again to see unfollowers.

**Script stops mid-run**
Instagram may have rate-limited the session. Wait a few minutes, refresh the page, and run the script again.

**Console says "allow pasting"**
Some browsers block pasting into the console as a security measure. Type `allow pasting` and press Enter first (Chrome), then paste the script.

---

## License

MIT — do whatever you want with it.
