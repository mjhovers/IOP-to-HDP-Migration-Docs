# Removing deprecated components and services {#clean_components .task}

Provides the steps required to remove the deprecated components and services which are not supported by Hortonworks Data Platform \(HDP\).

You must stop and remove all the deprecated services and components from the Ambari server.

1.  **Remove the HBASE REST SERVER component**. For each HBaseRestServer:
    1.  In Ambari, select the **Hosts** link
    2.  Select the host with the HBASE REST SERVER component running
    3.  From the Actions drop-down menu select **Stop**
    4.  From the Actions drop-down menu select **Delete** to delete the component
    5.  Delete the HBASE REST SERVER component using the following REST API as follows:

        ```
        curl -u <admin-username>:<admin-password> -H "X-Requested-By:ambari" -i -X DELETE 
        http://<ambari-server-host>:<port>/api/v1/clusters/<cluster-name>/services/HBASE/components/HBASE_REST_SERVER
        
        curl -u admin:admin -H "X-Requested-By:ambari" -i -X DELETE 
        http://iop1:8080/api/v1/clusters/iop/services/HBASE/components/HBASE_REST_SERVER
        
        ```

2.  **Remove the TITAN, SOLR, JNBG, SYSTEMML and R4ML services in order** \(if present\). To remove a component, follow these steps:

    1.  In Ambari, select the service \(for example, TITAN\) in the **Services** list.
    2.  Select **Service Actions** \> **Stop** to stop the service.
    3.  Select **Service Actions** \> **Delete** to delete the service.
    Repeat this process for all the services listed above.


The next step in the migration process is to migrate from IOP to HDP. See **\[HWX REFERENCE\]** for details.

