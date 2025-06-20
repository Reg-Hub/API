# Asset Multi Search API Documentation

The following is an integration guide for asset multi searches. This service allows easier ordering of multi-jurisdictional asset searches - rather than having to individually order each asset search for 2+ jurisdictions, you can use the AssetMultiSearch order type. Pass the serial number and the list of jurisdictions to be searched. Under the hood, this order type will still spin up individual orders for each jursidiction as each jurisdictional registry has different requirements and completion times for searching. 

The 'parent' asset multi search order will only be referred to as the parent order from this point on in the documentation, and the 'child' orders will only be referred to as a child or the children from this point on.

The parent order will complete when and only when all the child orders have been completed. Each child will have all the normal information at all the normal endpoints (summary, registry document, parsed results), but the parent multi will have an aggregated summary containing high level information about results found in each jurisdiction searched, as well as all the registry documents combined into one pdf. The parent, at the tiem of this writing (2025/06/20) does not supply aggregated parsed results but this is on our roadmap. The parent order does not come with a fee associated with it at the time of this writing (2025/06/20) but each child order will have the regular fees attached.

A quick note about turnaround times. These can vary wildly from 1-2 mins in very good cases to a business day or two in extreme cases. This will depend on search criteria and jurisdiction. The main problem jurisdiction is Ontario - if the search yields a high number of results they require a different search flow that can take up to two days to complete. If this is the case, the order will go into the status AwaitingHighVolumeResults. It's also worth noting that this can also occur in British Columbia, but in those cases the order is usually turned around in less than 30 minutes.

## Performing the Search

The flow is largely the same as other orders sent to RegHub APIs. Create your draft, submit the order, then wait for results. However because the parent order creates multiple child orders, there are some potential extra complications in validation and results retreival.

Begin by sending a POST request to the endpoint

```
{root}/api/v1/Orders/AssetMultiSearches
```

With a request body in the format

```
{
  "referenceNumber": "your reference number here",
  "orderTypeID": "AssetMultiSearch",
  "assets": [
    {
      "serialNumber": "your serial number here"
    }
  ],
  "searchParameters": {
        "searchJurisdictions": [
            {"jurisdiction": "BC"},
            {"jurisdiction": "AB"},
            {"jurisdiction": "SK"},
            {"jurisdiction": "MB"},
            {"jurisdiction": "ON"},
            {"jurisdiction": "QC"},
            {"jurisdiction": "YT"},
            {"jurisdiction": "NT"},
            {"jurisdiction": "NU"},
            {"jurisdiction": "NB"},
            {"jurisdiction": "NL"},
            {"jurisdiction": "NS"},
            {"jurisdiction": "PE"}
        ]
    }
}
```

This call will almost always return 201 Created with a representation of the created order. It will sometimes return 400 BadRequest if a breaking field was not supplied (eg. orderTypeID).

Note that the above is for a Canada wide asset search. You can supply one or more jurisdiction. Note that the order will still work if you supply only one jurisdiction, though there are no benefits to submitting an asset multi search order if you are searching one jurisdiction and may as well use the regular asset search.

If there are validation issues with **any** of the child orders, you will receive a single validation error or a single validation warning on the parent order like the following:

```
"errors": [
    {
        "path": "",
        "message": "All orders in the OrderGroup must be valid.",
        "userFriendlyMessage": "All orders in the group must be valid.",
        "entityID": "d999d048-7b50-4826-7c99-08ddb00b475e",
        "showForClient": true
    }
]
```

As with the normal order flow, you will not be allowed to submit the parent order until all the validation errors are cleared, which would mean clearing validation errors on the child orders. It's worth noting that on asset multi searches the list of possible validation errors on a child order is quite short, and will be mostly either that a serial number has not been supplied at all or that the serial number contains invalid characters for a particular jurisdiction. To find out what the validation errors are for the individual orders, you can perform a request in the following basic format:

```
GET {root}/api/v1/Orders?orderGroupID={orderGroupID}&pageSize=100
Accept: application/vnd.reghub.order-with-validation+json
```

This call will return a paginated resource of all the orders in the group (including the parent order). **Note that the default page size is 10**, so we suggest hardcoding the pageSize query parameter to 100 to ensure that all the orders in the group are always returned. The order group ID can be found on the parent order at the path /orderGroup/ID. **Note the accept header value.** This content type will include orders with their validation issues in the following format:

```
[
  {
    "resource": {
      //order representation here in the normal format
    },
    "validation: {
      //validation information here in the normal format
    }
  },
  ...
]
```

Child orders can be updated at the endpoints

```
PUT {root}/api/v1/Orders/AssetSearches/{childOrderID}
PATCH {root}/api/v1/Orders/AssetSearches/{childOrderID}
```

Once you have a fully valid asset multi search order, you can simply submit using the endpoint

```
PUT {root}/api/v1/Orders/AssetMultiSearches/{parentOrderID}
```

This will return, if successful, a 204 NoContent response with no response body (as with all other RegHub order flows). From here, you can either poll or use the callback strategy (fairly new as of June 2025, please ask for more information on integrating with this) to be alerted to when your order is complete. If you are using the polling strategy, we recommend the endpoint

```
GET {root}/api/v1/Orders/{parentOrderID}/Status
```

When the order is complete, this endpoint will return a response body like so

```
{
  "status": "Complete"
}
```

When the status endpoint returns the above response body, this means that the parent order and all child orders are complete. This endpoint will not return this response body until all child orders and the parent order are complete. 

## Results Retreival

Once you know the parent order is completed, there will be multiple documents available both on the parent order and on each child order. There is an aggregated summary available on the parent order at the endpoint (note: only available in pdf at this time)

```
GET {root}/api/v1/Orders/AssetMultiSearches/{parentOrderID}/Summary
Accept: application/pdf
```

There is an aggregated registry document (just all the registry documents combined into one PDF) at the endpoint

```
GET {root}/api/v1/Orders/AssetMultiSearches/{parentOrderID}/Document
Accept: application/pdf
```

Individual summaries for each child order are available in both json and pdf form

```
GET {root}/api/v1/Orders/AssetSearches/{childOrderID}/Summary
Accept: application/json
Accept: application/pdf
```

Individual registry results for each child order are available in both json and pdf form

```
GET {root}/api/v1/Orders/AssetSearches/{childOrderID}/Document
Accept: application/json
Accept: application/pdf
```

Note that the result of calling the above endpoint with the accept header set to application/json is the same as calling the below endpoint

```
GET {root}/api/v1/Orders/AssetSearches/{childOrderID}/Results/ParsedResults
```
