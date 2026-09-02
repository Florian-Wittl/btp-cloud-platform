<!-- loio5a086335e91846bb9a6c0026aecc963d -->

# Generate Heap Dumps and Store Them Remotely

If an application is bound to an Object Store service instance, heap dumps can be uploaded automatically to a cloud storage when an OutOfMemoryError occurs.



## Prerequisites

1.  Create an Object Store service instance. For example, the service name is **`myobjectstore`**, and its plan is **`s3-standard`**:

    ```
    cf create-service objectstore s3-standard myobjectstore
    ```

2.  In the `manifest.yml` file, bind this service instance to your Java application \(**`myapp`**\) and enable remote heap dump load:

    ```
    
    ---
    applications:
    - name: myapp
      ...
      buildpacks:
      - sap_java_buildpack_jakarta
      env:
        TARGET_TUNTIME: tomcat
        SJB_HEAPDUMP_TO_OBJECTSTORE: "true"
      services:
      - myobjectstore
    ```

3.  Deploy your application:

    ```
    cf push myapp
    ```


Your application is bound to an Object Store service instance. Object Store provides the secure cloud storage location \(bucket or container\) where heap dumps will be uploaded. For more information, see: [What Is Object Store?](https://help.sap.com/docs/object-store/object-store-service-on-sap-btp/what-is-object-store?version=Cloud)



## Context

Use this way when you need to capture and analyze heap dumps from Java applications experiencing *OutOfMemoryError* in environments where local disk space is limited or heap dumps need to be preserved after container restarts.

When this feature is enabled, heap dumps generated during *OutOfMemoryError* are uploaded to a protected and isolated cloud provider environment \(AWS S3 or Azure\). The respective storage location is automatically provided by the Object Store service, ensuring your heap dumps are stored securely within your organization's cloud infrastructure with the appropriate access control and encryption.

**Configuration Properties:**


<table>
<tr>
<th valign="top">

Category

</th>
<th valign="top">

Property

</th>
<th valign="top">

Description

</th>
<th valign="top">

Values

</th>
</tr>
<tr>
<td valign="top">

Required

</td>
<td valign="top">

`SJB_HEAPDUMP_TO_OBJECTSTORE`

</td>
<td valign="top">

Enables automatic heap dump uploads to the Object Store service in the event of *OutOfMemoryError*. The uploaded heap dump file is compressed \(GZIP\).

</td>
<td valign="top">

-   **true**
-   **false** \(default\)



</td>
</tr>
<tr>
<td valign="top" rowspan="3">

Optional

</td>
<td valign="top">

`HEAPDUMP_UPLOADER_PART_MB`

</td>
<td valign="top">

Enables multipart upload chunks. Their sizes are in MiB.

-   For faster networks, increase the default value.
-   For slower networks, decrease the default value.



</td>
<td valign="top">

Default: **64**

</td>
</tr>
<tr>
<td valign="top">

`HEAPDUMP_UPLOADER_DEBUG`

</td>
<td valign="top">

Enables verbose logging to troubleshoot upload issues.

</td>
<td valign="top">

-   **true**
-   **false** \(default\)



</td>
</tr>
<tr>
<td valign="top">

`HEAPDUMP_UPLOADER_KILL_JAVA_PROCESS`

</td>
<td valign="top">

Kills the Java process after the heap dump upload completes to trigger container restart.

</td>
<td valign="top">

-   **true** \(default\)
-   **false**



</td>
</tr>
</table>

Once the heap dump is fully uploaded to the storage bucket/container created by the Object Store service, you can acquire it for analysis by using your cloud provider's command line or console. To do that, follow the steps:



## Procedure

1.  Get the service binding details from your bound Object Store service instance \(bucket/container name, access credentials, endpoint URL\). To do that, run:

    ```
    cf env myapp
    ```

2.  Look for the VCAP\_SERVICES section containing your ***objectstore*** credentials. Example output:

    ```
    
     System-Provided:
      VCAP_SERVICES: {
        "objectstore": [
          {
            "credentials": {
              "access_key_id": "<access-key>",                     
              "secret_access_key": "<secret-key>",                   
              "bucket": "hcp-12345678-8888-abcd-aaa-12345678678",  
              "region": "eu-01"                             
            },
            "instance_name": "myobjectstore",
            "label": "objectstore",
            "plan": "s3-standard"
          }
        ]
      }
    ```

    **NOTE:** Pay attention to the values of the following credentials \(you'll need them to download the heap dump\):

    -   `access_key_id`
    -   `bucket`
    -   `region`

3.  Configure the cloud provider credentials. To do that, set up your cloud provider CLI with the credentials from **Step 2**.

    **For example:** If you use AWS S3, run the following commands:

    -   ```
export AWS_ACCESS_KEY_ID=<access-key>
```

    -   ```
export AWS_SECRET_ACCESS_KEY=<secret-key>
```

    -   ```
export AWS_DEFAULT_REGION=<region>
```




4.  List the available heap dumps. For AWS S3, run:

    ```
    aws s3 ls s3://<bucket>/heapdumps/
    ```

5.  Download the heap dump. The heap dump filename follows the pattern: **`sjb-<org>-<space>-<app>-<instance>-<timestamp>.hprof`**

    For AWS S3, run:

    ```
    aws s3 cp s3://<bucket>/heapdumps/<heapdump-filename>.hprof ./<heapdump-filename>.hprof.gz
    ```

6.  Decompress the heap dump. The downloaded file is compressed as `.gzip` and must be unzipped before analysis. To do that, run:

    ```
    gunzip <heapdump-filename>.hprof.gz
    ```




## Results

The **`.hprof`** heap dump file is generated and ready to be analyzed.

