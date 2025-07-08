# PPSA Asset and Debtor Search API Documentation

The following is an integration guide for asset and debtor searches. Both these service offerings are very similar and differ only in the endpoints that are used and the request bodies sent. The general flows for both requesting and results retreival will be identical for both servicces.

A quick note about turnaround times. These can vary wildly from 1-2 mins in very good cases to a business day or two in extreme cases. This will depend on search criteria and jurisdiction. The main problem jurisdiction is Ontario - if the search yields a high number of results they require a different search flow that can take up to two days to complete. If this is the case, the order will go into the status AwaitingHighVolumeResults. It's also worth noting that this can also occur in British Columbia, but in those cases the order is usually turned around in less than 30 minutes.

## Performing the Search

The general steps are as follows:
1. [Create a draft for review](https://github.com/Reg-Hub/API/blob/main/PPSA%20Searches/1.%20Create%20a%20Draft%20for%20Review.md)
2. Correct any validation issues
3. Submit the order and wait for results
4. Retreive results

The following sections will go into more depth on each of these steps.
