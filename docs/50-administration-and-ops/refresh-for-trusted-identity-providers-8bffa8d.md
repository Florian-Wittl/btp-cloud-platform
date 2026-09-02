<!-- loio8bffa8d8f6404af5ae6875d528722735 -->

# Refresh for Trusted Identity Providers

Trust refresh updates the SAP BTP trust configuration to reflect changes in the SAP Cloud Identity Services tenant. Use it to repair broken trust configurations while ensuring custom assertion attribute mappings are saved beforehand.



## Prerequisites

You established trust with an identity provider.

For more information, see:

-   [Establish Trust and Federation Between SAP Authorization and Trust Management Service and SAP Cloud Identity Services](establish-trust-and-federation-between-sap-authorization-and-trust-management-service-a-161f8f0.md)

-   [Establish Trust and Federation of Custom Identity Providers for Platform Users](establish-trust-and-federation-of-custom-identity-providers-for-platform-users-c368984.md)




## Context

When you change the OIDC issuer in your SAP Cloud Identity Services tenant, the trust configuration in SAP BTP breaks. To restore trust, you refresh the trust configuration. Refreshing the trust updates SAP BTP to reflect the latest settings from your SAP Cloud Identity Services tenant, including the issuer value.

> **Note:** For platform users, the global account automatically repairs the trust after 24 hours. To restore the trust immediately, you manually refresh the trust configuration.



## Procedure

1.  Back up attribute mappings of any applications.

    Refreshing the trust configuration resets the assertion attributes of associated applications in SAP Cloud Identity Services to their default values. You lose any customizations you made to the assertion attribute mappings. To preserve your customizations, save a copy of your current assertion attributes before you refresh.

    1.  Retrieve the current assertion attributes.

        You can retrieve the current assertion attributes by accessing the following application URL of SAP Cloud Identity Services:

        <code>https://<i class="varname">&lt;tenant&gt;</i>.accounts.ondemand.com/Applications/v1/<i class="varname">&lt;application-ID&gt;</i></code>

        You can retrieve the application ID from the SAP Cloud Identity Services admin console.

        For more information, see [Configuring User Attributes from the Identity Directory](https://help.sap.com/docs/cloud-identity-services/cloud-identity-services/configure-user-attributes-sent-to-application).

        You receive a JSON file of the application configuration.

    2.  In the results, locate `assertionAttributes`.

        The list shows the current attribute mappings. The following example illustrates this:

        ```
        "assertionAttributes": [
          { "assertionAttributeName": "first_name", "userAttributeName": "firstName", "inherited": false },
          { "assertionAttributeName": "last_name", "userAttributeName": "lastName", "inherited": false },
          { "assertionAttributeName": "mail", "userAttributeName": "mail", "inherited": false },
          { "assertionAttributeName": "user_uuid", "userAttributeName": "userUuid", "inherited": false },
          { "assertionAttributeName": "locale", "userAttributeName": "language", "inherited": false }
        ]
        
        ```

    3.  Save a copy of the `assertionAttributes` list.

        After the refresh, you use this copy to restore your attribute mappings if needed.


2.  In the SAP BTP cockpit, go to the trust configuration you want to refresh.

3.  Choose the trust configuration to open its details.

4.  Choose *Refresh*.

    The trust configuration updates to reflect the latest settings from your SAP Cloud Identity Services tenant, including the issuer value.

5.  In the SAP Cloud Identity Services admin console, go to the application associated with your trust configuration.

6.  Compare the current assertion attributes with the copy you saved before the refresh.

7.  Restore any missing or changed attribute mappings using your saved copy.


