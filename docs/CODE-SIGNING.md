# Code signing Winston with SignPath

Winston's official `winston.exe` is built by GitHub Actions (`.github/workflows/winston.yml`) and signed by
[SignPath Foundation](https://signpath.org/), which provides free certificates to qualifying open source
projects. Until the signing service is configured, the workflow still produces an **unsigned** build.

## One-time setup (maintainer)

1. **Apply** at <https://signpath.org/apply> with this public repository. Requirements include an OSI
   license (GPLv3 qualifies), an active public project, MFA on the accounts involved, and the
   "Code signing policy" section in the README (already present).
2. After approval, in the SignPath web app:
   - Create a **project** with slug `winston`.
   - Upload `.signpath/artifact-configuration.xml` as the project's artifact configuration.
   - Create two signing policies: `test-signing` (builds from `main`) and `release-signing`
     (tagged releases, with manual approval).
   - Add the **GitHub.com** trusted build system and link it to the project, and install the
     **SignPath GitHub App** on this repository.
   - Create a user (or CI user) with *submitter* permission and generate an **API token**.
3. In this repository (the token is a secret, enter it yourself, never commit it):

   ```bash
   gh variable set SIGNPATH_ORGANIZATION_ID --repo MDWSTLabs/Winston --body "<your SignPath organization id>"
   gh secret set SIGNPATH_API_TOKEN --repo MDWSTLabs/Winston
   ```

   If you used different slugs, also set `SIGNPATH_PROJECT_SLUG`, `SIGNPATH_TEST_POLICY_SLUG` and
   `SIGNPATH_RELEASE_POLICY_SLUG` as repository variables.

## Releasing

```bash
git tag v1.0.1 && git push origin v1.0.1
```

The workflow builds, submits the signing request, waits for approval of the `release-signing` policy, and
attaches the signed `winston.exe` and its SHA-256 to the GitHub release.

## Verifying a download

```powershell
Get-AuthenticodeSignature .\winston.exe | Format-List Status, SignerCertificate
(Get-FileHash .\winston.exe -Algorithm SHA256).Hash
```

`Status` should be `Valid`. Compare the hash with the `winston.exe.sha256` file on the release.
