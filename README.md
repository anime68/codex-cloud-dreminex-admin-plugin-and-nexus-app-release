# Dreminex Nexus Releases

Public, secret-free update channel for the signed Dreminex Nexus Android app.

## Public files

- Signed APK release assets
- `latest.json`
- SHA-256 checksums
- Permanent public certificate fingerprint
- Public release notes

Keystores, passwords, source code, OAuth credentials, recovery archives and internal reports must never be added here.

## Publishing a future update

1. Build the APK in the private source repository with the permanent Nexus key.
2. Verify package `com.dreminex.nexus`, a higher `versionCode`, and certificate SHA-256 from `CERTIFICATE_SHA256.txt`.
3. Create `release-input/vX.Y.Z/` containing the APK, `SHA256SUMS.txt`, and `RELEASE_NOTES.md`.
4. Update `latest.json` with the exact version, public release URL and APK SHA-256.
5. Push these files to `main`. The publish workflow verifies the checksum, publishes the GitHub Release, and validates `latest.json`.

Never overwrite a released APK with different bytes. Publish a higher version instead.
