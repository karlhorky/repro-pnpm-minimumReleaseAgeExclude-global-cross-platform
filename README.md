# repro-pnpm-minimumReleaseAgeExclude-global-cross-platform

Reproduction of configuring pnpm [`minimumReleaseAge`](https://pnpm.io/settings#minimumreleaseage) and [`minimumReleaseAgeExclude`](https://pnpm.io/settings#minimumreleaseageexclude) globally across Windows, macOS and Linux

After configuring `minimumReleaseAge` of `10080` minutes (7 days), pnpm correctly prevents installation of `@types/react@19.1.13` (which is 6 days old at the time of writing):

```bash
=== Init pnpm package.json===
Wrote to D:\a\repro-pnpm-minimumReleaseAgeExclude-global-cross-platform\repro-pnpm-minimumReleaseAgeExclude-global-cross-platform\package.json
{
  "name": "repro-pnpm-minimumReleaseAgeExclude-global-cross-platform",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "packageManager": "pnpm@10.17.0"
}
=== Attempt install (should succeed)===
Progress: resolved 1, reused 0, downloaded 0, added 0
Packages: +2
++
Progress: resolved 2, reused 0, downloaded 2, added 2, done
dependencies:
+ @types/react 19.1.13
Done in 1.2s using pnpm v10.17.0
Packages: -2
--
dependencies:
- @types/react 19.1.13
Done in 375ms using pnpm v10.17.0
=== Edit pnpm global config rc to set 7-day minimumReleaseAge ===
=== debugging ===
C:\Users\runneradmin\AppData\Local/pnpm/config/rc
minimum-release-age=10080
=== Perl commands ===
=== After writing ===
minimum-release-age=10080
minimum-release-age-exclude[]=eslint-config-upleveled
minimum-release-age-exclude[]=stylelint-config-upleveled
=== Attempt install again (should fail)===
 ERR_PNPM_NO_MATCHING_VERSION  No matching version found for @types/react@19.1.13 while fetching it from https://registry.npmjs.org/
This error happened while installing a direct dependency of D:\a\repro-pnpm-minimumReleaseAgeExclude-global-cross-platform\repro-pnpm-minimumReleaseAgeExclude-global-cross-platform
The latest release of @types/react is "19.1.13".
Other releases are:
  * ts2.0: 15.0.1
  * ts2.1: 15.0.20
  * ts2.2: 15.0.30
  * ts2.4: 16.0.36
  * ts2.5: 16.0.36
  * ts2.3: 16.0.36
  * ts2.7: 16.4.7
  * ts2.6: 16.4.7
  * ts2.8: 16.9.34
  * ts2.9: 16.9.35
  * ts3.0: 16.9.46
  * ts3.1: 16.9.49
  * ts3.2: 16.9.56
  * ts3.3: 17.0.0
  * ts3.4: 17.0.2
  * ts3.5: 17.0.8
  * ts3.6: 17.0.19
  * ts3.7: 17.0.35
  * ts3.8: 17.0.39
  * ts3.9: 18.0.12
  * ts4.0: 18.0.18
  * ts4.1: 18.0.25
  * ts4.2: 18.0.28
  * ts4.3: 18.2.21
  * ts4.4: 18.2.21
  * ts4.5: 18.2.39
  * ts4.6: 18.2.64
  * ts4.7: 18.3.3
  * ts4.8: 18.3.12
  * ts4.9: 18.3.12
  * ts5.0: 19.0.12
  * ts5.1: 19.1.9
  * ts6.0: 19.1.13
  * ts5.7: 19.1.13
  * ts5.2: 19.1.13
  * ts5.4: 19.1.13
  * ts5.3: 19.1.13
  * ts5.6: 19.1.13
  * ts5.8: 19.1.13
  * ts5.9: 19.1.13
  * ts5.5: 19.1.13
If you need the full list of all 664 published versions run "$ pnpm view @types/react versions".
Error: Process completed with exit code 1.
```

`minimumReleaseAgeExclude` is configured by editing the pnpm global configuration file (different locations on [Windows](https://github.com/karlhorky/repro-pnpm-minimumReleaseAgeExclude-global-cross-platform/blob/main/.github/workflows/ci-windows-latest.yml), [macOS](https://github.com/karlhorky/repro-pnpm-minimumReleaseAgeExclude-global-cross-platform/blob/main/.github/workflows/ci-macos-latest.yml) and [Linux](https://github.com/karlhorky/repro-pnpm-minimumReleaseAgeExclude-global-cross-platform/blob/main/.github/workflows/ci-ubuntu-latest.yml)).

Screenshots showing same behavior on Windows, macOS and Linux:

<img width="1434" height="855" alt="Screenshot 2025-09-18 at 21 57 37" src="https://github.com/user-attachments/assets/38cdf9c5-2d4e-4d2c-bdc3-20bfd910f245" />

<img width="1427" height="848" alt="Screenshot 2025-09-18 at 21 58 12" src="https://github.com/user-attachments/assets/87a939d2-dcfa-4ebd-b034-337259f508bb" />

<img width="1434" height="854" alt="Screenshot 2025-09-18 at 21 58 32" src="https://github.com/user-attachments/assets/f5a07d34-91bb-4b87-a1d8-c804feee26ef" />
