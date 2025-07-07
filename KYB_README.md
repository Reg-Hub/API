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
- Retrieve the business report results document at the /Document endpoint.
  - Set the "Accept" header to "application/json" for the JSON representation.
  - Set the "Accept" header to "application/pdf" for the PDF representation.
  - JSON and PDF are the only two formats available at the time of this writing (2025-07-07).
- Retreive the RegHub standardized business report at the /StandardizedDocument endpoint.
  - Set the "Accept" header to "application/pdf" for the PDF representation.
  - PDF is the only format available at the time of this writing (2025-07-07). The JSON format would be essentially the same as the /Document endpoint so you may just use that endpoint if you require the JSON representation.

It's worth noting that if you are very confident the entity exists and you already have the number for it, it is possible to skip the first (business search) workflow and go straight to the second (business report) workflow. However, it is recommended that you perform both workflows to avoid incurring costs on the second workflow in the case of mistaken numbers.

## Business Search Details

A typical request to create a business search order would look like the following:

```
{
  "referenceNumber": "your_reference_number",
  "jurisdiction": "_M",
  "country": "CA",
  "businessSearchCriteria": {
    "businessSearchCriteriaTypeID": "Name",
    "name": "business_name_criteria_as_a_string",
    "number": "business_number_criteria_as_a_string"
  },
  "searchParameters": {
    "searchJurisdictions": [
      "BC", "AB", "SK", "MB", "ON"
    ]
  }
}
```

You may search by either name or number. The possible values for the field businessSearchCriteriaTypeID are as follows:

- Name
- Number

The possible values for the searchJurisdictions field are as follows (and may be one to many):

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
| _F | Federal |

## Notes on Business Search Results Scoring

Business Search results are scored based on proximity of the result to the search criteria.  The score is a measurement of how many changes to the string are required to make the result match the criteria exactly (after some cleaning).  The search is completed with the criteria as provided, but for scoring results we clean both the search criteria and the result prior to scoring. Here are some examples of cleaning that will happen on both the search criteria and result:

- Remove all punctuation and special characters (@, ?, !, <, >, etc.)
- Remove all legal endings from business name (LTD, LLC, CO, etc.)
- Replace any accented characters with non-accented versions

Additionally, we add partial scores to entity which are no longer active or are registered extra-provincially. This is so that in the case of multiple results, active registrations in their home jurisdiction will be scored better.

If the search criteria (business name) is an exact character-for-character match of the result, you can expect a score of < 1. (0 if it's also active and registered in that jurisdiction). (edited

## Business Reports

A typical request to create a business report order would look like the following:

```
{
  "referenceNumber": "string",
  "jurisdiction": "NB",
  "country": "CA",
  "businessReportCriteria": {
    "number": "business_number_criteria_as_a_string",
    "businessName": "business_name_criteria_as_a_string"
  }
}
```

The search is performed by the number in every jurisdiction _except_ PE, which requires both name and number. For ease of integration purposes, you may pass both name and number for every jurisdiction and for jurisdictions outside of PE, the name will be ignored.
