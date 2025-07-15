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

### Discharge Type
| Number Value | String Value |
| ----- | ----- |
| 1 | TotalDischarge |
| 2 | AgreementToDischarge |

### Lien Fields (Lien)
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

### Lien Fields (Repair Lien)
| Field Name | AB | BC | MB | NT | NU | ON | SK | YT |
|---|---|---|---|---|---|---|---|---|
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

### PartyTypeID
| String Value               | ID  | AB | BC | SK | MB | ON | QC | YT | NT | NU | NL | NB | NS | PE |
| -------------------------- | --- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| BusinessDebtor             |  1  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  |
| IndividualDebtor           |  2  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  |
| BusinessSecuredParty       |  3  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  |
| IndividualSecuredParty     |  4  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  | X  |
| BusinessRegisteringAgent   |  5  |    | X  | X  | X  | X  |    |    |    |    |    |    |    |    |
| IndividualRegisteringAgent |  6  |    | X  | X  | X  | X  |    |    |    |    |    |    |    |    |
| BusinessDealer             |  7  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| IndividualDealer           |  8  |    |    |    |    |    | X  |    |    |    |    |    |    |    |

### QCFormTypeID
| String Value | ID |
|--------------|----|
| RHa          | 1  |
| RDd          | 2  |
| RDe          | 3  |
| RG34         | 4  |
| RG41         | 5  |
| RV           | 6  |
| RRa          | 7  |
| RHf          | 8  |
| RDf          | 9  |
| RG80         | 10 |
| RE           | 11 |
| RDc          | 12 |
| RDb          | 13 |
| RG14         | 14 |
| RDa          | 15 |
| RG13         | 16 |

### Order Completion Times
| Jurisdiction | Basic Discharge Time | Basic Renewal Time |
|--------------|-------------------|-------------------|
| BC | 10s – 5 min | 10s – 5 min |
| AB | 2 – 5 min | 2 – 5 min |
| SK | 2 – 5 min | 2 – 5 min |
| MB | 2 – 5 min | 2 – 5 min |
| ON | 2 - 5 min* | 2 - 5 min* |
| QC | 1 – ? days† | N/A |
| YT | 1 - 5 min | 1 - 5 min |
| NT | 1 - 5 min | 1 - 5 min |
| NU | 1 - 5 min | 1 - 5 min |
| NL | 1 - 5 min | N/A |
| NB | 1 - 5 min | N/A |
| NS | 1 - 5 min | N/A |
| PE | 1 - 5 min | N/A |

\*Provided that the order is submitted inside Ontario business hours.

†Processing time in Quebec can vary wildly due to the registry's rules. Extreme cases can be over a week (usually around holidays), but during normal times orders usually complete in around 1 day.
