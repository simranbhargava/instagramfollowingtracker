Here's the updated README. Changes: the username is now set in a `const` near the top of the script, the user ID comes from your login cookie, errors are explained, the script runs sequentially, and the troubleshooting section covers the endpoint failure you hit.

```markdown
# Instagram Unfollow Checker

A browser console script that tells you who isn't following you back on Instagram, who you haven't followed back, and **who has unfollowed you since your last run**.

No app installs, no third-party services, no password entry. It runs directly in your browser using your existing Instagram session.

---

## How it works

The script calls Instagram's internal API (the same one the web app uses) to fetch your full followers and following lists, then compares them to find the differences. Requests run one list at a time, with random delays between paginated requests, to avoid triggering rate limits.

Your user ID is read from the `ds_user_id` cookie that Instagram sets when you're logged in. If the cookie isn't available, the script falls back to the `web_profile_info` endpoint using your username.

To track unfollowers over time, the script saves a snapshot of your followers to `localStorage` after each run. On the next run, it compares the current follower list against that snapshot to detect anyone who dropped off.

---

## Usage

**1. Go to [instagram.com](https://www.instagram.com) and log in.**

**2. Open the browser console.**
- **Chrome / Edge**: `F12` → Console tab
- **Firefox**: `F12` → Console tab
- **Safari**: `Cmd + Option + C` (you may need to enable developer tools in Settings first)

**3. Set your username in the script.**

Near the top of the script, replace `your_username` with your actual Instagram username:

```js
const username = "your_username";
```

**4. Paste the script into the console and press Enter.**

**5. Wait for it to finish.**

Progress is logged as it runs. Large accounts (1000+ followers) may take a minute or two.

**6. Read the output.**

Three arrays are printed in the console:

- `PeopleNotFollowingMeBack`: accounts you follow that don't follow you back
- `PeopleIDontFollowBack`: accounts that follow you that you don't follow back
- `PeopleWhoUnfollowedMe`: accounts that were in your followers snapshot but have since unfollowed you

> **Note:** `PeopleWhoUnfollowedMe` will be empty on your first run, since there's no previous snapshot to compare against yet. Run the script again later to start seeing unfollowers.

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

- **Snapshot required for unfollower detection**: the script can't tell you who unfollowed you on the very first run. You need at least two runs (with time in between) to detect changes.
- **Your own account only (by default)**: the cookie lookup returns the ID of the account you're logged into. The fallback endpoint can look up other accounts, but only if their profile is public or you follow them.
- **Private accounts**: private accounts you don't follow back will show up in the lists, but you won't be able to view their profiles without a follow request.
- **Rate limits**: Instagram may temporarily limit requests if you run the script repeatedly in a short window. If you see errors, wait a few minutes and try again.
- **API changes**: this script uses Instagram's internal API, which can change without notice. If it stops working, an endpoint or the App ID may have changed (see Troubleshooting).

---

## Privacy & security

- The script runs entirely in your browser. No data is sent anywhere except to Instagram's own servers.
- It uses your existing logged-in session (via `credentials: "include"`). No passwords or tokens are stored or transmitted elsewhere.
- The follower snapshot is saved to `localStorage` in your browser. It stays on your device and is never sent anywhere.
- Only paste scripts into your console that you've read and trust. Console access can act as your logged-in session.

---

## Troubleshooting

**`Request failed (404)` or `Expected JSON but got text/html`**
Instagram has removed or changed the endpoint named in the error message. The script prints the failing URL, so you can see which one it was. Earlier versions of this script used the `topsearch` endpoint to look up user IDs, which Instagram stopped serving; the current version uses your login cookie instead.

**`Request failed (401)` or `(403)`**
You're probably not logged in, or your session expired. Refresh instagram.com, log in, and run the script again.

**`Request failed (429)`**
Instagram is rate limiting you. Wait several minutes (or longer) before trying again.

**"Could not find user" error**
Make sure you're logged into the account matching the username you set, and that you're running the script on instagram.com.

**Empty arrays / wrong counts**
Instagram's API occasionally returns incomplete data for very large accounts. If the follower/following counts look off, try running the script again.

**`PeopleWhoUnfollowedMe` is always empty**
This is expected on your first run. A snapshot is saved after each run, so come back later and run it again to see unfollowers.

**Script stops mid-run**
Instagram may have rate-limited the session. Wait a few minutes, refresh the page, and run the script again.

**Console says "allow pasting"**
Some browsers block pasting into the console as a security measure. Type `allow pasting` and press Enter first (Chrome), then paste the script.

---

## License

MIT. Do whatever you want with it.
```

One thing to check before committing: the Usage section says to edit `const username = ...` near the top, which matches the new script, but if your repo's script file still has the old `username = "example_username";` line at the bottom, update it to match.
