# formwright-ats-config

Signed ATS fixes for the [Formwright](https://github.com/cmason145/formwright) Chrome extension.

This repo holds data only, never code: field keys, label patterns, add-entry selectors, settle
times and a kill switch per ATS. The extension fetches `config.json` and `config.json.sig` at most
once a day. It applies the config only when the signature verifies against the public key built
into the extension. If anything fails, the extension uses its built-in rules.

## Updating

1. Edit `config.json`. `configVersion` must go up with every change; the extension refuses an older
   version than the one it has.
2. Sign it from the formwright repo (the private key never leaves the owner's machine):

   ```sh
   node scripts/sign-config.mjs ../formwright-ats-config/config.json
   ```

3. Commit both files and push. Never edit `config.json` after signing, or the signature fails.

## Format

- `schemaVersion`: always `1`.
- `configVersion`: a positive integer that only increases.
- `minExtensionVersion` (optional): older extension builds ignore the config.
- `killSwitch` (optional): ATS ids Formwright stays off, for example `["workday"]`.
- `adapters` (optional): per-ATS patches. See `src/lib/config/remote.ts` in the extension.
