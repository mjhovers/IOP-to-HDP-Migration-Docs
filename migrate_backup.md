# Backing up metadata {#migrate_backup .task}

Provides information on how to back up your Big SQL metadata using the Big SQL upgrade script.

Provides information on how to back up your Big SQL metadata using the Big SQL python script bigsql\_upgrade.py.

1.  Install the Big SQL version 5.0.1 service definition:
    1.  **Obtain the IBM Big SQL package** . Download the package from [http://birepo-build.svl.ibm.com/repos/BigSQL/RHEL7/x86\_64/5.0.0.0/build.latest/bigsql\_5.0.0.0.el7.x86\_64.bin](http://birepo-build.svl.ibm.com/repos/BigSQL/RHEL7/x86_64/5.0.0.0/build.latest/bigsql_5.0.0.0.el7.x86_64.bin).
    2.  Execute the downloaded file; choose the **migration** option when asked. See [Installing the IBM Big SQL package](hdp_valaddinst.md#) for details.
    3.  After the downloaded file has been executed, DO NOT enable the Big SQL 5.0.1 extension with the EnableBigSQLExtension.py script.
2.  **Run the backup option of the Big SQL python script**.
    1.  Back up your Big SQL environment by running the Backup option of the bigsql\_upgrade.py python command. The backup option performs the following actions on all nodes of the cluster:

        -   Backs up the Big SQL catalog, metadata and configuration information.
        -   Installs the binaries for the new version.
        Run the command:

        ```
        /usr/ibmpacks/scripts/5.0.1.0/upgrade/bigsql_upgrade.py -m Backup
        ```

    2.  After the backup is complete, verify Big SQL no longer appears in the Ambari user interface \(UI\).
3.  **Remove the Big SQL 5.0.1 service definition from your environment**.

    -   The bin file installs the file IBM-Big\_SQL.noarch. Delete this RPM file.
    -   If the directory /var/lib/ambari-server/resources/extensions exists on your system, delete it.
    If you are performing a migration, you must also back up your SOLR metadata; see **\[HWX REFERENCE\]** for details.


The next step in the migration process is to upgrade your version of Ambari. See **\[HWX REFERENCE\]** for details.

