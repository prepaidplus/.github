
# PWS Water Utilities Cooperation Use Cases and Workflows


## Use Case Actors, Responsibilities and Collaborators

Table 4.1 describes use case actors, their responsibilities and collaborators.

### Table 4.1: Roles, Responsibilities and Collaborators

| Use case actor | Responsibilities                                                                 | Collaborators                           |
|----------------|----------------------------------------------------------------------------------|-----------------------------------------|
| Customer       | - Initiates customer use cases.                                                  | - Vending Operator – (Finclude)         |
|                | - Provides transaction information.                                              |                                         |
|                | - Tenders payment.                                                               |                                         |
|                | - Receives payment receipt.                                                      |                                         |
| Vending Operator (Finclude) | - Verifies customer information.                                     | - PrepaidPlus Server                    |
|                | - Submits vending requests on behalf of customer.                                |                                         |
|                | - Performs operator specific tasks.                                              |                                         |
|                | - Hands payment receipts to customers.                                           |                                         |
|                | - Handles customer queries.                                                      |                                         |
| PrepaidPlus Server | - Authenticates the WUC Server.                                               | - Water Utilities Cooperation Server    |
|                | - Compiles and sends WUC request messages to WUC server.                         |                                         |
|                | - Receives and formats WUC response messages from the WUC server.                |                                         |
| WUC Server     | - Authenticates the WUC clients.                                                 | - WUC clients                           |
|                | - Complies with an appropriate WUC response message based on its application business logic. |                                         |
|                | - Responds with a fault response message, if required.                           |                                         |

## Water Utilities Cooperation Customer Confirmation Use Case

### Water Utilities Cooperation Customer Confirmation Description

The WUC Customer Confirmation is used to confirm that the customer number & contract numbers are valid and active, as well as confirming the due amount.

### Water Utilities Cooperation Customer Confirmation Desired Outcome

The Vendor wants to confirm whether a payment can be raised successfully for a particular customer number and contract number pair. The PrepaidPlus and WUC servers perform all required validations and return confirmation of the customer. These details will include customer’s name, plot, and amount due. The PrepaidPlus server returns a positive, or failure response to the Vendor.

### WUC Customer Confirmation Preconditions

- The customer’s phone number, customer number, and contract number shall be supplied.

### WUC Customer Confirmation Participants

- The customer
- The Vendor
- PrepaidPlus Server
- Water Utilities Cooperation server

### WUC Customer Confirmation Key Business Rules

- Customer number and contract number exist and are active.
- Customer details match to ones the customer would like to transact for.
- Normal Vending related exception messages are implemented as they would be for standard vending operation.

### WUC Customer Confirmation Workflow

The workflow for the WUC Customer Confirmation process is as presented below in Table 4.2.

### Table 4.2: WUC Customer Confirmation Process Workflow

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | The cashier selects the WUC Payments option, and submits the customer number & contract number. The Finclude POS will provide the following in addition to the information provided by the cashier; TerminalID, OperatorId, and ClientSaleID. |                                                                                    |
| 2        |                                                                        | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. If the API key is successfully validated, PrepaidPlus sends a request to the WUC server with the customer’s details for validation. |
| 3        | Where Finclude POS receives a successful response from PrepaidPlus, the names, amount owed and the amount the customer would like to pay towards their account are then confirmed with the end customer. This response will have an entry box for the amount to be paid. |                                                                                    |

## Water Utilities Cooperation Fee Confirmation Use Case

### Water Utilities Cooperation Fee Confirmation Description

The WUC Fee Confirmation is used to confirm fees that will be associated with a particular transaction depending on the payment amount.

### Water Utilities Cooperation Fee Confirmation Desired Outcome

The Vendor wants to confirm the transaction fees and the total transaction amount for the amount the customer intends to make a payment for, and should receive the transaction fee that is associated with the particular amount the customer wants to pay and the total transaction amount that the customer is to bring forth.

### WUC Fee Confirmation Preconditions

- The payment amount that the customer intends to pay their WUC bill should be provided.

### WUC Fee Confirmation Participants

- The customer
- The Vendor
- PrepaidPlus Server

### WUC Fee Confirmation Key Business Rules

- Calculate and confirm the transaction fee associated with a particular WUC payment.

### WUC Fee Confirmation Workflow

The workflow for the WUC Fee Confirmation process is as presented below in Table 4.3.

### Table 4.3: WUC Fee Confirmation Process Workflow

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | The cashier captures the amount the WUC customer intends to pay and submits. The Finclude POS will provide the following in addition to the information provided by the cashier; api key and password. |                                                                                    |
| 2        |                                                                        | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. If the API key is successfully validated, PrepaidPlus calculates and returns transaction fees that would be associated with the payment and the total transaction amount. |
| 3        | When Finclude POS receives a successful response from PrepaidPlus, and the customer acknowledges the total transaction amount required, subsequent requests may be made to complete the transaction. |                                                                                    |

## Water Utilities Cooperation Confirm Payment Use Case

### WUC Confirm Payment Description

The WUC Confirm Payment use case is used to pay outstanding WUC bills or pay additional monies into a selected account.

### WUC Confirm Payment Desired Outcome

The customer pays for and has their WUC bill account paid. In the event the account was suspended due to non-payment, service may be restored.

### WUC Confirm Payment Vend Preconditions

- The customer number and the contract numbers shall be supplied.
- The desired payment amount shall be supplied.

### WUC Confirm Payment Participants

- The customer
- The Vendor
- PrepaidPlus Server
- WUC server

### WUC Confirm Payment Workflow

The workflow for the WUC Confirm Payment process is as presented below in Table 4.4.

### Table 4.4: WUC Confirm Payment Workflow

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | The cashier collects payment from the customer, and then captures the amount on the POS and submits the transaction details. The payment transaction details are transmitted to PrepaidPlus for processing. NB: Once Finclude POS confirms the payment with PrepaidPlus, the sale cannot be rolled back. |                                                                                    |
| 2        |                                                                        | PrepaidPlus receives confirmation of the payment from Finclude POS. PrepaidPlus validates the merchant API Key and checks the available balance in the Merchant account (i.e. within PrepaidPlus system) to ensure there is sufficient balance to meet the amount requested for purchase. If the balance is insufficient, an error message is returned. If the above checks are positive, PrepaidPlus sends the payment request to the WUC server, and receives a confirmation once the payment is successfully processed and sends back a response to Finclude POS. |
| 3        |                                                                        | Finclude POS prints the payment receipt details. |



## METHODS
### CONFIRM CUSTOMER

The Confirm Customer Request is used to verify that the following are in place before accepting payment towards a contract number linked to WUC:
- The customer number & contract number are valid and are the correct ones the buyer intends to make payment for, and also that the account is active.

The Confirm Customer request either returns the customer details along with amounts owed and plot number, or returns an indication that the transaction cannot proceed, as well as an explanation of the cause of the failure.

#### Send a Confirm Customer Call Request

Send a Confirm Customer request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/trialvend

The Confirm Customer call requires the following fields:

| Argument       | Type   | Description                                                                                                      |
|----------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey         | string | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password       | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId   | string | **Mandatory:** This is a unique client supplied reference number. Used to identify request. Supplied by: Caller.  |
| contractNumber | string | **Mandatory:** This is the customer’s WUC contract number.                                                       |
| customerNumber | string | **Mandatory:** This is the customer’s WUC customer number.                                                       |
| cellPhone      | string | **Mandatory:** This is the customer phone number.                                                                |
| operatorId     | string | **Mandatory:** This is the name of person/channel or id of POS making the transaction. Supplied by Caller.       |
| outletId       | string | **Mandatory:** This is the id of the store or channel the request is being made from. Supplied by Caller.        |



The contractNumber & customerNumber to be used:

### Table 6.16: Test Numbers

| Customer Number | Contract Number | Account Details |
|-----------------|-----------------|-----------------|
| 161362          | 186439          | Firstname: GOBA Surname: REETSANG Plot: 8557 |
| 184960          | 215798          | Firstname: FRED Surname: KOBUE Plot: 01662 |
| 492686          | 659319          | Firstname: MOTINGWA Surname: GODFREY Plot: 1-07-00342 |

### Making the Confirm Customer Call

The Confirm Customer Request call is successful when you receive the following details: customer name, plot number, and due amount.

If the call fails, you will receive one of the following faults/errors:

### Table 6.17: Confirm Customer Errors

| Error No | Short Message        | Description                                      |
|----------|----------------------|--------------------------------------------------|
| 01       | API key missing       | API Key not provided.                           |
| 02       | Invalid API key       | Unable to authenticate supplied API key.        |
| 03       | Invalid API Password  | Unable to authenticate supplied password.       |
| 04       | Disabled API key      | The provided API Key is disabled                |
| 06       | Unexpected Error      | An unexpected exception has occurred.           |
| 11       | Authorisation failed  | Authorisation Header is not provided            |
| 102      | clientSaleId missing  | Missing a mandatory field - provider            |
| 103      | outletId missing      | Missing a mandatory field - provider            |
| 105      | operatorId missing    | Missing a mandatory field - provider            |
| 110      | Contract number missing | Contract Number is not provided.              |
|          | Customer number missing | Customer number is not provided.              |
| 203      | Vendor Error          | Pass-on Business Error returned by Vendor.      |
| 204      | Disabled Outlet       | The provided OutletId is currently disabled.    |
| 205      | Invalid Outlet Id     | The provided OutletId was not found.            |

The above errors are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid.

### Confirm Customer Response

### Table 6.18: Confirm Customer Response

| Argument        | Type    | Description                                      |
|-----------------|---------|--------------------------------------------------|
| contractNumber  | string  | The customer’s WUC contract number               |
| customerNumber  | string  | The customer’s WUC customer number               |
| firstName       | string  | WUC customer’s first name                        |
| surname         | string  | WUC customer’s last name                         |
| amountDue       | number  | Amount due to WUC.                               |
| meterPlot       | string  | WUC customer’s plot number                       |
| cellPhone       | string  | The cellphone number supplied by the customer    |
| verified        | boolean | The status of whether the customer’s details have been verified with WUC or not |

### Confirm Customer Examples

The example request and response demonstrate a Confirm Customer Request call and response. 

### Example Request

This example uses the following fields and values:

![Example Request Screenshot](/assets/example%20request%20wuc.png)

### Example Response

Here is an example of a JSON response you might receive:

![Example Response Screenshot](/assets/wuc%20response.png)

### 7.5.2 Confirm Fees

The Confirm Fees Request is used to calculate & verify the following:
- The transaction fees and the total transaction amount that would be associated with the transaction dependent on the payment amount that the customer wants to pay.

The Confirm Fee request either returns the transaction fee and the transaction amount and/or returns an indication that the transaction cannot proceed, as well as an explanation of the cause of the failure.

#### Send a Confirm Fee Call Request
Send a Confirm Fees request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/calculateFees](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/calculateFees)

The Confirm Fees call requires the following fields:

**Table 6.15: Confirm Fees call requirements**

| Argument      | Type    | Description                                                                                                      |
|---------------|---------|------------------------------------------------------------------------------------------------------------------|
| apiKey        | string  | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password      | string  | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| paymentAmount | number  | **Mandatory:** This is the amount the customer intends to pay towards their WUC bill.                            |

#### Making the Confirm Fees Call
The Confirm Fees Request call is successful when you receive the following details: payment amount, transaction fee, transaction amount.
If the call fails, you will receive one of the following faults/errors:

**Table 6.17: Confirm Fee Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |
| 110      | Invalid payment amount   | The provided payment amount value is in an invalid format.                  |
|          | PaymentAmount missing    | Payment amount not provided                                                 |

The above errors are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid.

#### Confirm Fees Response

**Table 6.18: Confirm Fees Response**

| Argument         | Type    | Description                                                                 |
|------------------|---------|-----------------------------------------------------------------------------|
| paymentAmount    | number  | The amount the customer intends to pay towards their bill                    |
| transactionFee   | number  | The fee that would be associated with the payment                            |
| transactionAmount| number  | The total transaction amount                                                 |

#### Confirm Fees Examples
The example request and response demonstrate a Confirm Fees Request call and response.

**Example Request**


![Example confirm fees Screenshot](/assets/confirm%20fees%20example%20.png)

**Example Response**

![Example confirm fees response Screenshot](/assets/confirm%20fees%20response%20.png)

### 7.5.3 Confirm Payment

The Confirm Payment Request is to be made after confirming the customer and confirming the fees to be associated with the transaction. The confirm payment request is used to process and confirm the WUC payment that the customer intended to make. The Confirm Payment request either returns the transaction fee and the transaction amount and/or returns an indication that the transaction cannot proceed, as well as an explanation of the cause of the failure.

#### Send a Confirm Payment Call Request
Send a Confirm Payment request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/creditvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/creditvend)

The Confirm Payment call requires the following fields:

**Table 6.15: Confirm Payment call requirements**

| Argument        | Type    | Description                                                                                                      |
|-----------------|---------|------------------------------------------------------------------------------------------------------------------|
| apiKey          | string  | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password        | string  | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| paymentAmount   | number  | **Mandatory:** This is the amount the customer intends to pay towards their WUC bill.                            |
| contractNumber  | string  | **Mandatory:** This is the customer’s WUC contract number.                                                       |
| customerNumber  | string  | **Mandatory:** This is the customer’s WUC customer number.                                                       |
| cellPhone       | string  | **Mandatory:** This is the customer phone number.                                                                |
| operatorId      | string  | **Mandatory:** This is the name of person/channel or id of POS making the transaction. Supplied by Caller.       |
| clientSaleId    | string  | **Mandatory:** This is a unique client supplied reference number. Used to identify request. Supplied by Caller.  |
| outletId        | string  | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |
| terminalId      | string  | **Mandatory:** This is an identifier for the device or channel carrying out the transaction.                     |

#### Making the Confirm Payment Call
The Confirm Payment Request call is successful when you receive the following details: payment amount, transaction fee, transaction amount, transactionId, and response element with a successful value. If the call fails, you will receive one of the following faults/errors:

**Table 6.17: Confirm Payment Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |
| 102      | clientSaleId is missing  | clientSaleId is not provided.                                               |
| 110      | Contract number missing  | Contract Number is not provided.                                            |
| 110      | Customer number missing  | Customer number is not provided.                                            |
| 110      | Invalid payment amount   | The provided payment amount value is in an invalid format.                  |
|          | PaymentAmount missing    | Payment amount not provided                                                 |

The above errors are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid.

#### Confirm Payment Response

**Table 6.18: Confirm Payment Response**

| Argument            | Type    | Description                                                                 |
|---------------------|---------|-----------------------------------------------------------------------------|
| paymentAmount       | number  | The amount the customer intends to pay towards their bill                    |
| transactionFee      | number  | The fee that would be associated with the payment                            |
| transactionAmount   | number  | The total transaction amount                                                 |
| customerNumber      | string  | The customer's WUC customer number                                           |
| contractNumber      | string  | The customer’s WUC contract number                                           |
| cellPhone           | string  | The customer’s phone number                                                  |
| customerName        | string  | The customer’s full names                                                    |
| customerPlot        | string  | The customer’s plot number                                                   |
| code                | string  | The transaction’s response code                                              |
| transactionId       | string  | The transaction id used to identify the transaction                          |
| transactionDateTime | string  | The date stamp the transaction was recorded                                  |
| clientSaleId        | string  | The transaction clientSaleId                                                 |
| provider            | string  | Water Utilities Corporation                                                  |

#### Confirm Payment Examples
The example request and response demonstrate a Confirm Payment Request call and response.

**Example Request**
![Example Confirm Payments Screenshot](/assets/confirm%20payments.png)

**Example Response**
![Example Confirm Payments Screenshot](/assets/confirm%20payments%20response%20.png)


## 7.5.4 WUC LastResponse

This method is called subsequent to an ongoing Payment Request network timeout/connection failure or an exception. Its purpose is to check if a payment had been successfully made prior to abandoning the payment. In an event that the failed Payment Request had resulted in a successful payment, the payment receipt is retrieved and returned for printing, otherwise the payment is abandoned. The LastResponse request either returns the transaction receipt details, or an indication that the requested transaction was processed and/or returns an indication that the transaction cannot proceed, as well as an explanation of the cause of the failure.

#### Send a LastResponse Call Request
Send a LastResponse request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/advice](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/wuc/advice)

The LastResponse call requires the following fields:

**Table 6.15: LastResponse call requirements**

| Argument      | Type    | Description                                                                                                      |
|---------------|---------|------------------------------------------------------------------------------------------------------------------|
| apiKey        | string  | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password      | string  | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId  | string  | **Mandatory:** This is the clientSaleId of the request whose status is in question.                              |

#### Making the LastResponse Call
The LastResponse Request call is successful when you receive the transaction receipt details or a response stating that the transaction in question was not processed. If the call fails, you will receive one of the following faults/errors:

**Table 6.17: Confirm Fee Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |

The above errors are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid.

#### LastResponse Examples
The example request and response demonstrate a LastResponse Request call and response.

![Example Last Response Call Screenshot](/assets/last%20response%20call.png)

**Example Request**

This example uses the following fields and values:
![Example Last Response Call Screenshot](/assets/last%20response%20call%20.png)