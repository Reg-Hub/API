# Purpose

The purpose of this document is to describe the changes required to move from a "v1" implementation for UBO to a "v2" implementation. The major difference between the two "versions" is that an attestor may supply more than one level of ownership in a single attestation session. This _greatly_ improves the attestor experience as often the top level attestor will know the shareholders of the business they are attesting to. Under "v1" they would have had to input the shareholders at that top level, submit that attestation, then our client (you) would have to submit another business report order then UBO order to obtain the shareholders for each business shareholder at that top level to obtain those shareholders.

# Discussion of Changes

The afforementioned improvements necessitated database schema changes and response shape changes. All of the data available in "v1" is still available in "v2" - retreiving the data is a mapping exercise. Shareholders are no longer found in the UBO order. Shareholders are available in actual tree form all at one endpoint.

A business report order is no longer a prerequisite for ordering a UBO order at any level other than the first level. A UBO order may be ordered against any business shareholder that does not itself have business shareholders. For example, if the attestor has provided 100% of the shareholders for business A, of which business B is holding 50% of, and 25% of the shareholders for business B, but your requirement is to collect at least 76% of the ownership percentage of business A, you would then want to order a UBO order on business A to collect the shareholders of that business.

# Mapping

Previously, shareholders existed at the path endpoint 
```
/api/v1/Orders/UltimateBeneficialOwnershipReports/{orderID}
```
