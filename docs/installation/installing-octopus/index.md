---
title: Installing Octopus
position: 0
description: How to install the central Octopus Deploy server, requirements, and troubleshooting.
---

The Octopus Deploy Server is software that you download and install on one of your own servers, just like you would install Microsoft SQL Server. The Octopus Deploy Server:

- runs as a Windows Service
- stores its data in a [SQL Server database](/docs/administration/octopus-database/index.md)
- has an embedded HTTP server which presents the main Octopus user interface (the **Octopus Web Portal**)

## Requirements {#InstallingOctopus-Requirements}

Your Octopus Deploy server requires:

- Windows Server 2008 SP2+, 2008 R2, 2012, 2012 R2 or 2016 ("Server with a GUI" install, not Server Core)
- .NET Framework 4.5+ ([download](https://www.microsoft.com/en-au/download/details.aspx?id=30653))
- .NET Framework 4.5.1+ for Octopus Server 3.4.0 and later
- Microsoft SQL Server installed locally, on another server, or using a hosted service ([more details](/docs/installation/installing-octopus/sql-server-database-requirements.md))
- Hardware:
    - Absolute minimum to make it run: 512MB RAM, 1GHz CPU, 2GB free disk space
    - Recommended for smaller deployments (less than 30 Tentacles for example): 2GB RAM, dual-core CPU, 10GB free disk space
    - Recommended for larger deployments: 4GB RAM, dual-core, 20GB free disk space

:::success
The latest Octopus Deploy MSI can always be [downloaded from the Octopus Deploy downloads page](https://octopus.com/downloads). You can also use our [permalinks](/docs/reference/download-permalinks.md) if you plan to automate the download.
:::

## Installation {#InstallingOctopus-Installation}

This three minute video  will walk you through the installation process:

<iframe src="//fast.wistia.net/embed/iframe/fsxoijvtvm" allowtransparency="true" frameborder="0" scrolling="no" class="wistia_embed" name="wistia_embed" allowfullscreen="" mozallowfullscreen="" webkitallowfullscreen="" oallowfullscreen="" msallowfullscreen="" width="640" height="360" style="margin: 30px"></iframe>

### Using a Managed Service Account (MSA) {#InstallingOctopus-UsingaManagedServiceAccount(MSA)}

You can run the Octopus Server using a Managed Service Account (MSA):

1. Install the Octopus Server and make sure it is running correctly using one of the built-in Windows Service accounts or a Custom Account
2. Reconfigure the Octopus Server Windows Service to use the MSA, either manually using the Service snap-in, or using `sc.exe config "OctopusDeploy" obj= Domain\Username$`
3. Restart the Octopus Server Windows Service

Learn about [using Managed Service Accounts](https://technet.microsoft.com/en-us/library/dd548356(v=ws.10).aspx).

## Troubleshooting {#InstallingOctopus-Troubleshooting}

In a few cases a bug in a 3rd party component causes the installer displays a "Installation directory must be on a local hard drive" error. If this occurs, running the install again from an elevated command prompt using the following command (replacing Octopus.3.3.4-x64.msi with the name of the installer you are using)

`msiexec /i Octopus.3.3.4-x64.msi WIXUI_DONTVALIDATEPATH="1"`

:::warning
**Deploying applications to an Azure website?**
If you get the following error it means you have a local copy of Web Deploy and that is being used. You will either need to upgrade your local version of Web Deploy to 3.5 or greater, or uninstall the local copy so Octopus can reference the embedded copy.
:::
