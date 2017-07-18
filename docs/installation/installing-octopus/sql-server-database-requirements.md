---
title: SQL Server Database Requirements
description: Information on the Microsoft SQL Server database requirements required to support Octopus Deploy.
position: 0
---

Octopus Deploy stores projects, environments, and deployment history in a Microsoft SQL Server database.

## Supported versions and editions {#SQLServerDatabaseRequirements-Supportedversionsandeditions}

Octopus works with a wide range of versions and editions of SQL Server, from a local SQL Server Express instance, all the way to an Enterprise Edition cluster, or hosted database-as-a-service. The following versions/editions are supported:

- Supported versions:
    * SQL Server 2008
    * SQL Server 2008 R2
    * SQL Server 2012
    * SQL Server 2014
    * SQL Server 2016
- Supported editions:
    * Express (free)
    * Web
    * Datacenter
    * Standard
    * Enterprise
- Microsoft Azure SQL Database
- AWS RDS SQL Database

:::hint
We automatically test every release of Octopus Server against each of these databases.
:::

### Using SQL Server Express {#SQLServerDatabaseRequirements-UsingSQLServerExpress}

The easiest, and cheapest way to get started is [download SQL Server Express](http://downloadsqlserverexpress.com/) and install Octopus Server and SQL Server Express side-by-side on your machine/server. This is a great way to test Octopus for a proof of concept. Depending on your needs, you might decide to use SQL Server Express, or upgrade to another supported edition.

## Creating the database

The Octopus installation wizard can create the database for you (our preferred method). Otherwise you can point Octopus to an existing database. Octopus works with both local and remote database servers, but it is worth considering the [performance implications](/docs/administration/performance.md) before making a decision.

:::hint
If you are using a hosted database service you will need to [create your own database](#create-your-own) and provide Octopus with the connection details.
:::

### Let the Octopus setup wizard create the database

1. Start the Octopus Server setup wizard
    - The user running the wizard must have the privileges on your SQL Server to create databases and grant permissions
1. On the database step, select the SQL Server where you want the database hosted
1. Enter the name that you would like to call this database
    - If you accidentally enter the name of an existing database, the setup process will install Octopus into that pre-existing database!
1. When you click the `Next` button the wizard will create the database with the appropriate configuration and permissions for Octopus Server to run successfully

![](/docs/images/3048120/3278498.png "width=500")

### Create your own database {#create-your-own}

If you need to create the database yourself:

1. The database must not be shared with any other application.
1. The default schema must be **dbo**.
1. The database must use a **case-insensitive collation** (a collation with a name containing "\_CI\_").
1. If you are using **Integrated Authentication** to connect to your database:
    - The user account installing Octopus must be a member of the **db\_owner** role for that database.
    - The account the Octopus Deploy windows server process runs under (by default, the `Local System` account) must be a member of the **db\_owner** role for that database.
1. If you are using **SQL Authentication** to connect to your database:
    - The SQL user account defined in your connection string must be a member of the **db\_owner** role for that database.

:::hint
**Changing the database collation**
See [here](/docs/administration/octopus-database/changing-the-collation-of-the-octopus-database.md) for information about changing the database collation after the initial Octopus installation.
:::

## Database administration and maintenance

For more information about maintaining your Octopus database please read our [database administrators guide](/docs/administration/octopus-database/index.md).