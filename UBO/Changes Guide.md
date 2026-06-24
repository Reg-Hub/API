# Purpose

The purpose of this document is to describe the changes required to move from a "v1" implementation for UBO to a "v2" implementation. The major difference between the two "versions" is that an attestor may supply more than one level of ownership in a single attestation session. This _greatly_ improves the attestor experience as often the top level attestor will know the shareholders of the business they are attesting to. Under "v1" they would have had to input the shareholders at that top level, submit that attestation, then our client (you) would have to submit another business report order then UBO order to obtain the shareholders for each business shareholder at that top level to obtain those shareholders.

# Discussion of Changes

The afforementioned improvements necessitated database schema changes and response shape changes. All of the data available in "v1" is still available in "v2" - retreiving the data is a mapping exercise. Shareholders are no longer found in the UBO order. Shareholders are available in actual tree form all at one endpoint.

A business report order is no longer a prerequisite for ordering a UBO order at any level other than the first level. A UBO order may be ordered against any business shareholder that does not itself have business shareholders. For example, if the attestor has provided 100% of the shareholders for business A, of which business B is holding 50% of, and 25% of the shareholders for business B, but your requirement is to collect at least 76% of the ownership percentage of business A, you would then want to order a UBO order on business A to collect the shareholders of that business.

# Mapping

Previously, shareholders existed at the endpoint 

```
/api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}
```

at the path 

```
/UltimateBeneficialOwnershipRequest/Shareholders
```

This was only shareholders supplied by one attestor for one level of the overall tree.

Now, shareholders exist at the endpoint

```
Accept: application/vnd.reghub.hub-report-with-ubo-trees+json
/api/v1/Orders/{orderID}/HubReport
```

The order ID passed for this call can be _any_ business report or UBO order submitted for this hub report. This endpoint returns the current state of the _entire_ shareholder tree in the following shape. The swagger docs have some extra fields on there but these are the relevant ones for UBO orders:

```
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "envelopeID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
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
        "jobTitle": "string",
        "hasOneNameOnly": true,
        "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
        "partyType": {
          "id": "None",
          "name": "string"
        },
        "partyTypeID": "None",
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
      "createdForOrderID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
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
