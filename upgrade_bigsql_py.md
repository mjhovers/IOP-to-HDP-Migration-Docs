# bigsql\_upgrade.py - Big SQL upgrade utility {#upgrade_bigsql_py .reference}

Allows you to Backup, Upgrade, Finalize, or Restore a Big SQL installation.

## Authorization { .section}

You must run this utility as root; you must either be logged on as the root user or use the sudo command.

## Prerequisites { .section}

The following are prerequisites for running the bigsql\_upgrade.py command:

-   To upgrade Big SQL, you must have a user with the following attributes:

    -   passwordless sudo access on all nodes of the cluster, including the Ambari server itself.
    -   The ability to connect passwordlessly through ssh from the Ambari server to all Big SQL nodes.
    This user can be root. If the user is not root, the user name must be passed to the upgrade script with the -a option. The upgrade script must be run with root user privilege. This can be achieved by using the sudo command. If you have configured Ambari for non-root access \(see [Configuring Ambari for non-root access](inst_non_root.md#)\), use the -a option with the user name created for that purpose.

-   The HDFS, Hive, and HBase services \(and the services they depend on\) must be running for you to perform the Big SQL upgrade. HDFS should not be running in safe mode.
-   Big SQL installation requires about 2 GB of free disk space on the /usr partition. The upgrade process requires about 4 GB of free disk space on the root partition. This space is used temporarily by the upgrade process. It is released when the upgrade is complete.
-   You must disable all Big SQL high availability before performing an upgrade.
-   Ambari configuration groups are not supported. The upgrade script produces a warning message if you use configuration groups. If you override the warning, you must validate that the configuration of all nodes in all configuration groups is updated as part of the upgrade. It is recommended that you remove all configuration groups before performing an upgrade.
-   You must disable Yarn and Slider support for Big SQL before performing an upgrade.

## Command parameters { .section}

-m phase
:   \(Required\) Specifies the upgrade phase under which the utility is to run. There are four valid modes:

    Backup
    :   Backs up the Big SQL database.

        **Note:** The Backup phase removes Big SQL from Ambari.

    Upgrade
    :   Upgrades your version of Big SQL.

    Finalize
    :   Finalizes the Big SQL upgrade.

        CAUTION:

        After the upgrade is finalized, the backups of the catalog, metadata and configuration information of Big SQL are cleaned up and are no longer available.

    Restore
    :   Restores the upgrade to the previously backed-up version. Used in the event of an upgrade failure.

        **Important:** The restore phase is optional and must be run only in case of failure of the upgrade phase. The bigsql\_upgrade.py script prevents you from running the restore phase if the upgrade phase is successful.

-u ambari-admin-user
:   \(Optional\) Specifies the Ambari admin user name. The default is admin.

-p ambari-admin-password
:   \(Optional\) Specifies the Ambari admin password. The default is admin.

-s protocol
:   \(Optional\) Specifies the Ambari server protocol \(http or https\) . The default is http.

-P server-port
:   \(Optional\) Specifies Ambari server port number. The defaults is 8080.

-k kerberos-admin-principal
:   \(Optional\) Specifies the Kerberos admin principal. This is required if Kerberos is enabled on the cluster.

-w kerberos-admin-password
:   \(Optional\) Specifies the Kerberos admin password. This is required if Kerberos is enabled on the cluster.

-a ssh-user
:   Specifies the identity used for non-root access to Ambari. The default is root.

## Usage notes { .section}

**The Big SQL service configuration contains the credentials**ambari-admin-user and ambari-admin-password to connect Big SQL to the Ambari server. The Big SQL upgrade script bigsql\_upgrade.py requires a user name and password to connect to Ambari. We highly recommended you use the same user name as that in the Big SQL service configuration. If you wish to use a different set of credentials, you must make sure that the ones in the Big SQL service configuration are valid \(and in particular that the password is not expired\).

**The Big SQL administrator can upgrade a Kerberos-enabled cluster without disabling Kerberos**. Make sure that the Kerberos administrator credentials are available. If your cluster is Kerberos enabled, and valid credentials are not provided during the upgrade process, the upgrade will not proceed.

**Upgrade is supported with the following limitations**:

-   Big SQL high availability clusters. You must disable Big SQL high availability before performing an upgrade.
-   Ambari configuration groups are not supported. The upgrade script produces a warning message if you use configuration groups. If you override the warning,  you must validate that the configuration of all nodes in all configuration groups is updated as part of the upgrade. It is recommended that you remove all configuration groups before you run the upgrade.
-   You cannot upgrade through the Ambari dashboard; it is a command-line upgrade only.
-   Big SQL Yarn & Slider support. You must disable Yarn & Slider support for Big SQL before performing an upgrade.

**Note:** During the Big SQL migration process, BigSQL is fully removed from Ambari during most of the migration until the finalize step, discussed in [Upgrading and finalizing Big SQL](migrate_up_bigsql.md#). You can not interact with BigSQL through Ambari \(whether the web UI or REST API\).

**If you perfom an offline upgrade**, make sure that the repoinfo.xml file is updated. If you upgrade from version 4.1.0.2 to version 4.2.2, make sure that the source and the target versions of the repo information are present in the file. Use the following example as a guide to what the file might look like:

```

<reposinfo>  
<mainrepoid>IOP-4.2</mainrepoid>  
<os family="redhat6">
  <repo>
    <baseurl>http://birepo-build.svl.ibm.com/repos/IOP/RHEL6/x86_64/4.2/20160401_0401</baseurl>
    <repoid>IOP-4.2</repoid>
      <reponame>IOP</reponame>
  </repo>
  <repo>
    <baseurl>http://birepo-build.svl.ibm.com/repos/IOP-UTILS/RHEL6/x86_64/1.1</baseurl>
    <repoid>IOP-UTILS-1.1</repoid>
      <reponame>IOP-UTILS</reponame>
  </repo>
  <repo>
    <baseurl>http://birepo-build.svl.ibm.com/repos/BigInsights-Valuepacks/RHEL6/x86_64/4.2.0.0/DEV/20160419_1025</baseurl>
    <repoid>BIGINSIGHTS-VALUEPACK-1.2.0.0</repoid>
    <reponame>BIGINSIGHTS-VALUEPACK-1.2.0.0</reponame>
   </repo>
  </os>
</reposinfo>
```

## Examples { .section}

Example scenario:
:   For this example, assume the following criteria:

-   The Ambari dashbard is https://ambari.company.com:9443/.
-   The administrator user name is ambadmin and the password is ambpassword.
-   Kerberos is not enabled.
-   Ambari is set up for non-root access with an ambari user.
-   You are logged in as the root user.

Run the 'Backup' phase of the Big SQL service upgrade process on your cluster:

```

/usr/ibmpacks/scripts/5.0.1.0/upgrade/BIGSQL/bigsql_upgrade.py 
   -u ambadmin -p ambpassword -s https -P 9443 -a ssh_user -m Backup
```

