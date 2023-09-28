# Omnichannel Inventory Postman Setup for Partners

_Created by Tom Zarr with key contributions from Sandra Golden and Jordane Bachelet_

This material is supplemental to the Omnichannel Inventory Partner Learning Camp course. See the course for the complete setup including configuring OpenSSL and a JWT (JSON Web Token) on Windows.

This postman collection contains API endpoints from various Salesforce Commerce domains, but the emphasis is on completing B2B Commerce checkouts and performing operational tasks related to that end through the Connect API and other flavors of API available on the Salesforce platform.

Unlike many other Postman collections, this one is meant to be user friendly and have meaningful error messages when something is not set up correctly or there are issues in the request chains.

## ⚠️ Disclaimers

- This collection is provided as-is. It's not officially supported by Salesforce or covered by SLAs.
- API documentation is not provided with the collection. Please refer to the official documentation.
- The documentation for the majority of the endpoints in this collection can be found in the [B2B and D2C Commerce Resources](https://developer.salesforce.com/docs/atlas.en-us.chatterapi.meta/chatterapi/connect_resources_commerce.htm) of the Connect REST API Developer Guide.

## What this collection is and isn't

This collection is intended to be used for a B2B standalone setup. That isn't to say you can't use it with a Salesforce Org containing other commerce products, just that B2B is what's targeted.

## Approach

1. I've tried to stay "close to the metal" by using the Postman Scripting API directly. There are a few cases where this just isn't possible or realistic because responses are not true JSON or HTTP status codes are reported in the HTML body text, but those should be true exceptions and definitely not the rule.
2. I wanted oAuth 2.0 to not be a headache. This is my approach and it's about the best I could come up with given the limitations of the tool. Have a request where you set it once and then everything following just uses Bearer Token. It works and better is the enemy of good enough.
3. The request chains are long; This is by design. At the risk of being didactic, this is ultimately a *teaching tool*. When it comes to working with APIs I find more detail is better.
4. Collection variables are calculated and presented before each request.
5. Tests are applied following each response. If something isn't right I want you to know about it early so I assume little to nothing that a response is successful.

## This collection provides the following:

1. Upload Inventory (Happy Path)
2. Get Running Import Jobs
3. Get and Delete Import Jobs
4. Get Inventory Availability (Log In Flow + Connect API)
5. Get Inventory Availability (oAuth Flow + Connect API)

## Example inventory upload files are provided for:

 1.  Single Capricorn Coffee item
 2.  Multiple Capricorn Coffee items
 3.  Single item in which you supply the SKU

## Connected App

Because we're using APIs you'll need to set up two connected apps in your org if you want to be able to use both the Headless and Connect APIs:

- Postman_OCI
- Postman_OCI_ConnectApi

You will need to obtain some values from your Connected App in order to establish connectivity (see: [Variables](./#Variables))

## Authentication Approach

Authentication is generally handled one of three ways:

1. Logging in as an administrator (often used in the request chain's outset for lookup operations to make things reusable across orgs)
2. Logging in as a 'known good' Buyer (aka Contact under Account with a User - all three must be set up and this is commonly _not_ going to be the case with a System Administrator account)
3. Establishing oAuth 2.0 *once per folder* and then having subsequent requests set to Bearer Token in the "Authorization" tab (see [ oAuth 2.0 is set once in each folder](./#oauth-20-is-set-once-per-folder-where-needed)

### Logging in as an Administrator or Buyer

This is handled inline. Just supply the environment with the needed variables like these and the collection and scripting should take care of the rest for you:

| Name | Description
| --- | --- |
| `orgLoginUrl` | Either `https://login.salesforce.com` (production / trial) or `https://test.salesforce.com` (sandbox)
| `orgHost` | Protocol and host portion of the Salesforce org's URL Example: `https://yourusername-august.lightning.force.com` |
| `orgAdminUsername` | The System Administrator username for the Salesforce org |
| `orgAdminPassword` | The System Administrator password for the Salesforce org |
| `orgAdminSecurityToken` | The security token for the Salesforce Org System Administrator User |

### oAuth 2.0 is set once per folder where needed

Please don't take a "do-it-yourself" approach here. Why?

1. Most importantly, you don't need to.
2. There's some scripting which checks if your token set up is correct to begin making requests.
3. Tokens are passed in subsequent requests using __Bearer Token__ authentication on the requests needing it.
4. This was done "by design" so you can easily add your own requests or copy them and move them around with little to no impact whenever oAuth is needed.
5. You can also find and copy the requests named something like "Set your oAuth 2.0 Token in Authorization tab" whenever you need to establish oAuth 2.0 before another request or add it to a folder (regular copy paste operations of these objects is supported in Postman).

#### Establishing oAuth 2.0 (First Time and Details)

- Look for the request with a name like "Set your oAuth 2.0 Token in Authorization tab"
- Don't try to do a bunch of manual work on your token setup or get fancy as it's all filled in for you with variables.

Just follow these steps which will also be provided during an oAuth error state in Postman's __Console__ as errors:

1. Click on the Request with a name like "Set your oAuth 2.0 Token here in Authorization tab"
2. Click the "Authorization" tab
3. Click the "Get New Access Token" button
4. Click the "Proceed" button
5. Click the "Use Token" button
6. Optional - Use the delete button's dropdown option to remove expired tokens (it's best to remove all of them except the newest)
7. Retry your request(s)

## Variables

⚠️ __Note__: You must set up your environment variables correctly for this all to work. Collection variables will be calculated between requests and used in subsequent  requests. The naming convention used in the collection is to prefix collection variables with an underscore.

These are bad examples. You should never (or almost never) see a call like these in the collection and it's strongly recommended that you not create them this way unless you like needless debugging:

1. `pm.collectionVariables.set('myVariable', 'My new values');`
2. `pm.collectionVariables.get('myVariable');`
3. `pm.environment.set('_myVariable', 'My new values');`
4. `pm.environment.get('_myVariable');`

These are good examples as they adhere to the established naming convention and it's clear which dictionary we're referring to:

1. `pm.collectionVariables.set('_myVariable', 'My new values');`
2. `pm.collectionVariables.get('_myVariable');`
3. `pm.environment.set('myVariable', 'My new values');`
4. `pm.environment.get('myVariable');`

Philosophically speaking, I don't like mixing which dictionary I get a value from. A value with an underscore prefix in this naming convention should correspond to pm.collectionVariables and one without should come from (or be written to) pm.environment. I don't use a context stand-on object or variable that would allow pulling the value from either pm.collectionVariables or pm.environment. I believe quite strongly in the single definition principal and not coding by coincidence - even with tests. If those terms are not familiar to you I'd recommend the book "The Pragmatic Programmer" as it could replace many on your shelf (or device).

### Input values

#### Some Environment variables are used for lookups to support reuse

These are some examples:

1. webstoreName (resolves to a WebStore Id)
2. buyerAccountName (resolves to an Account Id)

#### Some Environment variables can be used to provide comma delimited values

 1. productNamesCommaDelimited (used in the B2B Postman collection and resolves to a list of Product2 Ids) 
 2. productStockKeepingUnitsCommaDelimited
 3. locationGroupIdentifiersCommaDelimited
 4. locationIdentifiersCommaDelimited

#### Some Environment variables can be used to provide a single string value

 1. productSearchTerm (used in the B2B Postman collection)

## Standardized variables (also documented in the Postman collection)

⚠️ *Note*: The naming convention found here is used across other Salesforce Commerce product Postman collections in the Partner Readiness space when possible to support reuse.

This Postman collection relies on the following variables:

| Name | Description | Location |
| --- | --- | --- |
| `orgLoginUrl` | Either `https://login.salesforce.com` (production/trial) or `https://test.salesforce.com` (sandbox) | User supplied |
| `orgHost` | Protocol and host portion of the Salesforce org's URL | User supplied. Example: `https://yourusername-august.lightning.force.com` |
| `orgAdminUsername` | The System Administrator username for the Salesforce org | User supplied |
| `orgAdminPassword` | The System Administrator password for the Salesforce org | User supplied |
| `orgAdminSecurityToken` | The security token for the Salesforce Org System Administrator User | Autogenerated |
| `orgHostMySalesforceFormat` | The protocol and host portion of the Salesforce org's URL in 'my.salesforce.com' format. Useful for avoiding redirection problems and 'Invalid Session Id' errors post authentication | User supplied. Example: `https://yourusername-august.my.salesforce.com` |
| `orgId` | The Salesforce.com Organization ID for the Salesforce org | Setup > Company Information |
| `apiVersion` | The Salesforce API version (e.g. 58.0). | User supplied. Most recent value recommended. |
| `connectedAppConsumerKey` | The Consumer Secret value for the Connected App in the Salesforce org. | Setup > App Manager > Connected App Record > View > Manage Consumer Details |
| `connectedAppConsumerSecret` | The Consumer Secret value in the Connected App. | Setup > App Manager > Connected App Record > View > Manage Consumer Details |
| `host` | The OCI Host for the Salesforce Org | Setup > Omnichannel Inventory > 'Base URL for Inventory API Calls' |
| `tenantId` | The OCI Tenant Id for the Salesforce org | Setup > Omnichannel Inventory > 'Tenant Group ID' |
| `JWT` | JSON Web Token | Consult the course for setup procedure |
| `bearerToken` | The bearer token sent in the Authorization header | Autogenerated |
| `productStockKeepingUnitsCommaDelimited` | The Stock Keeping Unit(s) for the products needing inventory levels | User supplied. Must be comma delimited. Example 1: `PROSE,B-C-COFMAC-001,ESP-IOT-1,ID-PEM,PS-EL,PS-INF,TR-COFMAC-001` Example 2: `ID-PEM`  |
| `locationGroupIdentifiersCommaDelimited` | The Location Group(s) for the availability query | User supplied. Must be comma delimited. Example 1: `LocationGroup01` Example 2: `LocationGroup01,LocationGroup02,WestCoastDistributionChain` |
| `locationIdentifiersCommaDelimited` |  The Location(s) for the availability query | User supplied. Must be comma delimited. Example 1: `Warehouse01` Example 2: `Warehouse01,Store056,Pier37` |
| `importsUploadLinkPostBody` | The pseudo JSON file content posted for inventory level uploads | Provided for example products and can be modified to suit your needs |
| `webstoreName` | Name of the webstore used to look up a corresponding Id | The value specified when the store / site was created such as 'B2B LWR Enhanced Store from TSO.' Can be found in the Commerce App under 'Stores.' |
| `buyerUsername` | Registered B2B Buyer User's username. | User supplied. |
| `buyerPassword` | Registered B2B Buyer User's password. | User supplied. |
| `buyerAccountName` | Name of the Account used to look up the Accoujnt Id which is tied to the Buyer User. | User supplied. Example: `United Coffee Bean Corp` |

Consult the course documentation for additional details.
