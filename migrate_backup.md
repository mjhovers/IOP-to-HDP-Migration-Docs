# Backing up metadata {#migrate_backup .task}

It is critical to back up your metadata before performing a migration.

The following are prerequisites for backing up your meta data:

-   You must disable all high availability.
-   Ambari configuration groups are not supported. The upgrade script produces a warning message if you use configuration groups. If you override the warning, you must validate that the configuration of all nodes in all configuration groups is updated as part of the upgrade. It is recommended that you remove all configuration groups.
-   You must disable Yarn and Slider support for Big SQL.

Provides information on how to back up:

-   Data Service Manager \(DSM\) metadata
-   Big SQL metadata using the Big SQL python script bigsql\_upgrade.py

1.  **Install the Big SQL version 5.0.1 service definition**:
    1.  Obtain the IBM Big SQL package . See [hdp\_obtain.md\#](hdp_obtain.md#) for details.
    2.  Execute the downloaded file; choose the **migration** option when asked. See [hdp\_valaddinst.md\#](hdp_valaddinst.md#) for details.
    3.  After the downloaded file has been executed, DO NOT enable the Big SQL 5.0.1 extension with the EnableBigSQLExtension.py script.
2.  **Back up the Ambari Server database**. For details, **\[HWX REFERENCE\]**.
3.  **Run the backup option of the Big SQL python script**. \(For details on the script, see [bigsql\_upgrade.py - Big SQL upgrade utility](upgrade_bigsql_py.md#).\)
    1.  Back up your Big SQL environment by running the Backup option of the bigsql\_upgrade.py python command. The Backup option performs the following actions on all nodes of the cluster:

        -   Backs up the Big SQL catalog, metadata and configuration information
        -   Installs the binaries for the new version
        -   Temporarily disconnects the Big SQL service definition from Ambari
        To perform the backup phase of the upgrade, run the bigsql\_upgrade.py script with the -m option and the value Backup. Include any additional options as documented in the [bigsql\_upgrade.py - Big SQL upgrade utility](upgrade_bigsql_py.md#). For example, if you have configured Ambari for non-root access, you should use the -a option.

        ```
        python /usr/ibmpacks/scripts/5.0.1.0/upgrade/bigsql_upgrade.py -m Backup
        ```

    2.  When the Backup phase is complete, the Big SQL service is no longer visible in the Ambari dashboard. However, it is operational, but not running. If needed, you can start the service from the command line and use it. In this case, the version executed is the initial Big SQL version.
4.  \(Optional in case the backup phase fails\) Consult the master log output or the upgrade log located at /var/ibm/bigsql/logs/upgrade.log to identify and resolve the problem. After it is resolved, re-run the backup phase.
5.  **Backup DSM metadata**. Save a copy of the following folders to a backup directory, such as /tmp:

    -   /usr/ibmpacks/IBM-DSM/$VERSION/ibm-datasrvrmgr/Config/default\_rep\_db
    -   /usr/ibmpacks/IBM-DSM/$VERSION/ibm-datasrvrmgr/Config/privileges.json
    For example:

    ```
    mkdir /tmp/dsm; cd /tmp/dsm
    cp -rf /usr/ibmpacks/IBM-DSM/$VERSION/ibm-datasrvrmgr/Config/default_rep_db .
    cp /usr/ibmpacks/IBM-DSM/$VERSION/ibm-datasrvrmgr/Config/privileges.json .
    ```


The next step in the migration process is to upgrade your version of Ambari. See **\[HWX REFERENCE\]** for details.

