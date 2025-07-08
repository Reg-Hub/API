# Valid Values
### Jurisdiction

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

# Field Relevance
### Lien
| Field Name | AB | BC | MB | NB | NL | NS | NT | NU | ON | PE | QC* | SK | YT |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| term | X | X | X | X | X | X | X | X | X | X | X | X | X | 
| isInfiniteTerm | X | X | X | X | X | X | X | X | X | X | X | X | X |
| expiryDate | | | X | | | | | | X | X | |
| amount | | | | | | | | | | | X | | |
| additionalInformation | X | | X | X | X | X | X | X | | X | X | | X |
| generalCollateral.collateralValue | X | X | X | X | X | X | X | X | X | X | X | X | X |
| trustIndenture | X | X | X | | | | | | | | | X | |
| stillHavePossession | | | | | | | | | | | | | |
| vehicleReleasedDate | | | | | | | | | | | | | |
| workLastCompletedDate | | | | | | | | | | | | | |
| partsProvidedDate | | | | | | | | | | | | | |
| otherChanges | X | | | | | | | | | | | | |
| courtOrder | X | | | | | | | | | | | | |
| surrenderDate | | | | | | | | | | | | | |
| serviceLastRendered | | | | | | | | | | | | | |
| dateOfLien | | | | | | | | | | | | | |
| systemGeneratedRIN | | | | | | | | | | | | X | |
| rin | | | | | | | | | | | | | X |
| noRIN | | | | | | | | | | | | X | |
| perfectionInAnotherJurisdiction | | | X | | | | | | | | | | |
| purchaseMoneySecurityInterest | | | X | | | | | | | | | | |
| receiverAppointment | | | X | | | | | | | | | | |
| securityInterest | | | X | | | | | | | | | | |
| mvIncluded | | | | | | | | | X | | | | |
| cautionFiling | | | | | | | | | X | | | | |
| consumerGoods | | | | | | | | | X | | | | |
| inventory | | | | | | | | | X | | | | |
| equipment | | | | | | | | | X | | | | |
| accounts | | | | | | | | | X | | | | |
| other | | | | | | | | | X | | | | |
| noFixedMaturityDate | | | | | | | | | X | | | | |
| maturityDate | | | | | | | | | X | | | | |
| qcFormTypeID | | | | | | | | | | | X | | |
| sumOfHypothec | | | | | | | | | | | X | | |
| useDefaultSumOfHypothec | | | | | | | | | | | X | | |
| signingDate | | | | | | | | | | | X | | |
| signingCity | | | | | | | | | | | X | | |
| signingJurisdiction | | | | | | | | | | | X | | |
| signingCountry | | | | | | | | | | | X | | |

\*QC field relevance will depend on the value of qcFormTypeID. During integration you should be told by your business team/project manager what form types to be using for QC.

### Repair Lien
| Field Name | AB | BC | MB | NT | NU | ON | SK | YT |
|---|---|---|---|---|---|---|---|
| term | | X | | | | X | X | |
| isInfiniteTerm | | X | | | | | X | |
| expiryDate | | | | | | | X | |
| amount | | | | | | | | |
| additionalInformation | X | | | X | X | | | X |
| generalCollateral.collateralValue | | X | | | | X | X | |
| trustIndenture | | | | | | | | |
| stillHavePossession | X | | X | | | | | |
| vehicleReleasedDate | X | | | | | | | |
| workLastCompletedDate | X | | | | | | | |
| partsProvidedDate | X | | | | | | | |
| otherChanges | X | | | | | | | |
| courtOrder | X | | | | | | | |
| surrenderDate | | | X | | | | | |
| serviceLastRendered | | | X | | | | | |
| dateOfLien | | | | X | X | | | X |
| systemGeneratedRIN | | | | | | | X | |
| rin | | | | | | | X | |
| noRIN | | | | | | | | |
| perfectionInAnotherJurisdiction | | | | | | | | |
| purchaseMoneySecurityInterest | | | | | | | | |
| receiverAppointment | | | | | | | | |
| securityInterest | | | | | | | | |
| mvIncluded | | | | | | X | | |
| cautionFiling | | | | | | | | |
| consumerGoods | | | | | | | | |
| inventory | | | | | | | | |
| equipment | | | | | | | | |
| accounts | | | | | | | | |
| other | | | | | | | | |
| noFixedMaturityDate | | | | | | | | |
| maturityDate | | | | | | | | |
| qcFormTypeID | | | | | | | | |
| sumOfHypothec | | | | | | | | |
| useDefaultSumOfHypothec | | | | | | | | |
| signingDate | | | | | | | | |
| signingCity | | | | | | | | |
| signingJurisdiction | | | | | | | | |
| signingCountry | | | | | | | | |
