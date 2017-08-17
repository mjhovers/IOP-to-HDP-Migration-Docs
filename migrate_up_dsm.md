# **Upgrading IBM Data Server Manager \(DSM\)** {#migrate_up_dsm .task}

As the final step in an IOP to HDP Big SQL migration, you must upgrade the IBM DSM.

If you are performing this step as part of a migration, you must follow all the preceding steps described in [Migrating from IOP to HDP](migrate_process.md#).

**Note:** Data Server Manager requires Knox to be installed, configured, and started. For more information, see [../../com.ibm.swg.im.bigsql.admin.doc/doc/admin\_knox\_enable.md\#](../../com.ibm.swg.im.bigsql.admin.doc/doc/admin_knox_enable.md#).

This topic describes how to upgrade your version of DSM.

1.  In the Ambari UI, [re-install DSM on the cluster](hdp_dsm_inst.md#).Two important notes while re-installing DSM:
    -   Ignore the "Before you begin" portion of the DSM installation topic
    -   Click OK when asked if Ambari can install TEZ
2.  Restore the backup copy of the default\_rep\_db to /usr/ibmpacks/IBM-DSM/5.0.1.0/ibm-datasrvrmgr/Config.
3.  Restore the backup copy of privileges.json \(if the file exists\) to /usr/ibmpacks/IBM-DSM/5.0.1.0/ibm-datasrvrmgr/Config.
4.  Ensure the ownership on the restored file privileges.json, the restored directory default\_rep\_db, and all of its contents match the rest of the files under the directory /usr/ibmpacks/IBM-DSM/5.0.1.0/ibm-datasrvrmgr/Config.
5.  In Ambari, restart DSM by selecting DSM in the list of services and then selecting **Service Actions** \> **Restart All**.

