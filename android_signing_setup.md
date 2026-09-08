# Android Release Signing

The `android/app/build.gradle` in this project already reads from
`key.properties` for release signing (see the `signingConfigs` block) —
this doc walks through generating that file's contents and wiring the
matching GitHub secrets.

## 1. Generate a keystore locally (one-time), if you don't have one:

```bash
keytool -genkey -v -keystore release-key.jks -keyalg RSA -keysize 2048 \
  -validity 10000 -alias rto_expert_key
```

## 2. Add these as GitHub repo secrets (Settings → Secrets and variables → Actions):

| Secret name | Value |
|---|---|
| `KEYSTORE_BASE64` | `base64 -w0 release-key.jks` output (Linux) or `base64 -i release-key.jks \| pbcopy` (macOS) |
| `KEYSTORE_PASSWORD` | the password you set when generating the keystore |
| `KEY_ALIAS` | `rto_expert_key` (or whatever alias you used) |
| `KEY_PASSWORD` | usually same as store password unless you set a separate one |

Once these are set, pushing a tag like `v1.0.0` triggers the `build_release`
job in `.github/workflows/build.yml`, which produces a signed release APK
and AAB and attaches them to a GitHub Release automatically.

**Never commit `release-key.jks` or `key.properties` to the repo** — both
are generated fresh inside the CI runner from secrets and discarded after
the job finishes.
