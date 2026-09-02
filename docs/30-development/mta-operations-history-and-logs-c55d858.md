<!-- loioc55d858ed5b449b3ac5ee29738c265e4 -->

# MTA Operations History and Logs

When the SAP Cloud Deployment service processes a multitarget application \(MTA\), it keeps a history of the operations and produces operation logs that record the progress and outcome of each operation.

Every time an MTA is deployed, undeployed, or otherwise processed, the SAP Cloud Deployment service creates an operation and writes operation logs for it. You can view the history of the recent MTA operations, including their unique operation IDs, status, and other useful information. If you need to inspect a deployment or perform troubleshooting, you can retrieve this history and the associated operation logs to understand the progress of an operation and investigate failures.

You can work with the history and logs of an MTA operation in the following ways:

-   List the recent operations and download their logs by using multitarget application commands, as described below.
-   Export the operation logs to an SAP Cloud Logging service instance, so that you can store, visualize, and analyze them together with the rest of your observability data. For more information, see [\(Experimental\) Exporting MTA Operation Logs to SAP Cloud Logging](experimental-exporting-mta-operation-logs-to-sap-cloud-logging-e443336.md).

> ### Note:  
> The output of the commands shown on this page is provided as an example and might differ over time.



<a name="loioc55d858ed5b449b3ac5ee29738c265e4__section_retention"/>

## Retention

The operation logs are available for the retention period of the operation. The expiration time for all MTA operations in the Cloud Foundry environment is 3 days. When this time limit is reached, the operation and its logs are no longer available, and an operation that is still active is automatically aborted.

If you want to keep the operation logs beyond the retention period, download them before the operation expires, or export them to an SAP Cloud Logging service instance, where they are subject to the retention of that service.



<a name="loioc55d858ed5b449b3ac5ee29738c265e4__section_history"/>

## Viewing the Operations History

Use the `mta-ops` command of the MultiApps CF CLI plugin to list the recent operations of a multitarget application. The command shows the operations together with their unique operation IDs, status, and other useful information, which you can use to inspect a deployment or perform troubleshooting.

> ### Sample Code:  
> ```
> cf mta-ops --mta my-mta
> ```

You can use the operation ID from the history to download the logs of a specific operation, as described below.



<a name="loioc55d858ed5b449b3ac5ee29738c265e4__section_download"/>

## Downloading the Operation Logs

Use the `download-mta-op-logs` command \(alias `dmol`\) of the MultiApps CF CLI plugin to download the log files of one or more MTA operations. If you do not know the ID of the operation whose logs you want, first list the operations with the `mta-ops` command, as described above.

Download the operation logs by operation ID:

> ### Sample Code:  
> ```
> cf download-mta-op-logs -i <OPERATION_ID>
> ```

By default, the logs are saved to the `./mta-op-<OPERATION_ID>/` directory. To save them to a different location, use the `-d` option.

> ### Sample Code:  
> ```
> cf download-mta-op-logs -i <OPERATION_ID> -d ./my-logs
> ```

Alternatively, download the logs of the last operations of a multitarget application without specifying an operation ID.

> ### Sample Code:  
> ```
> cf download-mta-op-logs --mta my-mta --last 1
> ```

For the full list of options, see [download-mta-op-logs](../50-administration-and-ops/multitarget-application-commands-for-the-cloud-foundry-environment-65ddb1b.md#loio65ddb1b51a0642148c6b468a759a8a2e__section_fhv_fkk_vt).

**Related Information**  


[Multitarget Application Commands for the Cloud Foundry Environment](../50-administration-and-ops/multitarget-application-commands-for-the-cloud-foundry-environment-65ddb1b.md "A list of additional commands to deploy multitarget applications (MTA) to the Cloud Foundry environment.")

[\(Experimental\) Exporting MTA Operation Logs to SAP Cloud Logging](experimental-exporting-mta-operation-logs-to-sap-cloud-logging-e443336.md "Export the operation logs of your multitarget application to SAP Cloud Logging, so that you can store, visualize, and analyze them together with the rest of your observability data.")

