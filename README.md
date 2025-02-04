# API Documentation

## INTEGRATION GUIDE
Welcome to the Reg Hub API integration documentation. This guide is designed to help you authenticate with our API using OAuth (provided by IdentityServer) so you can securely access our services. Follow the steps below to obtain your access token.

## Authentication
Prerequisites
Before you begin, ensure you have the following:

* A valid client_id and client_secret provided by Reg Hub.
* Your Reg Hub account username and password.

### Step 1: Obtain an Access Token

Prepare your request: To authenticate, you will need to send a request to the Reg Hub authentication endpoint. Your request must include the following parameters:

* URL: https://auth.uat.reghub.ca/connect/token
* Method: POST
* Headers: Content-Type: application/x-www-form-urlencoded

Body Parameters:
* grant_type: "password" (This specifies the type of grant you are requesting.)
* scope: "opened RegHubAPI" (This specifies the permissions your access token will have.)
* username: Your Reg Hub account username.
* password: Your Reg Hub account password.
* client_id: The client ID provided to you by Reg Hub.
* client_secret: The client secret provided to you by Reg Hub.

Send the request: Use your preferred HTTP client to send the POST request with the specified headers and body parameters.

### Step 2: Receive and Store Your Access Token

Upon successful authentication, you will receive a response containing the following information:

* access_token: The token you will use for authenticating your API calls.
* expires_in: 3600 (The time in seconds before the token expires. You will need to re-authenticate after this period.)
* token_type: "Bearer" (The type of the token.)
* scope: The scope of access granted, which will be "openid RegHubAPI".

Store the access_token securely, as you will need it to authenticate your calls to Reg Hub API.

### Step 3: Use the Access Token to Authenticate API Calls

To make authenticated API calls to Reg Hub, include the access_token in the Authorization header as a Bearer token:

`Authorization: Bearer YOUR_ACCESS_TOKEN`

Replace YOUR_ACCESS_TOKEN with the token you received from the authentication step.

Handling Token Expiry
Your access_token will expire in 3600 seconds (1 hour). Upon expiry, you will need to repeat the authentication process to obtain a new access token.

### That's it!

You are now ready to securely integrate with Reg Hub API using OAuth. Should you have any questions or encounter any issues during integration, please feel free to reach out to our support team for assistance.

----

## Order Process Flow

### Create Draft
CreateOrder (POST) endpoint will create an order in a "Draft" state. The response JSON payload will include an ID, a unique GUID, which will be used for all subsequent actions. It will also include validation results for this draft order. All Errors must be resolved before the order can be submitted (for more information, see Validation).

### Order Groups
Each Order belongs to an Order Group, with a unique GUID. Some Order Types will create a new Order Group upon creation while others require an existing Order Group ID for Order creation. The latter will action the latest completed Order in the group (Ex. Renewal).

### Callback
Customers can enter a Callback URL on each order. Upon completion, a JSON payload of the completed order will be posted to the callback URL. The call back proocess will not include any authorization, so please ensure that the endpoint is publicly accessible (IP whitelisting is a security option).

### Update/Delete
While an order is in a "Draft" state, PUT and PATCH endpoints are available to update any details on the Order. DELETE endpoint is also available for any orders in a "Draft" state you wish to abandon.

### Submit Order
Once all Errors have been resolved, the Submit (POST) endpoint will move the Order from "Draft" state to a "Pending" state.

### Pricing
Pricing must be set in order to submit. Service Fees will be incurred on the Order upon submission. If there is no pricing set for this Order Type - Jurisdiction combination you will receive an error and the Order will remain in "Draft" state.

### SLA
While the Order is in "Pending" state, Reg Hub will take the Order to the appropriate Registry. Time to completion will depend on the Order Type, Jurisdiction, and time of day.

### Complete Results
Once an order has been completed, the results (GET) are available in perpetuity. Order Results will depend on Order Type and Jurisdiction.

### Fees
Upon Order completion, any applicable registry or agent fees will be added to the Order. All Order Fees will be added to an invoice (for more information, see Invoicing).

### Invoicing
Per customer specification, invoices will be created either Daily, Weekly, Monthly, or Order-by-Order. Upon Order completion, Order Fees will be added to the appropriate draft invoice. At the end of the Invoicing Cycle, the invoice is closed. (Monthly Invoices closed on the last day of the month, Weekly Invoices on Sunday). At the end of each day, any closed invoices that have not been sent will be so.

### Validation Errors
Validation Errors are issues that are required to be fixed prior to submission. They are determined on an order-by-order basis, but there is a Validation Rules endpoint (get) to get a list of all possible errors for a given Order Type and Jurisdiction.

### Warnings
Validation Warnings are issues that are not required to be fixed prior to submission, but Reg Hub wants you to be aware of. They are determined on an order-by-order basis, but there is a Validation Rules endpoint (get) to get a list of all possible warning for a given Order Type and Jurisdiction.

----

## Search Specific Information

### Search Parameters
Not all fields in the searchParameters field will be used in all jurisdictions. Below is a list of what fields are available in each jurisdiction.

| Jurisdiction | ExactOnlyResults | SearchFromDate | SearchToDate |
| ------------ | ---------------- | -------------- | ------------ |
| BC | X | | |
| AB | X | | |
| SK | X | | |
| MB | X | | |
| ON | X | X | |
| QC | X | | |
| NL | X | | |
| NS | X | | |
| NB | X | | |
| PE | X | | |
| YT | X | | |
| NT | X | | |
| NU | X | | |

----

## Enumerations

#### Valid Asset Types Per Jurisdiction (Base Order Type: Lien)
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
| Other                                       | 19  |    |    |    |    |    |    |    |    |    |    |    |    |    |
| AircraftCanada                              | 20  | X  |    |    |    |    |    |    |    |    |    |    |    |    |
| AircraftForeign                             | 21  | X  |    |    |    |    |    |    |    |    |    |    |    |    |
| AircraftAirframeRegisteredInCanada          | 22  |    | X  |    |    |    |    |    |    |    |    |    |    |    |
| AircraftAirframeNotRegisteredInCanada       | 23  |    | X  |    |    |    |    |    |    |    |    |    |    |    |
| Aircraft                                    | 24  |    |    |    | X  |    |    |    |    |    |    |    |    |    |
| AircraftDOT                                 | 25  |    |    | X  |    |    |    |    |    |    |    |    |    |    |
| AircraftSerial                              | 26  |    |    | X  |    |    |    |    |    |    |    |    |    |    |
| TrailerOrSemiTrailer                        | 27  |    |    |    |    |    | X  |    |    |    |    |    |    |    |
| MotorHome                                   | 28  |    |    |    |    |    | X  |    |    |    |    |    |    |    |

#### Valid Asset Types Per Jurisdiction (Base Order Type: Repair Lien)
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

#### Valid Party Types per jurisdiction
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

Note: For dealers in QC, the validity of this provided party type depends on the form type being used.

#### Document Result Type
| Description | Value |
| ----------- | ----- |
| NoResult | 1 |
| RawDocument | 2 |
| ParsedDocument | 3 |
#### Order State
| Description | Value |
| ----------- | ----- |
| Draft | 1 |
| Pending | 2 |
| Complete | 3 |
| Invalid | 4 |

### Swagger Documentation
https://api.uat.reghub.ca/swagger/index.html
