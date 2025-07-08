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
    "expiryDate": date,
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

Not all fields will be relevant in all jurisdictions. Please visit the values page for a table listing what fields are relevant in each jurisdiction.

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
