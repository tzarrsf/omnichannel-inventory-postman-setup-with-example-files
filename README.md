# Omnichannel Inventory Postman Setup for Partners
_Created by Tom Zarr with key contributions and examples from Sandra Golden and Jordane Bachelet_

## Background and Use Case

This material is supplemental to the Omnichannel Inventory Partner Learning Camp course. See the course for the complete setup including configuring OpenSSL and a JWT (JSON Web Token) on Windows.

This postman collection contains API endpoints from various Salesforce Commerce domains, but the emphasis is uploading inventory levels, monitoring or deleting jobs in that regard, and getting availability levels through the Connect API.

Unlike many other Postman collections, this one is meant to be *user friendly* and have *meaningful error messages* when something is not set up correctly or there are issues in the request chains.

## ⚠️ Disclaimers

- This collection is provided as-is. It's not officially supported by Salesforce or covered by SLAs.
- API documentation is not provided with the collection. Please refer to the official documentation.
- The documentation for the majority of the endpoints in this collection can be found in these guides, but keep in mind this collection was put together through the lens of a Salesforce org-based developer, not B2C Commerce:
    - [Omnichannel Inventory Headless API Endpoints](https://help.salesforce.com/s/articleView?id=sf.inv_omnichannel_inventory_headless_apis.htm&type=5)
    - [Omnichannel Inventory Connect REST APIs](https://help.salesforce.com/s/articleView?id=sf.inv_omnichannel_inventory_connect_rest_apis.htm&type=5)
    - [getInventoryImport](https://developer.salesforce.com/docs/commerce/commerce-api/references/impex?meta=getInventoryImport)
    - [deleteInventoryImport](https://developer.salesforce.com/docs/commerce/commerce-api/references/impex?meta=deleteInventoryImport)
    - [Salesforce Omnichannel Inventory Implementation Guide](https://resources.docs.salesforce.com/latest/latest/en-us/sfdc/pdf/salesforce_omnichannel_inventory_implementation_guide.pdf)

## What this collection is and isn't

This collection is intended to be used for a Salesforce Order Management standalone setup with Omnichannel Inventory. That isn't to say you can't use it with a Salesforce Org containing other commerce products, just that SOM with OCI is what's targeted.

## Approach

1. I've tried to stay "close to the metal" by using the Postman Scripting API directly. There are a few cases where this just isn't possible or realistic because responses are not true JSON or HTTP status codes are reported in the HTML body text, but those should be true exceptions and definitely not the rule.
2. I wanted oAuth 2.0 to be easy so I could move things around. This is my approach and it's about the best I could come up with given the limitations of the tool: Have a dummy request where you set it once in that folder then everything following just uses Bearer Token. It works and better is the enemy of good enough.
3. The request chains are long; This is by design. At the risk of being didactic, this is ultimately a *teaching tool*. When it comes to working with APIs I find more detail is better.
4. Collection variables are calculated and presented before each request.
5. Tests are applied following each response. If something isn't right I want you to know about it early so I assume little to nothing about a response being successful.

## This collection provides the following

1. Upload Inventory (Happy Path) - using the Headless APIs
2. Get Running Import Jobs - using the Headless APIs
3. Get and Delete Import Jobs - using the Headless APIs
4. Get Inventory Availability (Log In Flow + Connect API) - using Buyer Login and Connect APIs
5. Get Inventory Availability (oAuth Flow + Connect API) - using oAuth Login and Connect APIs

There are plans to expand on these basic examples once the B2B, D2C and Order Management collections are rolling.

## Example inventory upload files

*Note*: When uploading inventory level files, the format should contain 1 item per line. Consult the official documentation for details.

 1.  Single Capricorn Coffee item
 2.  Multiple Capricorn Coffee items
 3.  Single item in which you supply the SKU

## Connected App Requirements

Because we're using APIs you'll need to set up two Connected Apps in your org if you want to be able to use both the Headless and Connect APIs:

- Postman_OCI
- Postman_OCI_ConnectApi

Using the Headless APIs (such as for loading inventory levels) requires setting up a JSON Web Token. Check the course activity around Headless APIs for details.

You will need to obtain some values from your Connected App in order to establish connectivity (see: [Variables](./#Variables))

## Authentication Approach

Authentication is generally handled in three ways:

1. Logging in as an Administrator (often used at request chain outset for lookup operations to preserve reusability across orgs). See [Logging in as an Administrator or Buyer](./logging-in-as-an-administrator-or-buyer).
2. Logging in as a 'known good' Buyer (aka Contact under Account with a User). Please note that all three must be set up and this is commonly _not_ going to be the case with a System Administrator account. See [Logging in as an Administrator or Buyer](./logging-in-as-an-administrator-or-buyer). 
3. Establishing oAuth 2.0 *once per folder* and then having subsequent requests set to Bearer Token in the **Authorization** tab. See [oAuth 2.0 is set once per folder where needed](./#oauth-20-is-set-once-per-folder-where-needed).

### Logging in as an Administrator or Buyer

This is handled inline. Just supply the environment with the needed variables like these and the collection and scripting should take care of the rest:

| Name | Description
| --- | --- |
| `orgLoginUrl` | Either `https://login.salesforce.com` (production / trial) or `https://test.salesforce.com` (sandbox)
| `orgHost` | Protocol and host portion of the Salesforce org's URL Example: `https://yourusername-august.lightning.force.com` |
| `orgAdminUsername` | The System Administrator username for the Salesforce org |
| `orgAdminPassword` | The System Administrator password for the Salesforce org |
| `orgAdminSecurityToken` | The security token for the Salesforce Org System Administrator User |

If you need to move this type of Administrator or Buyer authentication scheme around, just copy the request and paste it into another folder or location in the current folder. Copy and paste operations are supported in Postman.

### oAuth 2.0 is set once per folder where needed

Please don't take on a "do-it-yourself" approach with the oAuth 2.0 setup. Why?

1. Most importantly, you don't need to. This has all been completed using variables. There's no guesswork on which log in needs the token appended to the password, etc.
2. There's scripting which checks if your token set up is correct to begin making requests.
3. Tokens are passed in subsequent requests using __Bearer Token__ authentication on the requests needing it. Just turn it on - done.
4. This was done "by design" so you can easily add your own requests or copy them and move them around with little to no impact whenever oAuth 2.0 is needed.
5. You can also find and copy the requests named something like "Set your oAuth 2.0 Token in Authorization tab" whenever you need to establish oAuth 2.0 before another request or add it to a folder.

#### Establishing oAuth 2.0 (First Time and Details)

- Look for the request with a name like "Set your oAuth 2.0 Token in Authorization tab"
- Please don't try to do a bunch of manual work on your token setup or get fancy. Agains, it's all filled in for you with variables.

Follow these steps which will also be provided during an oAuth error state in Postman's *Console* as errors:

1. Click on the Request with a name like "Set your oAuth 2.0 Token here in Authorization tab"
2. Click the "Authorization" tab
3. Click the "Get New Access Token" button
4. Click the "Proceed" button
5. Click the "Use Token" button
6. Optional - Use the delete button's dropdown option to remove expired tokens (it's best to remove all of them except the newest)
7. Retry your request(s)

## Error Handling

It's my intent to trap every error state predictably possible and save anyone using this collection some time. I welcome your feedback on that front. That said, I can't possibly cover every single org configuration or set of data and this is where you come in as a partner. Below are some of the common cases I have tried to account for to tell you when something's wrong or provide hints to help troubleshoot what you're seeing.

### Clear Collection Variables

It's recommended you stick to this pattern as it does a few things to ensure your request chains are kept consistent:

1. The **Pre-request** tab makes sure that your environment is selected and stops the chain (dead programs tell no lies):

```
// Check for environment selection
if(pm.environment.name === undefined) {
    const msg = 'No Postman environment selected or set.';
    pm.expect.fail(msg);
}
```

2. The **Pre-request** tab makes sure that it clears out the collection variables:

```
// Clean up the variables from the collection set throughout the various calls
pm.collectionVariables.clear();
```

3. The **Test** tab ensures the collection is indeed empty:

```
pm.test('Make sure collection variables are clean', () => {
    pm.expect(pm.collectionVariables.values.map((v) =>  v.key + ': ' + v.value)).to.be.an('array').empty;
});
```

### Request names are pulled in dynamically to both the Pre-request and Test code

Whatever the request is named in the user interface is reflected dynamically by these code snippets:

`console.log(`${pm.info.requestName} Pre-request Script...`);`

`console.log(`${pm.info.requestName} Tests...`);`

If you called the request "Heinz 57" you will see `Heinz 57 Pre-request...` or `Heinz 57 Tests...` in the console. You can drill into your request and response bodies as needed knowing what was passed to the endpoint.

### Requests must meet Preconditions

If **environment** variables are expected for a request they are tested on the Pre-request script tab:

```
// Expected strings in environment variables
['host', 'tenantId', 'bearerToken'].forEach(esiev => {
    if(!pm.environment.has(esiev)) {
        const msg = `Expected Postman environment variable not found: '${esiev}' in environment: '${pm.environment.name}'.`;
        pm.expect.fail(msg);
    }
    pm.expect(pm.environment.get(esiev)).to.exist;
    pm.expect(pm.environment.get(esiev)).to.be.an('string');
});
```

If **collection** variables are expected they are tested on the Pre-request script tab

```
// Expected strings in collection variables
['_webStoreId', '_token', '_orgId'].forEach(esicv => {
    if(pm.collectionVariables.get(esicv) === undefined) {
        const msg = 'Expected Postman collection variable not found: ' + esicv;
        pm.expect.fail(msg);
    }
    pm.expect(pm.collectionVariables.get(esicv)).to.exist;
    pm.expect(pm.collectionVariables.get(esicv)).to.be.an('string');
});
```

### Collection variables are listed in each Pre-request

This snippet allows you to see things _before_ each request is made:

```
console.log('Collection variables before:\r\n'.concat(pm.collectionVariables.values.map((v) =>  v.key + ': ' + v.value).sort().join('\r\n')));
```

Example of collection variables being printed to the console in a Pre-request script:

```
Collection variables before:↵
_instanceUrl: https://toms-org.my.salesforce.com↵
_locationGroupIdentifiers: ["LocationGroup01"]↵
_orgId: 00DHn0000YYYYYYYYY↵
_productStockKeepingUnits: ["PROSE","B-C-COFMAC-001","ESP-IOT-1","ID-PEM","PS-EL","PS-INF","TR-COFMAC-001"]↵
_token: 0xdeadbeef!0x8badfood!0xfeedfacecafebeefx.01123581321345589144233377610↵
_userId: 005HnXXXXXXXXXXXXX
```

## Variables

> ⚠️ **Note**: You must set up your environment variables correctly for all of this to work. Collection variables will be calculated between requests and used in subsequent requests. The naming convention used in the collection is to prefix collection variable keys with an underscore like `_tomsVariableKey` while  an environment variable should not contain an underscore. Example: `tomsVariableKey`. I would never recommend writing to environment variables at runtime. My approach is to keep these consistent across the collection and all folders across the collection and use them only when changing orgs, storefronts or users.

These are some *bad* examples. You shouldn't see a call like these in the collection and it's strongly recommended that you do not create them this way unless you like needless debugging:

1. `pm.collectionVariables.set('myVariable', 'My new value');`
2. `pm.collectionVariables.get('myVariable');`
3. `pm.environment.set('_myVariable', 'My new value');`
4. `pm.environment.get('_myVariable');`

These are good examples as they adhere to the established naming convention and it's clear which dictionary we're referring to:

1. `pm.collectionVariables.set('_myVariable', 'My new value');`
2. `pm.collectionVariables.get('_myVariable');`
3. `pm.environment.set('myVariable', 'My new value');`
4. `pm.environment.get('myVariable');`

I don't like mixing sources like dictionaries for retrieving values. A value with an underscore prefix in this naming convention should correspond to pm.collectionVariables and one without should come from. I don't use a context stand-in that would allow pulling or pushing a value by key from either pm.collectionVariables or pm.environment at runtime. I believe strongly that a few coding principals such as singular definition and not coding by coincidence - even with tests, actually - especially with tests can save time. If those terms are not familiar I'd like to recommend the book "The Pragmatic Programmer" as it could replace many on your shelf or device.

### Input values

#### Some Environment variables are used for lookups to support reuse

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

⚠️ __Note__: The naming convention found here is used across other Salesforce Commerce product Postman collections in the Partner Readiness space when possible to support reuse.

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

Please consult the curriculum and course documentation for additional details.

Enjoy the collection!
- Tom Zarr (tzarr@salesforce.com) September, 2023
