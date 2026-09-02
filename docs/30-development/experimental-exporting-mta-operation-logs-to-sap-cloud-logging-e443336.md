<!-- loioe443336a35d641d4a3da9b74d8451a07 -->

# \(Experimental\) Exporting MTA Operation Logs to SAP Cloud Logging

Export the operation logs of your multitarget application to SAP Cloud Logging, so that you can store, visualize, and analyze them together with the rest of your observability data.



## Prerequisites

> ### Note:  
> This is an experimental feature. Experimental features aren't part of the officially delivered scope that SAP guarantees for future releases. For more information, see [Important Disclaimers and Legal Information](https://help.sap.com/viewer/disclaimer).
> 
> Please use the *Feedback* button in this topic to let us know what you like and don't, and how we can improve it to make the experience more enjoyable for you.

-   You have an SAP Cloud Logging service instance. For more information, see [SAP Cloud Logging](https://help.sap.com/docs/SAP_CLOUD_LOGGING/454331d80e3b42b1804d83a672cf098b/834217698bdb47609642ab4001734251.html?locale=en-US).
-   The service instance is reachable from the same Cloud Foundry API endpoint that you use to deploy the multitarget application. A multitarget application can only export its operation logs to an SAP Cloud Logging service instance that is available under the same Cloud Foundry API endpoint as the deployment. For the available Cloud Foundry API endpoints, see [Regions and API Endpoints Available for the Cloud Foundry Environment](https://help.sap.com/docs/btp/sap-business-technology-platform/regions-and-api-endpoints-available-for-cloud-foundry-environment?locale=en-US).
-   You have created a service key for the SAP Cloud Logging service instance that provides mutual TLS ingestion credentials. See [Create a Service Key](https://help.sap.com/docs/cloud-logging/cloud-logging/create-sap-cloud-logging-instance-through-sap-btp-cockpit?locale=en-US&version=Cloud&ai=true#create-a-service-key).
-   The user who starts the multitarget application deployment has access to the SAP Cloud Logging service instance. If the instance resides in a different space, ensure that this user has the **Space Developer** role in that space as well. For more information, see [About Roles in the Cloud Foundry Environment](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/09076385086b4da3bd1808d5ef572862.html?locale=en-US).



## Context

When your multitarget application \(MTA\) is deployed, undeployed, or otherwise processed, the SAP Cloud Deployment service produces operation logs that describe the progress and outcome of the operation. By default, these logs are available for the retention period of the operation and can be retrieved with the multitarget application commands, for example by listing operations with `mta-ops` and downloading their logs with `download-mta-op-logs`. For more information, see [MTA Operations History and Logs](mta-operations-history-and-logs-c55d858.md).

By adding a resource of type `org.cloudfoundry.cloud-logging-service` to your deployment descriptor, you instruct the SAP Cloud Deployment service to also forward these operation logs to an SAP Cloud Logging service instance. The SAP Cloud Deployment service reads the credentials of a service key that belongs to the instance and uses them to establish a mutual TLS connection to the ingestion endpoint. You control which service key is used, the verbosity of the exported logs, and, optionally, the organization and space in which the instance resides.



## Procedure

1.  In your deployment descriptor \(`mtad.yaml`\), add a resource of type `org.cloudfoundry.cloud-logging-service`.

    > ### Tip:  
    > You can mark the resource as optional by setting `optional: true`. In this case, a missing or invalid SAP Cloud Logging configuration does not cause the deployment to fail. The same applies to runtime problems while exporting the logs, such as connectivity issues or rate limiting on the side of the SAP Cloud Logging service instance. As a result, some operation logs might not appear in your SAP Cloud Logging service instance, but the operation logs remain available through the multitarget application commands, such as `mta-ops` and `dmol`.

2.  Set the `service-name` parameter to the name of your SAP Cloud Logging service instance.

3.  Set the `service-key-name` parameter to the name of the service key that holds the ingestion credentials.

4.  **Optional:** Set the `log-level` parameter to control the verbosity of the exported operation logs.

    The supported values are `INFO`, `WARN`, `DEBUG`, `ERROR`, and `TRACE`. If you do not specify a value, `INFO` is used.

    > ### Note:  
    > The log levels are hierarchical. For example, `TRACE` exports logs of all levels, whereas `ERROR` exports only error-level logs. A more verbose log level increases the volume of data that is ingested into your SAP Cloud Logging service instance, which affects its consumption and eventual costs.

5.  **Optional:** If the SAP Cloud Logging service instance resides in a different organization or space than the one you deploy to, set the `destination` parameter with the `org-name` and `space-name` of the target organization and space.

    If you omit `destination`, the organization and space of the current deployment are used.

    > ### Sample Code:  
    > ```
    > resources:
    >   - name: my-cloud-logging
    >     type: org.cloudfoundry.cloud-logging-service
    >     parameters:
    >       service-name: my-cloud-logging-instance
    >       service-key-name: my-service-key
    >       log-level: INFO
    >       destination:
    >         org-name: my-org
    >         space-name: my-space
    > ```

6.  Deploy your multitarget application.

    For more information, see [Multitarget Application Commands for the Cloud Foundry Environment](../50-administration-and-ops/multitarget-application-commands-for-the-cloud-foundry-environment-65ddb1b.md).

7.  Verify that the operation logs of the deployment appear in your SAP Cloud Logging service instance.




## Next Steps

Each operation has a unique operation ID, which is exported to your SAP Cloud Logging service instance as the correlation ID of the operation logs. You can use this operation ID to filter the logs in SAP Cloud Logging and view only the entries that belong to a specific deployment operation.

**Related Information**  


 <?sap-ot O2O class="- topic/link " href="69da5ac68bec4ff69b4e1b2900685f2c.xml" text="" desc="" xtrc="link:1" xtrf="file:/home/builder/src/dita-all/jjq1673438782153/loio2080d0faf9d84ce6aa14caa4caa32935_en-US/src/content/localization/en-us/e443336a35d641d4a3da9b74d8451a07.xml" output-class="" outputTopicFile="file:/home/builder/tp.net.sf.dita-ot/2.3/plugins/com.elovirta.dita.markdown_1.3.0/xsl/dita2markdownImpl.xsl" ?> 

[Multitarget Application Commands for the Cloud Foundry Environment](../50-administration-and-ops/multitarget-application-commands-for-the-cloud-foundry-environment-65ddb1b.md "A list of additional commands to deploy multitarget applications (MTA) to the Cloud Foundry environment.")

[List of Supported MTA Parameters](list-of-supported-mta-parameters-6aa426e.md "A list of all the supported parameters for MTA deployment in the Cloud Foundry environment.")

[Resources](resources-9e34487.md "The application modules defined in the “modules” section of the deployment descriptor may depend on resources.")

