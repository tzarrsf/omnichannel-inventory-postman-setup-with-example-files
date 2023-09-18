# Omnichannel Inventory Postman Setup with example files

This material is supplemental to the Omnichannel Inventory Partner Learning Camp course. See the course for the complete setup including configuring OpenSSL and a JWT (JSON Web Token) on Windows.

## This collection provides the following:

1. Upload Inventory (Happy Path)
2. Get Running Import Jobs
3. Get and Delete Import Jobs
4. Get Inventory Availability (Log In Flow + Connect API)
5. Get Inventory Availability (Connect API) oAuth 2.0

## Example inventory upload files are provided for:

 1.  Single Capricorn Coffee item
 2.  Multiple Capricorn Coffee items
 3.  Single item in which you supply the SKU

## You will need to set up two connected apps in your org if you want to be able to use both the Headless and Connect APIs:
- Postman_OCI
- Postman_OCI_ConnectApi

## These variables in the environment can be altered for your Connect API calls as long as they are comma delimited:

1. productStockKeepingUnitsCommaDelimited - e.g: PROSE,B-C-COFMAC-001,ESP-IOT-1,ID-PEM,PS-EL,PS-INF,TR-COFMAC-001
2. locationGroupIdentifiersCommaDelimited - e.g: LocationGroup01,LocationGroup02
3. locationIdentifiersCommaDelimited - e.g: Warehouse01,CaliforniaWarehouse,VirginiaWarehouse,Toronto Facility

## Standardized variables (also documented in the Postman collection)

⚠️ **_Note_**: The naming convention found here is used across other Salesforce Commerce product Postman collections in the Partner Readiness space when possible to support reuse.

This Postman collection relies on the following variables:

| Name | Description | Location |
| --- | --- | --- |
| `host` | The OCI Host for the Salesforce Org | Setup > Omnichannel Inventory > 'Base URL for Inventory API Calls' |
| `tenantId` | The OCI Tenant Id for the Salesforce org | Setup > Omnichannel Inventory > 'Tenant Group ID' |
| `JWT` | JSON Web Token | Consult the course for setup procedure |
| `bearerToken` | The bearer token sent in the Authorization header | Autogenerated |
| `productStockKeepingUnitsCommaDelimited' | | Example 1: `PROSE,B-C-COFMAC-001,ESP-IOT-1,ID-PEM,PS-EL,PS-INF,TR-COFMAC-001` Example 2: `ID-PEM`  |
| `locationGroupIdentifiersCommaDelimited' | | Example 1: `LocationGroup01` Example 2: `LocationGroup01,LocationGroup02,WestCoastDistributionChain` |
| `locationIdentifiersCommaDelimited' | | | Example 1: `Warehouse01` Example 2: `Warehouse01,Store056,Pier37` |
| `importsUploadLinkPostBody` | The pseudo JSON file content posted for inventory level uploads | Provided for example products and can be modified to suit your needs |
| `orgHost` | Protocol and host portion of the Salesforce org's URL | User supplied. Example: `https://yourusername-august.lightning.force.com` |
| `orgAdminUsername` | The System Administrator username for the Salesforce org | User supplied |
| `orgAdminPassword` | The System Administrator password for the Salesforce org | User supplied |
| `orgAdminSecurityToken` | The security token for the Salesforce Org System Administrator User | Autogenerated |
| `orgHostMySalesforceFormat` | The protocol and host portion of the Salesforce org's URL in 'my.salesforce.com' format. Useful for avoiding redirection problems and 'Invalid Session Id' errors post authentication | User supplied. Example: `https://yourusername-august.my.salesforce.com` |
| `apiVersion` | The Salesforce API version (e.g. 58.0). | User supplied. Most recent value recommended. |
| `connectedAppConsumerKey` | The Consumer Secret value for the Connected App in the Salesforce org. | Setup > App Manager > Connected App Record > View > Manage Consumer Details |
| `connectedAppConsumerSecret` | The Consumer Secret value in the Connected App. | Setup > App Manager > Connected App Record > View > Manage Consumer Details |
| `orgLoginUrl` | Either `https://login.salesforce.com` (production/trial) or `https://test.salesforce.com` (sandbox) | User supplied |
| `orgId` | The Salesforce.com Organization ID for the Salesforce org | Setup > Company Information |
| `webstoreName` | Name of the webstore used to look up a corresponding Id | The value specified when the store / site was created such as 'B2B LWR Enhanced Store from TSO.' Can be found in the Commerce App under 'Stores.' |
| `buyerUsername` | Registered B2B Buyer User's username. | User supplied. |
| `buyerPassword` | Registered B2B Buyer User's password. | User supplied. |
| `buyerAccountName` | Name of the Account used to look up the Accoujnt Id which is tied to the Buyer User. | User supplied. Example: `United Coffee Bean Corp` |

Consult the course documentation for additional details.


