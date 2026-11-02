
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
