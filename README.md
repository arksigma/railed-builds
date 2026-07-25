# railed-builds

Build pipeline for [Railed](https://github.com/arksigma/railed) (private
source). Lives on a public repo so iOS/Android builds run on GitHub's free
standard runners instead of consuming private-repo Actions minutes.

Builds run `eas build --local`, so no EAS build credits are consumed either.

## Triggers

- `repository_dispatch` type `build` with `{"ref": "vX.Y.Z"}` (optionally
  `platform`, `submit`) — sent by the source repo's dispatcher on tag push.
- `workflow_dispatch` — manual runs: pick ref, platform (ios/android/all),
  and whether to submit to TestFlight / Play.

Manual example:

```sh
gh workflow run build.yml -R arksigma/railed-builds -f ref=v1.2.0 -f platform=ios
```

## Secrets

| Secret | Purpose |
| --- | --- |
| `SOURCE_CHECKOUT_TOKEN` | PAT with read access to `arksigma/railed` |
| `EXPO_TOKEN` | expo.dev access token (credential resolution for local builds + submit) |
| `ASC_KEY_ID` / `ASC_ISSUER_ID` / `ASC_KEY_P8_BASE64` | App Store Connect API key (TestFlight submit) |
| `GOOGLE_PLAY_KEY_JSON` | Play service account key (Play submit) |

Only build logs are public — never the source or secret values.
