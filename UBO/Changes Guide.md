
# Purpose

The purpose of this document is to describe the changes required to move from a "v1" implementation for UBO to a "v2" implementation. The major difference between the two "versions" is that an attestor may supply more than one level of ownership in a single attestation session. This _greatly_ improves the attestor experience as often the top level attestor will know the shareholders of the business shareholders they have provided. Under "v1" they would have had to input the shareholders at that top level, submit that attestation, then our client (you) would have to submit another business report order then UBO order to obtain the shareholders for each business shareholder at that top level to obtain those shareholders.

# High Level Discussion of Changes

The aforementioned improvements necessitated database schema changes and response shape changes. All of the data available in "v1" is still available in "v2" - retreiving the data is a mapping exercise. Shareholders are no longer found in the UBO order. Shareholders are available in actual tree form all at one endpoint.

The intial required steps (business search to validate the business number, business report to pull the data from the registry, and the initial UBO order) remain unchanged and are still required to initiate any ultimate beneficial ownership request.

The endpoint `/api/v1/Orders/{orderID}/HubReport/OwnershipSummary` remains unchanged between "v1" and "v2".

A business report order is no longer a prerequisite for ordering a UBO order at any level other than the first level. A UBO order may be ordered against any business shareholder. For example, if the attestor has provided 100% of the shareholders for business A, of which business B is holding 50% of, and 25% of the shareholders for business B, but your requirement is to collect at least 76% of the ownership percentage of business A, you would then want to order a UBO order on business B to collect the shareholders of that business.

The mechanism through which orders are placed for business shareholders has changed. Previously the original order ID was used, now the ID of the shareholder node itself is used.

# Mapping

Previously, shareholders existed at the endpoint 

```
GET
/api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}
```

at the path 

```
/ultimateBeneficialOwnershipRequest/shareholders
```

This was only shareholders supplied by one attestor for one level of the overall tree.

Now, shareholders exist at the endpoint

```
GET
Accept: application/vnd.reghub.hub-report-with-ubo-trees+json
/api/v1/Orders/{orderID}/HubReport
```

The order ID passed for this call can be _any_ business report or UBO order submitted for this hub report. This endpoint returns the current state of the _entire_ shareholder tree in the following shape. The swagger docs have some extra fields on there but these are the relevant ones for UBO orders:

```
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "hubReportType": "UltimateBeneficialOwnership",
  "reference": "string",
  "country": "string",
  "jurisdiction": "string",
  "uboTrees": [
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "hubReportID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "hubReportReference": "string",
      "parentID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "partyID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "party": {
        "busName": "string",
        "firstName": "string",
        "middleName": "string",
        "lastName": "string",
        "isEstate": true,
        "dateOfBirth": "2026-06-24T16:25:08.085Z",
        "busNumber": "string",
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "partyTypeID": "IndividualShareholder",
        "contactDetails": {
          "email": "string",
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "address": {
            "unit": "string",
            "streetNumber": "string",
            "streetName": "string",
            "streetType": "string",
            "quadrant": "st",
            "city": "string",
            "postalCode": "string",
            "jurisdiction": "st",
            "countryCode": "st",
            "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          },
        },
        "name": "string"
      },
      "orders": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "referenceNumber": "string",
          "orderTypeID": "BusinessReport",
          "orderStatusTypeID": "Draft",
          "jurisdiction": "string",
          "displayJurisdiction": "string",
          "country": "string",
          "dateRequested": "2026-06-24T16:25:08.086Z",
          "orderStatusUpdatedDate": "2026-06-24T16:25:08.086Z",
          "isVisible": true,
          "orderTypeName": "string",
          "description": "string",
          "orderGroupID": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
        }
      ],
      "children": [
        { } // This is a recursive array. The elements in this array have the same shape as this parent element.
      ],
      "ownershipPercentageOfParent": 0,
      "ownershipPercentageOfRoot": 0,
      "remainingOwnershipPercentage": 0,
      "isValidated": true,
      "isUnregisteredEntity": true,
      "businessCountry": "string",
      "businessJurisdiction": "string",
      "hasDeclaredNoSignificantShareholders": true
    }
  ]
}
```

### The following describes each field and how it maps from "v1" to "v2" if applicable. If not, it is listed as a new field.

- hubReportType
  - New field
  - This field only has one possible value at this time: "UltimateBeneficialOwnership".

- reference
  - Previously found at endpoint `GET /api/v1/Orders/{orderID}/HubReport and path: `/reference`
  - This field is copied from the reference passed from the initial business report order.

- country
  - New field
  - This field is copied from the country passed from the initial business report order.

- jurisdiction
  - Previously found at endpoint `GET /api/v1/Orders/{orderID}/HubReport` and path: `/jurisdiction`

- uboTrees
  - New field
  - This is an array of top shareholder nodes. For the purposes of this response the original entity of interest is treated as a shareholder of itself owning 100% of itself. It is an array for future capability plans. You may assume it contains one and only one element.

- uboTrees/{index}/parentID
  - Analogous to the path `/hubReportOrders/{index}/parentHubReportOrderID` found at endpoint `GET /api/v1/Orders/{orderID}/HubReport` but not a direct one to one mapping.
  - This will always be null for the root entity (the entity that we are trying to discover the ultimate beneficial ownership for).

- uboTrees/{index}/party
  - Previously found at endpoint `GET /api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}` and path: `/ultimateBeneficialOwnershipRequest/shareholders/{index}/party`
  - ALL FIELDS INSIDE THIS OBJECT/ELEMENT ARE THE SAME AS "v1"

- uboTrees/{index}/orders
  - New field
  - Previously orders were the nodes in the tree, now the shareholders are the nodes that may have orders attached to them. You can tell by looking at the elements in this array if a UBO or business report order have been ordered for this business shareholder.

- uboTrees/{index}/children
  - New field 
  - Business shareholders only 
  - _These elements have the exact same shape as the above schema._ This is a JSON representation of a tree of shareholders.

- uboTrees/{index}/ownershipPercentageOfParent
  - Previously found at endpoint `GET /api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}` and path: `/ultimateBeneficialOwnershipRequest/shareholders/{index}/ownershipPercentage`
  - This lists the ownership percentage _this_ shareholder holds of it's _immediate parent._

- uboTrees/{index}/ownershipPercentageOfRoot
  - New field 
  - This lists the ownership percentage _this_ shareholder holds of the _top level_ entity.
  - This, compared with the sum of this business shareholder's child shareholder percentages, can be used to easily determine if the information already provided for this business shareholder meets your requirements.

- uboTrees/{index}/isValidated
  - Previously found at endpoint `GET /api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}` and path: `/ultimateBeneficialOwnershipRequest/shareholders/{index}/isValidated`
  - Business shareholders only 
  - This tells if the attestor validated this shareholder (if it is a business shareholder). It's not recommended to order a business report off a shareholder where this is false as there is a high chance of not receiving results.

- uboTrees/{index}/isUnregisteredEntity
  - New field
  - Business shareholders only
  - This tells if the attestor explicitly listed this shareholder as unregistered. Do not order a business report for this entity as you will not receive results.

- uboTrees/{index}/businessCountry
  - Previously found at endpoint `GET /api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}` and path: `/ultimateBeneficialOwnershipRequest/shareholders/{index}/businessCountry`
  - Business shareholders only
  - This tells where the business is registered. This may be different than the shareholder's address country.

- uboTrees/{index}/businessJurisdiction
  - Previously found at endpoint `GET /api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}` and path: `/ultimateBeneficialOwnershipRequest/shareholders/{index}/businessJurisdiction`
  - Business shareholders only 
  - This tells where the business is registered. This may be different than the shareholder's address jurisdiction.

- uboTrees/{index}/hasDeclaredNoSignificantShareholders
  - New field
  - Business shareholders only 
  - This tells when the attestore has listed this shareholder as having no shareholders owning 25% or more.

### Further Business Report Orders

Some attestation templates have the ability to automatically order business report orders on validated business shareholders up to a certain level or percentage of ownership. If this is the case for your template, this section is skippable.

Under "v1" you would place a business report order at the following endpoint for a business shareholder:

```
POST
/api/v1/Orders/BusinessReports
```

```
{
  "referenceNumber": "string",
  "orderTypeID": "BusinessReport",
  "jurisdiction": "string",
  "country": "string",
  "originalOrderID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "businessReportCriteria": {
    "number": "string",
    "businessName": "string",
  }
}
```

Where the originalOrderID was the order ID of the business report for that business shareholder. Under "v2" you place the order at the same endpoint but with a request body like so:

```
{
  "referenceNumber": "string",
  "orderTypeID": "BusinessReport",
  "jurisdiction": "string",
  "country": "string",
  "uboTreeNodeID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "businessReportCriteria": {
    "number": "string",
    "businessName": "string",
  }
}
```

Note the only difference is originalOrderID has been swapped out for uboTreeNodeID. UboTreeNodeID should refer to the ID of the actual business shareholder from the above response.


### Further Ultimate Beneficial Ownership Report Orders

Under "v1" you would place an ultimate beneficial ownership report order at the following endpoint for a business shareholder:

```
POST
/api/v1/Orders/UltimateBeneficialOwnershipReports
```

```
{
  "referenceNumber": "string",
  "originalOrderID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "ultimateBeneficialOwnershipRequest": {
    "templateID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  },
  "attestor": {
    "busName": "string",
    "busNumber": "string",
    "firstName": "string",
    "middleName": "string",
    "lastName": "string",
    "contactDetails": {
      "email": "string",
      "phoneNumber": "string",
      "address": {
        "unit": "string",
        "streetNumber": "string",
        "streetName": "string",
        "streetType": "string",
        "quadrant": "string",
        "city": "string",
        "postalCode": "string",
        "jurisdiction": "string",
        "countryCode": "string"
      }
    }
  }
}
```

Where the originalOrderID was the order ID of the business report for that business shareholder. Under "v2" you place the order at the same endpoint but with a request body like so:

```
{
  "referenceNumber": "string",
  "uboTreeNodeID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "ultimateBeneficialOwnershipRequest": {
    "templateID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  },
  "attestor": {
    "busName": "string",
    "busNumber": "string",
    "firstName": "string",
    "middleName": "string",
    "lastName": "string",
    "contactDetails": {
      "email": "string",
      "phoneNumber": "string",
      "address": {
        "unit": "string",
        "streetNumber": "string",
        "streetName": "string",
        "streetType": "string",
        "quadrant": "string",
        "city": "string",
        "postalCode": "string",
        "jurisdiction": "string",
        "countryCode": "string"
      }
    }
  }
}
```

Note the only difference is originalOrderID has been swapped out for uboTreeNodeID. UboTreeNodeID should refer to the ID of the actual business shareholder from the above response.
