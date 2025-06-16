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

At the end of the workflow run, each built installer can be found in a zip file
on the workflow output page.

## Current build limitations

The Windows installer must currently be downloaded and code signed before
distribution.

The macOS packages will be code signed in the build if the relevant secrets and
ID variables are added to the GitHub repository. They must be downloaded,
notarized and stapled before distribution.
