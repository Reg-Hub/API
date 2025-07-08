

## 1. Create a Draft for Review

Begin by sending a POST request to the appropriate endpoint

```
{root}/api/v1/Orders/AssetSearches
{root}/api/v1/Orders/DebtorSearches
```

With a request body in the format for asset searches:

```
{
  "referenceNumber": string,
  "jurisdiction": string,
  "country": string,
  "asset": {
    "serialNumber": string
  },
  "searchParameters": {
    "exactOnlyResults": boolean
  }
```

Or in the format for debtor searches:

```
{
    "referenceNumber": string,
    "jurisdiction": string,
    "country": string,
    "debtor":
        {
            "busName": string,
            "partyTypeID": number | string
        },
    "searchParameters": {
      "exactOnlyResults": boolean,
      "searchFromDate": date,
      "searchToDate": date
    }
}
```

The following is a brief explanation of the fields:

#### Reference Number
- Type: string
- Description: Can be any string - used for your internal reference only. Can be used to find an order later if you do not have the order ID. Reference numbers do not need to be unique across orders. Every order will end up getting its own unique identifier in its ID field.

#### Jurisdiction
- Type: string
- Max length: 2
- See below table for allowed values (as of writing on July 8, 2025)
  
| Value | Full name (not used in API, only provided here for clarity) |
| ----- | ----------------------------------------------------------- |
| BC | British Columbia |
| AB | Alberta |
| SK | Saskatchewan |
| MB | Manitoba |
| ON | Ontario |
| QC | Quebec |
| YT | Yukon |
| NT | Northwest Territories |
| NU | Nunavut |
| NL | Newfoundland and Labrador |
| NB | New Brunswick |
| NS | Nova Scotia |
| PE | Prince Edward Island |
