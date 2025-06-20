# Asset Multi Search API Documentation

The following is an integration guide for asset multi searches. This service allows easier ordering of multi-jurisdictional asset searches - rather than having to individually order each asset search for 2+ jurisdictions, you can use the AssetMultiSearch order type. Pass the serial number and the list of jurisdictions to be searched. Under the hood, this order type will still spin up individual orders for each jursidiction as each jurisdictional registry has different requirements and completion times for searching. 

The 'parent' asset multi search order will only be referred to as the parent order from this point on in the documentation, and the 'child' orders will only be referred to as a child or the children from this point on.

The parent order will complete when and only when all the child have been completed. Each child will have all the normal information at all the normal endpoints (summary, registry document, parsed results), but the parent multi will have an aggregated summary containing high level information about results found in each jurisdiction searched, as well as all the registry documents combined into one pdf. The parent, at the tiem of this writing (2025/06/20) does not supply aggregated parsed results but this is on our roadmap.
