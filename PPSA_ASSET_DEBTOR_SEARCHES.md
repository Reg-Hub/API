# PPSA Asset and Debtor Search API Documentation

The following is an integration guide for asset and debtor searches. Both these service offerings are very similar and differ only in the endpoints that are used and the request bodies sent. The general flows for both requesting and results retreival will be identical for both servicces.

A quick note about turnaround times. These can vary wildly from 1-2 mins in very good cases to a business day or two in extreme cases. This will depend on search criteria and jurisdiction. The main problem jurisdiction is Ontario - if the search yields a high number of results they require a different search flow that can take up to two days to complete. If this is the case, the order will go into the status AwaitingHighVolumeResults. It's also worth noting that this can also occur in British Columbia, but in those cases the order is usually turned around in less than 30 minutes.

## Performing the Search

The general steps are as follows:
1. Create a draft for review
2. Correct any validation issues
3. Submit the order
4. Wait for results
5. Retreive results

The following sections will go into more depth on each of these steps.

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
