# Cleaning up IBM value-add services {#task_r5h_kdq_5r .task}

This topic describes how to remove IBM value-added services \(such as Text Analytic\) from your system as part of a migration.

These clean-up processes do not remove the Ambari server, nor do they impact any of the configurations. The scripts will remove the top-level value-add RPMs. The value-add components you might see include:

-   Text Analytics \(component name: TEXTANALYTICS\)
-   BigInsights Home \(component name: WEBUIFRAMEWORK\)
-   BigSheets \(component name: BIGSHEETS\)
-   Big R \(component name: BIGR\)
-   Data service manager \(component name: DATASERVERMANAGER\)

**Note:** The cleanup process for removing BigSheets removes data on HDFS for the child workbooks. If you want to save any of the child workbook data, use the **Export Data** option from the BigSheets home page for each of the child workbooks and save the data on HDFS. For information on how to export data, see [Exporting data from a BigSheets workbook](https://www.ibm.com/support/knowledgecenter/SSPT3X_4.2.5/com.ibm.swg.im.infosphere.biginsights.analyze.doc/doc/t0057549.html).

In the Ambari UI, when you navigate to the service page for a service, the last action listed under **Service Actions** is **Delete Service**. The **Delete Service** option should NOT be used for these services, as it can leave the service in an undefined state. The proper way to remove any of these four services is to follow the steps on this page.  If you already used the **Delete Service** option for one of the services, you still must follow the steps on this page for proper removal of the service. In such a scenario, you will see a message like the following during removal \(using Text Analytics as an example\). You must type in y or Y to finish the removal successfully.

```
Current TEXTANALYTICS status: EMPTY
Unable to detect TEXTANALYTICS in Ambari. It may have already been removed.
Would you like to continue cleanup? (Y/n) y
```

**Important:** DO NOT remove Big SQL using the script described in this topic.

1.  Navigate to the directory that contains the clean up scripts:

    ```
    cd /usr/ibmpacks/bin/<version>
    ```

    **Note:** The <version\> numbers you see will start with 3.0, not 5.0.0.1.

2.  The value-add services include scripts to help you remove the value-add services and to clean up your environment:

        |**Service cleanup**|    ```

remove_value_add_services.sh 
  -u <AMBARI_ADMIN_USERNAME> 
  -p <AMBARI_ADMIN_PASSWORD> 
  -x <AMBARI_PORT>   
  -a <STOPSERVICECOUNT> 
  -b <REMOVESERVICECOUNT>  
  -r 
  -l  
  -c <RUN_AS_USER>
  -f
  -s
  -q
    ```

|

    Use the following parameter definitions:

    -u
    :   The Ambari administrator user name.

    -p
    :   The Ambari administrator password.

    -x
    :   The Ambari server port number.

    -s
    :   The service to remove. The following values are allowed:

        Service
        :   -   TEXTANALYTICS
-   WEBUIFRAMEWORK - This is the BigInsights Home.
-   BIGSHEETS
-   BIGR
-   DATASERVERMANAGER
    -f
    :   This option is optional. The FORCE option allows you to continue removing the service, even if intermittent steps fail.

        **Warning:** This might result in an Ambari unknown service state.

    -q
    :   This option is optional. If removing a service, the parameter specifies to remove stack files that are associated with the service.

        **Warning:** When running the remove service script, this option prevents a reinstallation.

    -a
    :   This option is optional. Specifies the number of attempts to stop the service.

    -b
    :   This option is optional. Specifies the number of attempts to remove the service.

    -r
    :   \(optional\) Removes service users.

    -l
    :   This option is optional. It enables secure https mode.

    -c
    :   This option is optional. Run as user if a non-root user is installed.

    **Note:** For the Text Analytics service -TEXTANALYTICS- if an existing MySQL database server was used during install, then delete the database manually from the server after running the removal script:

    `mysql> drop database tawebtoolingdb;`

3.  Complete the removal process with the following steps:
    1.  Restart the Ambari service to make sure that the cache is cleared:

        **Note:** Some previously launched services, such as Knox, may take several minutes to refresh. Ambari may display these services in a warning state and attempts to manually launch these services may fail during the refresh.

        ```
        sudo ambari-server restart
        ```

    2.  There are files that are left in /var/lib/ambari-server/resources/stacks/BigInsights/4.2.5/services/$SERVICE/package/archive.zip. These can remain and have no impact on future service additions.

Service cleanup examples:
:   Normal removal
:   Remove the BigR service:

    ```
    sudo remove_value_add_services.sh 
      -u admin -p admin -x 8081 BIGR
    ```

Removal including users
:   Remove the Big R service including the users:

    ```
    sudo remove_value_add_services.sh 
      -u admin -p admin -x 8081 BIGR -r
    ```

Run as non-root user
:   Remove the Big R service as non-root user \(with sudo privilege\) biadmin:

    ```
    sudo remove_value_add_services.sh 
      -u admin -p admin -x 8081 BIGR 
      -r  -c biadmin
    ```

The next step in the migration process is to remove deprecated components and services. See [Removing deprecated components and services](clean_components.md#) for details.

