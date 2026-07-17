# rku is Roku publishing made easy. From your terminal.

Roku has no publish API. `rku` is one. Roku ships channels through a web portal you click through by hand — signing in, linking your account, uploading a `.pkg`, publishing. `rku` drives that portal programmatically, so shipping a channel becomes a command.

```bash
npm install -g @namiml/rku-cli
```

Requires Node 18+. The first run of `rku connect` downloads a Chromium it uses to sign you in to Roku — nothing else needs a browser.

## Quick start

Three steps to your first publish:

```bash
rku login      # sign in to rkuhub (opens your browser to approve)
rku connect    # link your Roku developer account (opens a browser for Roku sign-in)
rku publish -c my-channel -p ./my-app.pkg -v 1.0.0
```

Honest output — no emoji, no surprises. `rku` tells you what happened and what to do next, then gets out of the way:

```
❯ rku publish -c acme-eu-beta -p ./acme-eu-4.12.1.pkg -v 4.12.1
↑ uploaded acme-eu-4.12.1.pkg (12,438,016 bytes)
▶ publishing to 'acme-eu-beta' (job 806e6110)
✓ published to 'acme-eu-beta'
```

## Commands

| Command | What it does |
|---|---|
| `rku login` | Sign in to rkuhub via device flow (opens your browser to approve). |
| `rku connect` | Link your Roku developer account — opens a local browser so you can complete Roku's sign-in. |
| `rku publish -c <slug> -p <pkg>` | Upload a `.pkg` and publish it to a channel; streams status until it's live or fails. |
| `rku channels` | List the channels rkuhub tracks for your org. |
| `rku jobs [-c <slug>]` | List recent publish jobs (optionally filtered by channel). |
| `rku whoami` | Show the org you're signed in as. |
| `rku disconnect` | Invalidate your linked Roku session on rkuhub. |
| `rku logout` | Clear stored credentials on this machine. |

### `rku publish` options

```
-c, --channel <slug>     target channel slug          (required)
-p, --package <path>     path to the .pkg             (required)
-v, --version <version>  version label for the publish
    --name <name>        artifact name (defaults to the .pkg filename)
    --notes <text>       "What's New" release notes recorded with the publish
    --notes-file <path>  release notes read from a text/markdown file (instead of --notes)
    --timeout <seconds>  max seconds to wait for a terminal state (default 300)
    --interval <seconds> seconds between status polls (default 5)
```

`rku publish` exits non-zero if the publish fails, so it drops straight into a script.

## In CI

`rku` is built for shipping, not clicking — the same publish you run by hand runs in CI. CI authenticates with a rkuhub **API token** (rather than the interactive `rku login`). See the CI guide at [rku.dev](https://www.rku.dev).

## License

© Nami ML Inc. Your use is subject to the terms at <https://www.rku.dev/>.
