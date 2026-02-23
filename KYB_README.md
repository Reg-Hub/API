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
  "referenceNumber": "string",
  "jurisdiction": "_M", <---- Always "_M" for this order type
  "country": "CA",
  "businessSearchCriteria": {
    "businessSearchCriteriaTypeID": "string",
    "businessSearchScopeID": enumerated,
    "name": "string",
    "number": "string"
  },
  "searchParameters": {
    "searchJurisdictions": [
      "AB", "BC" ...
    ]
  }
}
```

You may search by either name or number. The possible values for the field businessSearchCriteriaTypeID are as follows:

- Name
- Number

The field "businessSearchScopeID has the possible values (see below for an explanation):

- Full
- Fast
  
If not provided, this will default to however your account is configured. In the "Full" scope, the order will reach out to each individual provincial registry along with the centralized Canadian database in order to perform the search. This takes significantly longer (~20-35s) than the "Fast" version. With the "Fast" scope, we only reach out to the centralized database, and this usually takes only a second or two, but could take up to ~5-10s with vague criteria. So the tradeoff between "Full" and "Fast" is comprehensive results but slower turnaround with "Full", and less comprehensive results but much faster turnaround time with "Fast". We generally recommend you stay with "Full" unless you have business reasons to need a very fast turnaround time.

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

Prior to performing any scoring we perform some normalization of the result string. this includes
- Removing all punctuation and special characters (@, ?, !, <, >, etc.)
- Removing all legal endings from business name (LTD, LLC, CO, etc.)
- Replacing any accented characters with non-accented versions
- Changing all letters to upper case (in effect performing case-insensitive scoring)

The scoring works as follows:
- We start by performing a version of the [Levenshtein string distance algorithm](https://en.wikipedia.org/wiki/Levenshtein_distance) to determine the initial distance between the criteria string and each result string.
- If a result is an EP (extra-provincial registration) it adds 0.25 to the score. This is because normally you want to pull the report on the entity in its home jurisdiction as that report will have the current status and the directors/officers. Reports on EPs (depending on the jurisdiction) often show significantly less data. For example, Ontario EP reports do not show the status, directors, or officers of the entity; they tell you to refer to the home jurisdiction.
- If a result is inactive it adds 0.25 to the score. This is to favor active entities.

_A score of 0 is an exact match. The lower the score, the stronger the match._

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
