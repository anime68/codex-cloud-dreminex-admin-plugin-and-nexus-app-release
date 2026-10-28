# Dreminex Update Release Runbook

এই একটি document অনুসরণ করে যেকোনো অনুমোদিত developer, maintainer বা Codex session থেকে Dreminex Suite plugin এবং Dreminex Nexus app-এর public update নিরাপদভাবে প্রকাশ করা যাবে।

## 1. স্থায়ী ঠিকানা

| কাজ | Repository |
|---|---|
| Source, tests, signed build | `anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app` (Private) |
| Public updater channel | `anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release` (Public) |
| Manifest | `https://raw.githubusercontent.com/anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release/main/latest.json` |
| Releases | `https://github.com/anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release/releases` |

## 2. যে নিয়ম কখনো ভাঙা যাবে না

1. একই public tag বা asset overwrite করবে না। ভুল হলে higher version দিয়ে নতুন release করবে।
2. একই staging draft tag-এ `--clobber` করা artifact-কে আগের verified artifact ধরে নেবে না। প্রতিবার hash পুনরায় পরীক্ষা করবে।
3. User বা release report-এর expected SHA-256 না মিললে publication বন্ধ করবে।
4. Nexus APK সবসময় package `com.dreminex.nexus` এবং স্থায়ী certificate দিয়ে signed হবে।
5. Android `versionCode` আগের public APK থেকে বড় হবে।
6. Plugin ZIP-এর একমাত্র top-level directory হবে `dreminex-suite/`। ZIP-এর মধ্যে APK, installer, keystore বা secret থাকবে না।
7. Public repository-তে keystore, password, token, recovery archive বা private source রাখা যাবে না।
8. `latest.json`, release folder এবং hashes একই commit-এ publish করবে।
9. Workflow সবুজ এবং public assets read-back সফল হওয়ার আগে release completed বলা যাবে না।
10. Production website পরিবর্তন release publication-এর অংশ নয়।

## 3. Version পরিকল্পনা

Release শুরু করার আগে লিখে রাখো:

```text
PLUGIN_VERSION=1.23.0
ANDROID_VERSION=0.10.0
ANDROID_VERSION_CODE=13
TAG=dreminex-suite-v1.23.0-nexus-v0.10.0
```

একই value নিচের সব জায়গায় মিলবে:

### Plugin

- `dreminex-suite.php` → `Version:`
- `DREMINEX_SUITE_VERSION`
- ZIP filename
- `latest.json` → `plugin.version`
- combined release tag

### Nexus

- Gradle `versionName`
- Gradle `versionCode`
- APK filename
- `latest.json` → `android.version`
- combined release tag

## 4. Artifact সংগ্রহ

Private repository-এর successful validation এবং signed build run থেকে artifacts নাও। Draft release ব্যবহার করলে draft asset-এর hash আবার পরীক্ষা করো। একই tag rerun-এর কারণে asset বদলে যেতে পারে।

Public নাম অবশ্যই:

```text
Dreminex_Suite_v<PLUGIN_VERSION>.zip
Dreminex_Nexus_v<ANDROID_VERSION>_signed.apk
```

Validation artifact-এর filename পুরোনো হলেও ভেতরের version এবং expected hash ঠিক থাকলে verified public নামে rename করা যাবে। Content rebuild বা APK resign করা যাবে না।

## 5. Publication-এর আগে বাধ্যতামূলক verification

### 5.1 Hash

```bash
sha256sum Dreminex_Suite_v1.23.0.zip
sha256sum Dreminex_Nexus_v0.10.0_signed.apk
```

দুটি result approved expected hashes-এর exact match হতে হবে।

### 5.2 Plugin ZIP

```bash
zipinfo -1 Dreminex_Suite_v1.23.0.zip | head
unzip -p Dreminex_Suite_v1.23.0.zip \
  dreminex-suite/dreminex-suite.php | grep -E 'Version:|DREMINEX_SUITE_VERSION'
```

নিশ্চিত করো:

- সব entry `dreminex-suite/` দিয়ে শুরু হয়েছে।
- `dreminex-suite/dreminex-suite.php` আছে।
- header এবং constant exact target version।
- কোনো `.jks`, `.keystore`, `.env`, APK বা nested release ZIP নেই।

### 5.3 Signed APK

Android SDK build-tools ব্যবহার করে:

```bash
apksigner verify --verbose --print-certs Dreminex_Nexus_v0.10.0_signed.apk
aapt dump badging Dreminex_Nexus_v0.10.0_signed.apk | head -1
```

নিশ্চিত করো:

- signature verification PASS
- package `com.dreminex.nexus`
- versionName target version
- versionCode approved higher integer
- certificate SHA-256 public repository-এর `CERTIFICATE_SHA256.txt`-এর exact match

## 6. Exact public folder

```text
release-input/
└── dreminex-suite-v1.23.0-nexus-v0.10.0/
    ├── Dreminex_Suite_v1.23.0.zip
    ├── Dreminex_Nexus_v0.10.0_signed.apk
    ├── RELEASE_NOTES.md
    └── SHA256SUMS.txt
```

Checksum file তৈরি করো:

```bash
cd release-input/dreminex-suite-v1.23.0-nexus-v0.10.0
sha256sum Dreminex_Suite_v1.23.0.zip \
  Dreminex_Nexus_v0.10.0_signed.apk > SHA256SUMS.txt
sha256sum --check SHA256SUMS.txt
cd ../..
```

## 7. latest.json template

```json
{
  "schema_version": 2,
  "plugin": {
    "version": "1.23.0",
    "download_url": "https://github.com/anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release/releases/download/dreminex-suite-v1.23.0-nexus-v0.10.0/Dreminex_Suite_v1.23.0.zip",
    "sha256": "PLUGIN_SHA256",
    "release_notes_url": "https://github.com/anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release/releases/tag/dreminex-suite-v1.23.0-nexus-v0.10.0"
  },
  "android": {
    "version": "0.10.0",
    "download_url": "https://github.com/anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release/releases/download/dreminex-suite-v1.23.0-nexus-v0.10.0/Dreminex_Nexus_v0.10.0_signed.apk",
    "sha256": "APK_SHA256"
  },
  "windows": { "version": "", "download_url": "", "sha256": "" },
  "release_notes_url": "https://github.com/anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release/releases/tag/dreminex-suite-v1.23.0-nexus-v0.10.0"
}
```

URL অবশ্যই public release repository-এর final asset URL হবে। Private URL, Actions artifact URL বা branch raw asset URL ব্যবহার করবে না।

## 8. Publish করার exact sequence

1. Private tests এবং signed build successful কিনা যাচাই করো।
2. Approved artifacts collect করো; rebuild বা resign করো না।
3. Hash, ZIP structure/version এবং APK identity/certificate যাচাই করো।
4. নতুন combined tag-এর `release-input` folder তৈরি করো।
5. দুই artifact, notes ও checksum রাখো।
6. `latest.json` target release দিয়ে update করো।
7. `git diff --check` এবং `sha256sum --check` চালাও।
8. সব release files একই commit-এ public `main`-এ push করো।
9. `Publish verified Dreminex updates` workflow monitor করো।
10. Workflow failure হলে কারণ ঠিক করো অথবা `latest.json` last-known-good commit-এ rollback করো। Broken manifest রেখে থামবে না।
11. Published ZIP ও APK final URL থেকে আবার download করো।
12. Downloaded bytes-এর hashes, APK identity/certificate এবং ZIP structure পুনরায় যাচাই করো।
13. Raw `latest.json` read-back করো।
14. Installed আগের Nexus version থেকে **Check for updates** contract যাচাই করো।
15. সব evidence final report-এ দাও।

## 9. শুধু একটি component বদলালে

Combined workflow একটি ZIP ও একটি APK দুটোই চায়।

- শুধু plugin বদলালে plugin version বাড়বে; last verified APK নতুন combined folder-এ copy হবে, Android version/hash অপরিবর্তিত থাকবে।
- শুধু app বদলালে Android version ও versionCode বাড়বে; last verified plugin ZIP নতুন combined folder-এ copy হবে, plugin version/hash অপরিবর্তিত থাকবে।

নতুন tag-এর folder-এ উভয় physical asset রাখতে হবে। পুরোনো tag-এর URL reuse করবে না।

## 10. সাধারণ failure ও সমাধান

| সমস্যা | কারণ | সমাধান |
|---|---|---|
| App update দেখায় না | manifest version installed version-এর চেয়ে বড় নয় | versionName এবং versionCode বাড়াও |
| Hash mismatch | draft rerun asset overwrite করেছে বা ভুল artifact | exact successful run artifact উদ্ধার করো; publish বন্ধ রাখো |
| Plugin update দেখায় না | header/constant/manifest version mismatch বা cache | versions মিলাও; WordPress update check refresh করো |
| Release title পুরোনো | workflow title hard-coded | dynamic manifest-derived title বজায় রাখো |
| APK install fails | package বা signing certificate বদলেছে | permanent Nexus key-এর verified APK ব্যবহার করো |
| Workflow HTTP 200 কিন্তু wrong file | URL valid, bytes ভুল | final download hash gate অবশ্যই PASS করাও |
| ZIP install fails | loose root files বা wrong root folder | `dreminex-suite/` root দিয়ে package rebuild করো |
| Website manifest পুরোনো | 15 মিনিট transient cache | অপেক্ষা/authorized cache refresh; public fallback read-back যাচাই করো |
| GitHub artifact download সমস্যা | temporary Actions storage URL expired/blocked | GitHub runner-এর মধ্যে exact run artifact export করো এবং hash gate রাখো |

## 11. Rollback policy

- Published asset overwrite বা delete করে একই version পুনরায় ব্যবহার করবে না।
- Broken `latest.json` হলে last-known-good manifest commit অবিলম্বে restore করো।
- Corrected plugin/app higher version দিয়ে নতুন immutable release প্রকাশ করো।
- Android rollback update হিসেবে lower versionCode install হয় না; correction-এর versionCode আরও বড় হতে হবে।
- Database rollback দরকার হলে release-specific private rollback report অনুসরণ করো।

## 12. Completion checklist

- [ ] Source validation PASS
- [ ] Signed build PASS
- [ ] Expected hashes exact match
- [ ] Plugin header এবং constant exact
- [ ] ZIP root শুধু `dreminex-suite/`
- [ ] APK package/versionName/versionCode exact
- [ ] APK signature PASS
- [ ] Certificate fingerprint exact
- [ ] New immutable tag used
- [ ] Four release-input files present
- [ ] `SHA256SUMS.txt` PASS
- [ ] `latest.json` exact
- [ ] Public workflow SUCCESS
- [ ] Release title correct
- [ ] Public ZIP HTTP 200 + hash PASS
- [ ] Public APK HTTP 200 + hash PASS
- [ ] Public APK identity/certificate read-back PASS
- [ ] Raw manifest read-back PASS
- [ ] Previous app → new app visibility PASS
- [ ] Production website untouched unless separately authorized

## 13. অন্য developer বা Codex-কে দেওয়ার copy-ready prompt

নিচের block-এর version, code এবং expected hashes পূরণ করে পাঠাও:

```text
Follow DREMINEX_UPDATE_RELEASE_RUNBOOK_BN.md exactly.

Private repo: anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app
Public repo: anime68/codex-cloud-dreminex-admin-plugin-and-nexus-app-release
Plugin version: <PLUGIN_VERSION>
Android version: <ANDROID_VERSION>
Android versionCode: <VERSION_CODE>
Tag: dreminex-suite-v<PLUGIN_VERSION>-nexus-v<ANDROID_VERSION>
Expected plugin SHA-256: <PLUGIN_SHA256>
Expected APK SHA-256: <APK_SHA256>
Private signed build run/tag: <RUN_OR_TAG>

Do not change application source, signing identity or production website.
Do not publish any hash mismatch.
Publish only after all preflight gates pass.
Monitor the public workflow, verify downloaded public assets, read back latest.json,
and report URLs, hashes, APK identity, certificate, workflow conclusion and update visibility.
```

## 14. Final report format

```text
Release URL:
Plugin ZIP URL:
Signed APK URL:
Plugin SHA-256:
APK SHA-256:
APK package:
APK versionName:
APK versionCode:
Certificate SHA-256:
Signature verification:
Workflow URL and conclusion:
Public HTTP/hash verification:
latest.json read-back:
Previous → new update visibility:
Production website changed: NO
```

