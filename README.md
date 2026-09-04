# ClassOrganizer Releases

Public distribution channel for **ClassOrganizer** (WinUI). The application source lives in a
private repository; this repo hosts only the auto-update **feed manifest** and the signed
**installer assets** so that the in-app updater can reach them without authentication.

## Update feed

The app reads `update-feed.json`. Two equivalent URLs are used by the client:

- Primary (always the newest published release):
  `https://github.com/ThomasVasileiadis/ClassOrganizer-releases/releases/latest/download/update-feed.json`
- Fallback (branch copy):
  `https://raw.githubusercontent.com/ThomasVasileiadis/ClassOrganizer-releases/main/update-feed.json`

### Manifest schema

```json
{
  "version": "1.0.2.0",
  "downloadUrl": "https://github.com/ThomasVasileiadis/ClassOrganizer-releases/releases/download/v1.0.2.0/ClassOrganizer.WinUI-1.0.2.0-setup.exe",
  "sha256": "<64 hex chars of the installer>",
  "sizeBytes": 123456789,
  "releaseNotesUrl": "https://github.com/ThomasVasileiadis/ClassOrganizer-releases/releases/tag/v1.0.2.0",
  "mandatory": false,
  "minimumVersion": "1.0.0.0",
  "publishedAtUtc": "2026-09-03T00:00:00Z"
}
```

| Field | Meaning |
|---|---|
| `version` | Latest available app version (`Major.Minor.Patch.Build`). |
| `downloadUrl` | Absolute URL to the Inno Setup installer `.exe` for `version`. |
| `sha256` | Lowercase hex SHA-256 of the installer; the client refuses to run a mismatched download. |
| `sizeBytes` | Expected installer size in bytes; used for progress and a sanity check. |
| `releaseNotesUrl` | Optional. Shown to the user in the update prompt. |
| `mandatory` | Optional. When `true` the client will not offer a Cancel option. |
| `minimumVersion` | Optional. Clients older than this must update before continuing. |
| `publishedAtUtc` | Optional. Informational timestamp. |

## Publishing

Releases here are produced automatically by the private source repo's
`winui-release` GitHub Actions workflow when a `classorganizer-v*` tag is pushed. The workflow
builds the installer, computes the SHA-256, generates `update-feed.json`, and uploads both as a
release asset here (tag `v{version}`) using a fine-grained PAT stored as the `RELEASES_REPO_TOKEN`
secret in the source repo.

Do not commit application source or secrets to this repository.
