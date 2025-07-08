## 1. Create a Draft for Review

Begin by sending a POST request to the appropriate endpoint

```
{root}/api/v1/Orders/Liens
{root}/api/v1/Orders/RepairLiens
```

With a request body in the following format:

```
{
  "referenceNumber": string,
  "jurisdiction": string,
  "country": string,
  "lien": {
    "term": integer,
    "isInfiniteTerm": boolean,
    "amount": decimal,
    "additionalInformation": string,
    "generalCollateral": {
      "collateralValue": string
    },
    "trustIndenture": boolean,
    "stillHavePossession": boolean,
    "vehicleReleasedDate": date,
    "workLastCompletedDate": date,
    "partsProvidedDate": date,
    "otherChanges": string,
    "courtOrder": string,
    "systemGeneratedRIN": boolean,
    "rin": string,
    "noRIN": boolean,
    "perfectionInAnotherJurisdiction": boolean,
    "purchaseMoneySecurityInterest": boolean,
    "receiverAppointment": boolean,
    "securityInterest": boolean,
    "mvIncluded": boolean,
    "cautionFiling": boolean,
    "consumerGoods": boolean,
    "inventory": boolean,
    "equipment": boolean,
    "accounts": boolean,
    "other": boolean,
    "noFixedMaturityDate": boolean,
    "maturityDate": date,
    "fileNumber": string,
    "qcFormTypeID": integer | string,
    "sumOfHypothec": string,
    "useDefaultSumOfHypothec": boolean,
    "signingDate": date,
    "signingCity": string,
    "signingJurisdiction": string,
    "signingCountry": string
  },
  "parties": [
    {
      "busName": string,
      "firstName": string,
      "middleName": string,
      "lastName": string,
      "isEstate": boolean,
      "dateOfBirth": date,
      "priority": integer | null,
      "reusableRegistryCode": string,
      "partyTypeID": integer | string,
      "generationID": integer | string,
      "contactDetails": {
        "email": string,
        "phoneNumber": string,
        "faxNumber": string,
        "address": {
          "streetNumber": string,
          "streetName": string,
          "unit": string,
          "city": string,
          "jurisdiction": string,
          "postalCode": string,
          "countryCode": string
        }
      },
      "linkedParties": [
        {
          "partyLinkType": number | string,
          "linkedPartyID": string | null,
          "linkedParty": party
        }
      ],
    }
  ],
  "assets": [
    {
      "serialNumber": string,
      "make": string,
      "model": string,
      "color": string,
      "year": integer,
      "priority": integer,
      "manufacturedHomeRegistrationNumber": string,
      "assetTypeID": integer | string
    }
  ]
}
```

The following is a brief explanation of the fields:

| Field | Type | Description |
| --- | --- | --- |
| referenceNumber | string | Can be any string - used for your internal reference only. Can be used to find an order later if you do not have the order ID. Reference numbers do not need to be unique across orders. Every order will end up getting its own unique identifier in its ID field. |
| jurisdiction | string | Only Canadian values are supported as of July 8, 2025. [See Values.md for valid values](https://github.com/Reg-Hub/API/blob/main/PPSA%20Searches/Values.md) |
| country | string | The only valid value (as of writing on July 8, 2025) is "CA" (Canada). We currently do not support PPSA searching outside of Canada. |
| asset | object | Contains information on the asset to be searched for asset searches. |
| serialNumber | string | The serial number (can be a VIN or other format, for example for equipment). |
| debtor | object | Contains information on the debtor to be searched for debtor searches. |
| busName | string/null | Name to be searched for a business debtor. |
| firstName | string/null | |
| middleName | string/null | |
| lastName | string/null | |
| dateOfBirth | date/null | |
| partyTypeID | number/string | This is an enumerated value. [See Values.md for valid values](https://github.com/Reg-Hub/API/blob/main/PPSA%20Searches/Values.md) |
| searchParameters | object | Contains additional search parameters. Not all fields will be available at every jurisdictional registry. |
| exactOnlyResults | boolean | Set to true to only retreive exact matches from the registry. Some registries have a very fuzzy matching scheme and will match even wildly different entries to the criteria provided. If you find you are getting too many results back on your searches for a given jurisdiction, consider setting this to true to only retreive exact matches. |
| searchFromDate | date/null | Available in Ontario, sets the maximum date in the past that will be included in the results. We do not recommend using this parameter unless you have a very specific use case for it. |
| searchToDate | date/null | Available in Ontario, sets the maximum date that will be included in the results. We do not recommend using this parameter unless you have a very specific use case for it. |

A successful request will have in its response a 201 Created status code, and the response body will contain an echo of the requested order with additional details (for example, unique identifiers will all be provided at this point). It's very important that you save the "id" property of the returned order as this is how you will interact with the system for this order going forward. Here is an example of a successfully requested order's response:

```
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Location: {root}/api/v1/Orders/2126f1c2-378e-4fda-f1f6-08ddbd9f205b

{
    "resource": {
        "id": "2126f1c2-378e-4fda-f1f6-08ddbd9f205b",
        "jurisdiction": "QC",
        "country": "CA",
        "orderTypeID": "DebtorSearch",
        "orderStatusTypeID": "Draft",
        "searchParameters": {
            "id": "54e4a839-37d2-48b7-48d2-08ddbd9f205d",
            "exactOnlyResults": false,
            "searchFromDate": null,
            "isChildSearch": false
        },
        "parties": [
            {
                "partyTypeID": "BusinessDebtor",
                "busName": "Business Name To Search",
                "firstName": null,
                "middleName": null,
                "lastName": null,
                "dateOfBirth": null
            }
        ],
        "referenceNumber": "Reference number for test"
    },
    "validation": {
        "errors": [],
        "warnings": [],
        "notices": []
    }
}
```

Note that not all headers in the response have been provided in the above example as most are irrelevant. Also note that some fields from the response body have been ommitted as they are irrelevant for the purposes of this guide.

Note that the "validations" section contains multiple empty arrays. Validation will be handled in the following section 2.

Once you have created your draft, [it is ready for review and submission](https://github.com/Reg-Hub/API/blob/main/PPSA%20Searches/2.%20Correct%20Any%20Validation%20Issues.md).
