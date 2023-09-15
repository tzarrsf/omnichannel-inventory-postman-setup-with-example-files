# Omnichannel Inventory Postman Setup with example files

This material is supplemental to the Omnichannel Inventory Partner Learning Camp course. See the course for the complete setup including configuring OpenSLL and a JWT (JSON Web Token) on Windows.

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

## The variables in the environment can be altered for your Connect API calls as long as they are comma delimited:

1. productStockKeepingUnitsCommaDelimited - e.g: PROSE,B-C-COFMAC-001,ESP-IOT-1,ID-PEM,PS-EL,PS-INF,TR-COFMAC-001
2. locationGroupIdentifiersCommaDelimited - e.g: LocationGroup01,LocationGroup02
3. locationIdentifiersCommaDelimited - e.g: Warehouse01,CaliforniaWarehouse,VirginiaWarehouse,Toronto Facility

## You will need to set up two connected apps in your org if you want to be able to use both the Headless and Connect APIs:
- Postman_OCI
- Postman_OCI_ConnectApi

Consult the course documentation for details.
