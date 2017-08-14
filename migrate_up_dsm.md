# **Upgrading IBM Data Server Manager \(DSM\)** {#migrate_up_dsm .task}

**Reviewers:** This file is brand new as of Aug 14 and not yet reviewed.

As the final step in an IOP to HDP Big SQL migration, you must upgrade the IBM DSM.

If you are performing this step as part of a migration, you must follow all the preceding steps described in [Migrating from IOP to HDP](migrate_process.md#).

This topic describes how to upgrade your version of DSM.

1.  In the Ambari UI, Add DSM back to the cluster:
    1.  Select **Actions** \> **+Add Service**.
    2.  In the Add Service Wizard, in the **Choose Services** pane, select **Data Service Manager**.
    3.  Click **Next**.
    4.  Proceed with any remaining steps in the Wizard to finish adding DSM.
2.  Make a backup copy of the default\_rep\_db to /usr/ibmpacks/IBM-DSM/5.0.1.0/ibm-datasrvrmgr/Config.
3.  Make a backup copy of privileges.json \(if the file exists\) to /usr/ibmpacks/IBM-DSM/5.0.1.0/ibm-datasrvrmgr/Config.
4.  In Ambari, restart DSM by selecting DSM in the list of services and then selecting **Service Actions** \> **Stop**.

