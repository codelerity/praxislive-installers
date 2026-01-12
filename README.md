# PraxisLIVE installer builds

This repository contains the configuration and build system for the installers
of [PraxisLIVE](https://www.praxislive.org).

## Build

Links and hashes of PraxisLIVE, the bundled JDK runtimes, and the NBPackage
installer build tool are configured in the `build.properties` file. This file
also contains the version number to write into each installer. Other configuration
for individual packages is included in the `config` files passed in to NBPackage.

Installers are built using the workflow at `.github/workflows/build.yaml`. This
workflow is triggered manually from the `Actions/Build` page using the
`Run workflow` button and selecting the required installers.

The workflow uses the `Exec.java` single source file to download and verify the
required packages, as well as to configure and execute NBPackage.

The workflow uses the `Exec.java` single source file to download and verify the
required files, as well as to configure and execute NBPackage. All downloads are
verified against the hashes provided in the `build.properties` file.

If a release tag is provided when triggering the workflow, installers will be
uploaded to this release. If no release tag is provided, the installers will be
uploaded as workflow artefacts, and can be found in zip files on the workflow
output page. Any release tag must correspond to an already created release,
although this may be in draft form.

## Code signing and secrets

The workflow will optionally sign Windows and macOS installers during the build
if the relevant repository secrets and variables have been defined. Signing is
triggered by the presence of certain variables (marked) - all related secrets
must then be defined.

### Variables

- `APP_CERT_ID` : the name of the Apple Developer Application Certificate passed
  in to `codesign` during the macOS installer build. Presence of this variable
  triggers signing, and all other related macOS variables and secrets must be
  added.
- `AZURE_CERT_PROFILE_NAME` : the name of the certificate profile used by the
  Azure Trusted Signing action during the Windows installer build. Presence of
  this variable triggers signing, and all other related Windows secrets must be
  added.
- `INST_CERT_ID` : the name of the Apple Developer Installer Certificate passed
  in to `pkgbuild` during the macOS installer build.

### Secrets

- `APP_CERT_BASE64` : the Apple Developer Application Certificate exported as
  base64 (see related [GitHub documentation][github-macos]). Required for macOS
  installer signing.
- `APP_CERT_PASSWORD` : password for the Apple Developer Application Certificate.
  Required for macOS installer signing.
- `AZURE_CLIENT_ID` : the `azure-client-id` parameter for the Azure Trusted
  Signing action. Required for Windows installer signing.
- `AZURE_CLIENT_SECRET` : the `azure-client-secret` parameter for the Azure
  Trusted Signing action. Required for Windows installer signing.
- `AZURE_CODE_SIGNING_NAME` : the `trusted-signing-account-name` parameter for
  the Azure Trusted Signing action. Required for Windows installer signing.
- `AZURE_ENDPOINT` : the `endpoint` parameter for the Azure Trusted Signing
  action. Required for Windows installer signing.
- `AZURE_TENANT_ID` : the `azure-tenant-id` parameter for the Azure Trusted
  Signing action. Required for Windows installer signing.
- `INST_CERT_BASE64` : the Apple Developer Installer Certificate exported as
  base64. Required for macOS installer signing.
- `INST_CERT_PASSWORD` : password for the Apple Developer Installer Certificate.
  Required for macOS installer signing.

## Current build limitations

The macOS packages will be code signed in the build if the relevant secrets and
ID variables are added to the GitHub repository. They must be downloaded,
notarized and stapled before distribution.
