# Migrating from IOP to HDP {#migrate_process .task}

This topic shows you how to migrate from IBM Open Platform \(IOP\) 4.2.x to Hortonworks Data Platform \(HDP\) 2.6.2 and upgrade Big SQL.

-   To upgrade Big SQL, you must have a user with the following attributes:

    -   passwordless sudo access on all nodes of the cluster, including the Ambari server itself.
    -   The ability to connect passwordlessly through ssh from the Ambari server to all Big SQL nodes.
    This user can be root. If the user is not root, the user name must be passed to the upgrade script with the -a option. The upgrade script must be run with root user privilege. This can be achieved by using the sudo command. If you have configured Ambari for non-root access \(see [hdp\_ambari\_non\_root\_access\_bigsql.md\#](hdp_ambari_non_root_access_bigsql.md#)\), use the -a option with the user name created for that purpose.

-   Big SQL installation requires about 2 GB of free disk space on the /usr partition. The upgrade process requires about 4 GB of free disk space on the root partition. This space is used temporarily by the upgrade process. It is released when the upgrade is complete.
-   You must disable all high availability before performing an upgrade.
-   Ambari configuration groups are not supported. The upgrade script produces a warning message if you use configuration groups. If you override the warning, you must validate that the configuration of all nodes in all configuration groups is updated as part of the upgrade. It is recommended that you remove all configuration groups before performing an upgrade.
-   You must disable Yarn and Slider support for Big SQL before performing an upgrade.

For additional prerequisites for your migration process, see **\[REFERENCE to HW content\]**.

Provides an overview of the full process for you to migrate from IBM Open Platform \(IOP\) versions 4.2.x to Hortonworks Data Platform \(HDP\) version 2.6.2. You must upgrade Big SQL to version 5.0.1 if you want full, functional support with the most recent version of the Hadoop Data Platform \(HDP\). For compatibility details see the [../../com.ibm.swg.im.bigsql.product.doc/doc/compatibility.md\#](../../com.ibm.swg.im.bigsql.product.doc/doc/compatibility.md#) topic.

1.  **Back up metadata**.
    -   For details on how to back up the open source services \(including Ambari\), see **\[HWX REFERENCE\]**.
    -   For details on how to backup IBM BigSQL and Data Server Manager \(DSM\), see [Backing up metadata](migrate_backup.md#).
2.  **Upgrade your version of Ambari**.For Ambari upgrade details, see **\[HWX REFERENCE\]**.
3.  **Clean up IBM value-added services from your environment**.For value-add service cleanup details, see [Cleaning up IBM value-add services](clean_valadd.md#).
4.  **Remove deprecated components and services from your environment**.For component and service deletion details, see [Removing deprecated components and services](clean_components.md#).
5.  **Upgrade the stack**.For stack upgrade details, see **\[HWX REFERENCE\]**.
6.  **Upgrade and finalize your Big SQL installation**.For upgrading and finalizing details, see [Upgrading and finalizing Big SQL](migrate_up_bigsql.md#).
7.  **Upgrade IBM DSM**.For upgrading details, see [Upgrading IBM Data Server Manager \(DSM\)](migrate_up_dsm.md#).

