# Know Your Business (KYB) API Documentation

The following is a general explanation of the workflow for performing a KYB search.

## Summary Of Endpoints

There are two general collections of endpoints and corresponding order types for a KYB workflow. One is the BusinessSearch set of endpoints, and the other is the BusinessReport set of endpoints. The BusinessSearch order type (also may be known as pre-search in some vernaculars) is used to verify an entity exists and to retrieve some very basic information on it. The BusinessReport order type is used when you have verified that an entity exists (either via a BusinessSearch order or some other means) and you want to retrieve the details (profile) of that entity.

## Workflow

A typical workflow would look like the following (each step is one or more API calls):

- Create a business search order with the name or number you wish to search for via the POST endpoint.
- Submit the created business search order via the /Submit endpoint.
- Wait and poll for the business search order to be in status "Complete" (these orders will take less than 10 seconds to complete the vast majority of the time).
- Retrieve the business search results at the /Results endpoint.

At this point, you must perform decisioning on whether or not to continue based on the results returned. If you are continuing, the business report workflow would look like the following:

- Create a business report order with the number you wish to get a profile for via the POST endpoint
- Submit the created business report order via the /Submit endpoint
- Wait and poll for the business report order to be in status "Complete" (the average completion time for these orders can vary from a few seconds to 2 minutes).
- Retrieve the business report results document at the /Results/Document endpoint.
- Retrieve the business report parsed results at the /Results/ParsedResults endpoint.

It's worth noting that if you are very confident the entity exists and you already have the number for it, it is possible to skip the first (business search) workflow and go straight to the second (business report) workflow. However, it is recommended that you perform both workflows to avoid incurring costs on the second workflow in the case of mistaken numbers.

## Business Search Details

A typical request to create a business search order would look like this:

```
{
  "referenceNumber": "your_reference_number",
  "jurisdiction": "_M",
  "country": "CA",
  "businessSearchCriteria": {
    "businessSearchCriteriaTypeID": "Name",
    "name": "your_criteria"
    "number": null
  },
  "searchParameters": {
    "searchJurisdictions": [
      "BC", "AB", "SK", "MB", "ON"
    ]
  }
}
```

You may search by either name or number. The possible values for the field businessSearchCriteriaTypeID are as follows:

| Name |
| Number |

The possible values for the searchJurisdictions field are as follows (and may be one to many):

| BC | AB | SK | MB | ON | QC | YT | NT | NU | NL | NB | NS | PE | _F |
