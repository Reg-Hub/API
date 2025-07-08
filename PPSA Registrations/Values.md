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

#### AssetTypeID (Lien)
| String Value                                | ID  | AB | BC | SK | MB | ON | QC | YT | NT | NU | NL | NB | NS | PE |
| ------------------------------------------- | --- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| MotorVehicle                                |  1  | X  | X  | X  | X  | X  |    | X  | X  | X  | X  | X  | X  | X  |
| Trailer                                     |  2  | X  | X  | X  | X  |    |    | X  | X  | X  | X  | X  | X  | X  |
| Boat                                        |  3  | X  | X  | X  | X  |    |    | X  | X  | X  | X  | X  | X  | X  |
| MobileHome                                  |  4  | X  |    | X  | X  |    |    | X  | X  | X  | X  | X  | X  | X  |
| OutboardMotor                               |  5  | X  | X  | X  | X  |    |    | X  | X  | X  | X  | X  | X  | X  |
| AircraftRegisteredInCanada                  |  6  |    |    |    |    |    |    | X  | X  | X  | X  | X  | X  | X  |
| AircraftNonRegisteredInCanada               |  7  |    |    |    |    |    |    | X  | X  | X  | X  | X  | X  | X  |
| ManufacturedHome                            |  8  | X  | X  |    |    |    |    |    |    |    |    |    |    |    |
| FarmVehicle                                 |  9  |    |    |    |    |    |    |    |    |    |    |    |    |    |
| Motorcycle                                  | 10  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| Bus                                         | 11  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| MiniBus                                     | 12  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| Taxi                                        | 13  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| AllTerrainVehicle                           | 14  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| SnowmobilePost1988                          | 15  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| PassengerVehicle                            | 16  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| EmergencyVehicle                            | 17  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| CommercialVehicle                           | 18  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| Other                                       | 19  |    |    |    | X  |    |    |    |    |    |    |    |    |    |
| AircraftCanada                              | 20  | X  |    |    |    |    |    |    |    |    |    |    |    |    |
| AircraftForeign                             | 21  | X  |    |    |    |    |    |    |    |    |    |    |    |    |
| AircraftAirframeRegisteredInCanada          | 22  |    | X  |    |    |    |    |    |    |    |    |    |    |    |
| AircraftAirframeNotRegisteredInCanada       | 23  |    | X  |    |    |    |    |    |    |    |    |    |    |    |
| Aircraft                                    | 24  |    |    |    | X  |    |    |    |    |    |    |    |    |    |
| AircraftDOT                                 | 25  |    |    | X  |    |    |    |    |    |    |    |    |    |    |
| AircraftSerial                              | 26  |    |    | X  |    |    |    |    |    |    |    |    |    |    |
| TrailerOrSemiTrailer                        | 27  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| MotorHome                                   | 28  |    |    |    |    |    | X  |    |    |    |    |    |    |    |

#### AssetTypeID (RepairLien)
| String Value                                | ID  | AB | BC | SK | MB | ON | YT | NT | NU |
| ------------------------------------------- | --- | -- | -- | -- | -- | -- | -- | -- | -- |
| MotorVehicle                                |  1  | X  | X  | X  | X  | X  | X  | X  | X  |
| Trailer                                     |  2  |    | X  | X  | X  |    | X  | X  | X  |
| Boat                                        |  3  | X  | X  | X  | X  |    | X  | X  | X  |
| MobileHome                                  |  4  |    |    | X  | X  |    | X  | X  | X  |
| OutboardMotor                               |  5  | X  | X  | X  | X  |    | X  | X  | X  |
| AircraftRegisteredInCanada                  |  6  |    |    |    |    |    | X  | X  | X  |
| AircraftNonRegisteredInCanada               |  7  |    |    |    |    |    | X  | X  | X  |
| ManufacturedHome                            |  8  |    |    |    |    |    |    |    |    |
| FarmVehicle                                 |  9  | X  |    |    |    |    |    |    |    |
| Motorcycle                                  | 10  |    |    |    |    |    |    |    |    |
| Bus                                         | 11  |    |    |    |    |    |    |    |    |
| MiniBus                                     | 12  |    |    |    |    |    |    |    |    |
| Taxi                                        | 13  |    |    |    |    |    |    |    |    |
| AllTerrainVehicle                           | 14  |    |    |    |    |    |    |    |    |
| SnowmobilePost1988                          | 15  |    |    |    |    |    |    |    |    |
| PassengerVehicle                            | 16  |    |    |    |    |    |    |    |    |
| EmergencyVehicle                            | 17  |    |    |    |    |    |    |    |    |
| CommercialVehicle                           | 18  |    |    |    |    |    |    |    |    |
| Other                                       | 19  |    |    |    |    |    |    |    |    |
| AircraftCanada                              | 20  | X  |    |    |    |    |    |    |    |
| AircraftForeign                             | 21  | X  |    |    |    |    |    |    |    |
| AircraftAirframeRegisteredInCanada          | 22  |    | X  |    |    |    |    |    |    |
| AircraftAirframeNotRegisteredInCanada       | 23  |    | X  |    |    |    |    |    |    |
| Aircraft                                    | 24  |    |    |    | X  |    |    |    |    |
| AircraftDOT                                 | 25  |    |    | X  |    |    |    |    |    |
| AircraftSerial                              | 26  |    |    | X  |    |    |    |    |    |
| TrailerOrSemiTrailer                        | 27  |    |    |    |    |    |    |    |    |
| MotorHome                                   | 28  |    |    |    |    |    |    |    |    |

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
