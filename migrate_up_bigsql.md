# Upgrading and finalizing Big SQL {#migrate_up_bigsql .task}

As the final step in an IOP to HDP Big SQL migration, you must upgrade and finalize your version of Big SQL.

If you are performing this step as part of a migration, you must follow all the preceding steps described in [Migrating from IOP to HDP](migrate_process.md#).

This topic describes how to upgrade and finalize your version of Big SQL.

1.  Install the 5.0.1 Big SQL service definition:
    1.  Obtain the Big SQL package by downloading it from [http://birepo-build.svl.ibm.com/repos/BigSQL/RHEL7/x86\_64/5.0.0.0/build.latest/bigsql\_5.0.0.0.el7.x86\_64.bin](http://birepo-build.svl.ibm.com/repos/BigSQL/RHEL7/x86_64/5.0.0.0/build.latest/bigsql_5.0.0.0.el7.x86_64.bin).
    2.  Execute the downloaded file; choose the **migration** option when asked. See [hdp\_valaddinst.md\#](hdp_valaddinst.md#) for details.
2.  Use the Upgrade option of the bigsql\_upgrade.py script to upgrade your version of Big SQL:

    ```
    python /usr/ibmpacks/scripts/5.0.1.0/upgrade/BIGSQL/bigsql_upgrade.py -m Upgrade
    ```

3.  In case the previous upgrade step fails, follow these steps:
    1.  Consult the script output or the upgrade log located at /var/ibm/bigsql/logs/upgrade.log to identify the problem.
    2.  Use the Restore option of the bigsql\_upgrade.py script to restore to per-upgrade conditions:

        ```
        python /usr/ibmpacks/scripts/5.0.1.0/upgrade/BIGSQL/bigsql_upgrade.py -m Restore
        ```

    3.  Repair the issue that caused the failure.
    4.  Re-run the bigsql\_upgrade.pt -m Upgrade command as in Step 2.
4.  Clean up the BIGSQL & DSM service from the old stack:Run the following commands:

    ```
    rm -rf /var/lib/ambari-server/resources/stacks/HDP/2.4/services/BIGSQL /var/lib/ambari-server/resources/stacks/HDP/2.5/services/BIGSQL /var/lib/ambari-server/resources/stacks/HDP/2.6/services/BIGSQL
    
    rm -rf /var/lib/ambari-server/resources/stacks/HDP/2.4/services/DATASERVERMANAGER /var/lib/ambari-server/resources/stacks/HDP/2.5/services/DATASERVERMANAGER /var/lib/ambari-server/resources/stacks/HDP/2.6/services/DATASERVERMANAGER
    ```

5.  Enable the Big SQL service extension by running the command:

    ```
    EnableBigSQLExtension.py
    ```

6.  Use the Finalize option of the bigsql\_upgrade.py script to finalize your upgrade of Big SQL:

    ```
    python /usr/ibmpacks/scripts/5.0.1.0/upgrade/BIGSQL/bigsql_upgrade.py -m Finalize
    ```


