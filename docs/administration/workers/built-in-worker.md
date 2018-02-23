---
title: Built-in worker
description: Octopus Server comes with a built-in worker which enables you to conveniently run parts of your deployment process on the Octopus Server without the need to install a Tentacle or other deployment target. This page describes how to configure the built-in worker for a variety of scenarios.
position: 1
---

Octopus Server comes with a built-in worker which enables you to conveniently run parts of your deployment process on the Octopus Server without the need to install a Tentacle or other deployment target. This is very convenient when you are getting started with Octopus Deploy, but it does come with several security implications.

This page describes how to configure the built-in worker for a variety of scenarios.

!toc

## Built-in worker

Prior to Octopus `3.0` you required a Tentacle somewhere to do work as part of a deployment. If you wanted to deploy a web site to Azure, you would need to configure a Tentacle as a deployment target and use it as a jump box, when all you needed to do was push your package to Azure and call some APIs. That Tentacle wasn't really a deployment target at all.

In Octopus `3.0` we added the concept of a **worker** into Octopus Server. Now you could install Octopus Server and deploy a web site to Azure in minutes all without any Tentacles involved.

In Octopus `3.3` we added a feature which lets you [run deployment steps on the Octopus Server](/docs/deployment-process/how-to-run-steps-on-the-octopus-server.md) which again removed a lot of friction for scenarios other than deploying to Azure.

However, this convenience comes at a cost: **security**.

## Default configuration

By default Octopus Server runs as the highly privileged `Local System` account on Windows. We typically recommend running Octopus Server as a different account, either a User or Managed Service Account (MSA), so you can grant specific privileges to that account.

When you first install Octopus Server the built-in worker is configured to run using the same user account as the Octopus Server itself. This means your deployment process can do the same things the Octopus Server can do.

## Running tasks on the Octopus Server as a different user

In Octopus `2018.1` you can configure the built-in worker to execute tasks as a different user account. This user account can be a down-level account with very restricted privileges.

```plaintext
Octopus.Server.exe builtin-worker --username=OctopusWorker --password=XXXXXXXXXX
```

All tasks which execute on the Octopus Server will run as that user account. The only gotcha is that the user account running the Octopus Server must be a member of the `BUILTIN\Administrators` group to launch new processes as the built-in worker user and impersonate the built-in worker user.

This same command-line tool can automatically configure the correct user accounts on the local machine, and wire it all up for you.

```plaintext
Octopus.Server.exe builtin-worker --auto-configure
```

Which results in something like this:

```plaintext
Creating a user account on the local machine called 'OctopusServer' and adding it to the 'BUILTIN\Administrators' group.
Granting the 'SeServiceLogonRight' privilege to the 'MACHINE-123\OctopusServer' user account.
Configuring the 'OctopusDeploy' Windows Service to start as the 'MACHINE-123\OctopusServer' user account.
[SC] ChangeServiceConfig SUCCESS
Creating a down-level user account on the local machine called 'OctopusWorker' for the built-in worker.
The built-in worker is now configured to execute scripts as MACHINE-123\OctopusWorker.
Testing the built-in worker configuration.
Built-in worker: SUCCESS
The success of this configuration depends on both the source and target user accounts.
Current User: MACHINE-123\admin-user
Target User: MACHINE-123\OctopusWorker


Step 1: Testing credentials... PASSED
Step 2: Testing thread impersonation... PASSED
Step 3: Testing process impersonation... PASSED
Step 4: Check the process impersonation worked as expected... PASSED

NOTE: This test succeeded when starting from the user account 'MACHINE-123\admin-user'. If the Octopus Server usually runs as a different user account these results may vary. The same test will be run each time the Octopus Server starts to be certain the built-in worker is configured correctly.

These changes require a restart of the Octopus Server.
```

## Troubleshooting

You cannot run the Octopus Server as the `Local System` account and successfully launch the built-in worker as a different user account. Please use the `--auto-configure` option, or create a user account as a member of the `BUILTIN\Administrators` group.