# Upgrading and finalizing Big SQL {#migrate_up_bigsql .task}

As a step in an IOP to HDP Big SQL migration, you must upgrade and finalize your version of Big SQL.

If you are performing this step as part of a migration, you must follow all the preceding steps described in [Migrating from IOP to HDP](migrate_process.md#).

The HDFS, Hive, and HBase services \(and the services they depend on\) must be running for you to perform the Big SQL upgrade. HDFS should not be running in safe mode.

If you are upgrading Big SQL in a non-root installation environment, you must follow the steps in [hdp\_ambari\_non\_root\_access\_bigsql.md\#](hdp_ambari_non_root_access_bigsql.md#).

This topic describes how to upgrade and finalize your version of Big SQL.

1.  Ensure the umask for the root and bigsql users is 0022. Run the following commands on each node on the cluster as the root user or a user with sudo privileges to run the following commands as root:

    ```
    umaskmigrate_prep_non_root
    sudo su - root -c umask
    sudo su - bigsql -c umask
    ```

2.  Disable TCP/IP connections to the Big SQL service.Perform the following steps on the BigSQL HeadNode as the BigSQL user:

    ```
    db2set DBCOMM=
    /usr/ibmpacks/current/bigsql/bigsql/bin/bigsql stop
    /usr/ibmpacks/current/bigsql/bigsql/bin/bigsql start
    ```

3.  Stop db2 level auditing of Big SQL.Login to the headnode as the Big SQL user and issue the following commands to stop auditing:

    ```
    db2audit stop
    ```

    After the upgrade has been completed, you can restart auditing from the Big SQL user on the headnode by issuing the command

    ```
    db2audit start
    ```

    .

4.  Use the Upgrade option of the bigsql\_upgrade.py script to upgrade your version of Big SQL.

    The upgrade phase takes the following actions on all nodes of the cluster:

    -   Upgrades the Big SQL catalog, metadata and configuration information to the new version of Big SQL.
    ```
    python /usr/ibmpacks/scripts/5.0.1.0/upgrade/bigsql_upgrade.py -m Upgrade
    ```

    To perform the upgrade option of the Big SQL upgrade, run the bigsql\_upgrade.py script with the -m option and the value Upgrade. Include any additional options as documented in [bigsql\_upgrade.py - Big SQL upgrade utility](upgrade_bigsql_py.md#) For example, if you have configured Ambari for non-root access, you should use the -a option.

    When the Upgrade phase is complete, the Big SQL service is not visible in the Ambari dashboard. However, the new version of BigInisghts Big SQL is operational and running. It is possible to connect applications to the BigInisghts Big SQL server to run sanity tests before proceeding with the Finalize phase of the upgrade.

    It is not possible to re-run the upgrade phase immmediately after it has completed \(succesfully or not\).

5.  In case the previous upgrade step fails, follow these steps:
    1.  Consult the script output or the upgrade log located at /var/ibm/bigsql/logs/upgrade.log to identify the problem.
    2.  Use the Restore option of the [bigsql\_upgrade.py](upgrade_bigsql_py.md#) script to restore to pre-upgrade conditions:

        ```
        python /usr/ibmpacks/scripts/5.0.1.0/upgrade/bigsql_upgrade.py -m Restore
        ```

    3.  Repair the issue that caused the failure.
    4.  Re-run the upgrade command as shown in Step 1.
6.  Enable the Big SQL service extension by running the command:

    ```
    EnableBigSQLExtension.py
    ```

    For details on this script, see [enable\_bigsql\_extension.md\#](enable_bigsql_extension.md#).

7.  Clean up the BIGSQL and DSM service from the old stack:Run the following commands:

    ```
    rm -rf /var/lib/ambari-server/resources/stacks/HDP/2.4/services/BIGSQL /var/lib/ambari-server/resources/stacks/HDP/2.5/services/BIGSQL /var/lib/ambari-server/resources/stacks/HDP/2.6/services/BIGSQL
    
    rm -rf /var/lib/ambari-server/resources/stacks/HDP/2.4/services/DATASERVERMANAGER /var/lib/ambari-server/resources/stacks/HDP/2.5/services/DATASERVERMANAGER /var/lib/ambari-server/resources/stacks/HDP/2.6/services/DATASERVERMANAGER
    ```

8.  Use the Finalize option of the bigsql\_upgrade.py script to finalize your upgrade of Big SQL.

    CAUTION:

    After the upgrade is finalized, the backups of the catalog, metadata and configuration information of Big SQL are cleaned up and are no longer available.

    The finalize phase takes the following actions on all nodes of the cluster:

    1.  Registers the Big SQL service in the Ambari dashboard.
    2.  Cleans up the binaries of the previous Big SQL version.
    3.  Cleans up the backups that were created during the backup phase.
    To perform the finalize phase of the Big SQL upgrade, run the bigsql\_upgrade.py script with the -m option and the value Finalize. Include any additional options as documented in the [bigsql\_upgrade.py - Big SQL upgrade utility](upgrade_bigsql_py.md#). For example, if you have configured Ambari for non-root access, you should use the -a option.

    ```
    python /usr/ibmpacks/scripts/5.0.1.0/upgrade/bigsql_upgrade.py -m Finalize
    ```

    When the Finalize phase is complete, the backups no longer exist. The Big SQL service is visible in the Ambari dashboard. The new version of Big SQL is operational and running.

    In case of failure of the finalize phase, consult the script output or the upgrade log located at /var/ibm/bigsql/logs/upgrade.log to identify and resolve the problem. After it is resolved, re-run the finalize phase.

9.  Re-enable your TCP/IP connections by logging on as the Big SQL user to the head node and issuing the following command:

    ```
    db2set DB2COMM=TCPIP
    /usr/ibmpacks/current/bigsql/bigsql/bin/bigsql stop
    /usr/ibmpacks/current/bigsql/bigsql/bin/bigsql start
    ```


The next step in the migration process is to upgrade DSM. See [Upgrading IBM Data Server Manager \(DSM\)](migrate_up_dsm.md#) for details.

