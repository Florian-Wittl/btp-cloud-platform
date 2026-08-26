<!-- loio6237892d5ac64a9bb03a3b5328f72696 -->

# Troubleshooting the MCP Server for SAP BTP Administration

Find solutions to common errors that occur when connecting to or using the MCP Server for SAP BTP Administration.


<table>
<tr>
<th valign="top">

Error

</th>
<th valign="top">

Cause

</th>
<th valign="top">

Solution

</th>
</tr>
<tr>
<td valign="top">

Authentication fails with an error indicating the session has expired or the token is no longer valid.

</td>
<td valign="top">

The authentication token issued during your last login has expired. Tokens are short-lived by design.

</td>
<td valign="top">

Re-authenticate using your AI client:

-   **Claude Code:** Type `/mcp` at the prompt, select the server, and complete the browser login again.
-   **GitHub Copilot in VS Code:** VS Code detects the expired session and prompts you to log in again automatically.
-   **OpenCode:** Restart OpenCode and complete the browser login when prompted.



</td>
</tr>
<tr>
<td valign="top">

Authentication fails with an error indicating the identity provider is not recognized or the tenant cannot be found.

</td>
<td valign="top">

The custom identity provider value in your connection configuration is incorrect or missing. This applies to Direct Connection only.

</td>
<td valign="top">

Check the IAS tenant subdomain value in your configuration:

-   **Claude Code:** Verify that the `BTP_CLI_ORIGIN` environment variable is set to the correct IAS tenant subdomain and is exported in the shell where you run `claude`.
-   **GitHub Copilot in VS Code:** Check the `X-Platform-Custom-IDP` header value in your `mcp.json`.
-   **OpenCode:** Check the `X-Platform-Custom-IDP` header value in your `opencode.json`.

You can find the correct IAS tenant subdomain in the SAP BTP cockpit under your global account trust configuration. If your platform user comes from the Default Identity Provider \(`accounts.sap.com`\), omit this value entirely.

</td>
</tr>
<tr>
<td valign="top">

A tool call returns a `403` error or a message indicating the operation is not authorized.

</td>
<td valign="top">

Your BTP user does not have the role collection required for the requested operation. The MCP server enforces the same role-based authorization as the SAP BTP cockpit.

</td>
<td valign="top">

Check the error message returned by the assistant for the name of the required role collection. Then ask your global account or subaccount administrator to assign that role collection to your user in the SAP BTP cockpit.

Once the role collection is assigned, retry your request. No reconnection is needed.

</td>
</tr>
<tr>
<td valign="top">

The MCP server does not appear in the AI client after registration, or the client reports that it cannot reach the server.

</td>
<td valign="top">

The server URL or client ID entered during registration is incorrect, or the required environment variables are not available in the current shell session.

</td>
<td valign="top">

Verify your registration configuration:

-   **Claude Code:** Run `claude mcp list` to check the registered URL. If it is incorrect, remove the entry with `claude mcp remove <server-name>` and register again using the correct URL and client ID from [Connect to the MCP Server for SAP BTP Administration](connect-to-the-mcp-server-for-sap-btp-administration-26262f3.md). For Direct Connection, confirm that the `BTP_USERNAME`, `BTP_PASSWORD`, and `BTP_CLI_ORIGIN` environment variables are exported in your current shell.
-   **GitHub Copilot in VS Code:** Check the URL in `.vscode/mcp.json` against the values in [Connect to the MCP Server for SAP BTP Administration](connect-to-the-mcp-server-for-sap-btp-administration-26262f3.md).
-   **OpenCode:** Check the URL and, for SSO, the `clientId` value in `opencode.json`.



</td>
</tr>
</table>



<a name="loio6237892d5ac64a9bb03a3b5328f72696__support"/>

## Reporting an Issue

If the steps above do not resolve your issue, create a support request using SAP component `BC-CP-ADMINMCP`. See [Getting Support](https://help.sap.com/docs/BTP/65de2977205c403bbc107264b8eccf4b/5dd739823b824b539eee47b7860a00be.html?locale=en-US&state=PRODUCTION&version=Cloud) to know more.

