# Cleaning up IBM IOP value-add services {#task_r5h_kdq_5r .task}

This topic describes how to remove IBM IOP value-added services \(such as Text Analytic\) from your system as part of a migration.

These clean-up processes do not remove the Ambari server, nor do they impact any of the configurations. The scripts will remove the top-level value-add RPMs. The value-add components you might see include:

-   Text Analytic \(component name: TEXTANALYTICS\)
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

1.  Navigate to the directory that contains the clean up scripts:

    ```
    cd /usr/ibmpacks/bin/<version>
    ```

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
    |**Assembly cleanup**|    ```

remove_value_add_services_and_assembly.sh 
  -A  <this is required>
  -u <AMBARI_ADMIN_USERNAME> 
  -p <AMBARI_ADMIN_PASSWORD> 
  -x <AMBARI_PORT>    
  -q
  -l  
  -c <RUN_AS_USER>
  -f
  
    ```

|
    |**Service and assembly cleanup**|    ```

remove_value_add_services_and_assembly.sh 
  -u <AMBARI_ADMIN_USERNAME> 
  -p <AMBARI_ADMIN_PASSWORD> 
  -x <AMBARI_PORT>   
  -a <STOPSERVICECOUNT> 
  -b <REMOVESERVICECOUNT>
  -q 
  -r 
  -l  
  -c <RUN_AS_USER>
  -f
 
    ```

|

    Use the following parameter definitions:

    -A
    :   This option is mandatory when performing assembly only removal by using the remove\_value\_add\_services\_assembly.sh script.

    -u
    :   The Ambari administrator user name.

    -p
    :   The Ambari administrator password.

    -x
    :   The Ambari server port number.

    -s
    :   Depending on the script that you run, the service to remove, or the service assembly to remove. The following values are allowed:

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
    :   This option is optional. If removing a service, the parameter specifies to remove stack files that are associated with the service. If removing assembly, the parameter specifies to remove the yum repo and the cache.

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

    **Note:**

    For the Text Analytics service -TEXTANALYTICS- if an existing MySQL database server was used during install, then delete the database manually from the server after running the removal script:

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
    >sudo remove_value_add_services.sh 
      -u admin -p admin -x 8081 BIGR
    ```

Removal including users
:   Remove the Big R service including the users:

    ```
    >sudo remove_value_add_services.sh 
      -u admin -p admin -x 8081 BIGR -r
    ```

Run as non-root user
:   Remove the Big R service as non-root user \(with sudo privilege\) biadmin:

    ```
    >sudo remove_value_add_services.sh 
      -u admin -p admin -x 8081 BIGR 
      -r  -c biadmin
    ```

Assembly cleanup examples:
:   Normal removal
:   Remove the assembly with repo clear:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -A  -u admin -p admin -x 8081
    ```

Removal with repo clean:
:   Remove the assembly with repo clean:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -A -u admin -p admin -x 8081 -q
    ```

Run with repo clean and non root user:
:   Remove the Data Scientist assembly and run as user biadmin:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -A -u admin -p admin -x 8081 -q -c biadmin
    ```

Service and assembly cleanup examples:
:   Normal removal
:   Removal all:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -u admin -p admin -x 8081
    
    ```

:   Removal the Analyst services and assembly:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -u admin -p admin -x 8081 
    ```

Removal including users
:   Remove all services and assemblies and users:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -u admin -p admin -x 8081 -r
    ```

Run as non-root user
:   Remove all and run as user biadmin:

    ```
    >sudo remove_value_add_services_and_assembly.sh 
      -u admin -p admin -x 8081  -r  -c biadmin
    ```

Removal with repo clean and non-root user and removing service users
:   Removing the Data Scientist service and assemblies and running as user biadmin:

    ```
    >sudo remove_value_add_assembly.sh 
      -u admin -p admin -x 8081 -r -q -c biadmin
    ```

./remove\_value\_add\_services\_and\_assembly.sh
:   The usage is given as:

    ```
    
    ./remove_value_add_services_and_assembly.sh 
    -u <Ambari UI username> -p <Ambari UI password> [-x <Ambari server port number> -a <stop service attempts> -b <remove service attempts> -q -r -A]
    ```

    Required Parameters:

    -u <username\>
    :   Ambari UI username.

    -p <password\>
    :   Ambari UI password.

    Optional Parameters:

    -x <number\>
    :   Port number for the Ambari server \(can be read automatically\)

    -a <number\>
    :   Number of times to re-attempt removing services from Ambari.

    -b <number\>
    :   Number of times to re-attempt removing services from Ambari.

    -f
    :   Force.

    -q
    :   Remove YUM repo cleaning.

    -r
    :   Remove service users.

    -l
    :   Connect to Ambari using HTTPS.

    -A
    :   Assembly cleanup only \(use if services have already been removed\).

    **Examples**:

    ```
    remove_value_add_services_and_assembly.sh -u admin -p admin
    remove_value_add_services_and_assembly.sh -u admin -p admin -x 8081 -a 5 -b 10
    for secure Ambari servers(https): remove_value_add_services_and_assembly.sh -u admin -p admin -x 8443 -l
    ```

