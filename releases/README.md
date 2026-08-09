# FlowShizz updates

Serves the update feed for the [FlowShizz](https://www.flowshizz.com) Mac app at
**https://updates.flowshizz.com** (GitHub Pages, custom domain).

Every installed copy of FlowShizz checks `appcast.xml` here once a day. **If this
site stops serving, auto-updates stop for everyone** — the URL is compiled into
the app and can't be changed without shipping an update, which needs this feed to
work. So: don't rename `appcast.xml`, don't delete old DMGs, don't take the
custom domain off.

```
appcast.xml              the feed Sparkle reads
FlowShizz-1.0.dmg        every released build, kept forever
FlowShizz-1.1.dmg
```

This mirrors `releases/` in the app repo exactly, so publishing is always
"upload everything in that folder" — the two can't drift apart.

## Publishing a release

Generated in the app repo, not by hand:

```bash
./release.sh --dmg-only /path/to/notarized/FlowShizz.app
```

That signs the DMG with the private EdDSA key from the login Keychain and
rewrites `appcast.xml` from every DMG in `releases/`. Upload both results here.

An update whose signature doesn't verify against the public key baked into the
app is refused — which is what stops a hijacked download installing itself on
an app that ships a root helper.

## Don't delete old DMGs

`generate_appcast` rebuilds the entire feed from whatever is in the folder.
Remove an old build and it silently drops out of the feed.

Full documentation: `UPDATES.md` in the FlowShizz Mac app repo.
