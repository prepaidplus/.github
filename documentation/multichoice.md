# PWS MultiChoice DSTV Use Cases and Workflows

PWS Multichoice DSTV use cases are used to describe the functionality exposed by the PWS Multichoice DSTV API. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers  
- Vendors  
- PWS client (PrepaidPlus Server)  
- MultiChoice Server (Service Provider’s Server)  

#### Download Postman Collection

You can download the Postman collection for this use case <a href="/assets/dstvPostmanCollectionPostmanCollection.json" download>here</a>. <!-- Replace with the actual path to your Postman collection file -->

### Use case actors, responsibilities and collaborators

Table  describes use case actors, their responsibilities and collaborators.

**Table: Roles, responsibilities and collaborators**

| Use case actor | Responsibilities | Collaborators |
|----------------|------------------|---------------|
| Customer | - Initiates customer use cases. <br> - Provides transaction information. <br> - Tenders payment. <br> - Receives payment receipt. | - Vending Operator  |
| Vending Operator  | - Verifies customer information. <br> - Submits vending requests on behalf of customer. <br> - Performs operator specific tasks. <br> - Hands payment receipts to customers. <br> - Handles customer queries. | - PrepaidPlus Server |
| PrepaidPlus Server | - Authenticates the Multichoice Payment Server. <br> - Compiles and sends MultiChoice DSTV request messages to Multichoice payment server. <br> - Receives and formats MultiChoice DSTV response messages from the Multichoice payment server. | - Multichoice DSTV Server |
| Multichoice DSTV Payment Server | - Authenticates the MultiChoice DSTV clients. <br> - Complies with an appropriate MultiChoice DSTV response message based on its application business logic. <br> - Responds with a fault response message, if required. | - MultiChoice DSTV clients |

### Prepaid Multichoice Methods
**Table: Supported DSTV Methods**

| Operation                | Description |
|--------------------------|-------------|
| SmartcardConfirmation Request | Verifies the smartcard number and confirms if it is valid and active. It also checks the subscription due date and amount. The SmartcardConfirmation returns the smartcard details, including the registered owner and amount due. |
| CreditVend Request       | Confirms the purchase of a DSTV subscription. It can be used to recharge a DSTV account by providing the smartcard number and payment details. The method returns a confirmation of the subscription payment. |
| LastResponse Request     | This method is called subsequent to an ongoing CreditVend Request network timeout/connection failure or an exception. Its purpose is to check if a payment had been successfully made prior to abandoning the transaction. In an event that the failed CreditVend Request had resulted in a successful payment, the payment receipt is retrieved and returned for confirmation, otherwise the payment is abandoned. |

<br>

## Multichoice DSTV Smartcard Confirmation Use Case

#### Multichoice DSTV Smartcard Confirmation Description

The DSTV Smartcard Confirmation is used to confirm that the smartcard is valid and active, as well as confirming the subscription due date and amount.

#### DSTV Smartcard Confirmation Desired Outcome

The Vendor wants to confirm whether a payment can be raised successfully for a particular smartcard. The PrepaidPlus and Multichoice DSTV Payment servers perform all required validations and return confirmation of the smart card. These details will include smart card registered owner, and amount due. The PrepaidPlus server returns a positive, or failure response to the Vendor.

#### DSTV Smartcard Confirmation Preconditions

The smartcard number shall be supplied.

#### DSTV Smartcard Confirmation Participants

- The customer.
- The Vendor.
- PrepaidPlus Server.
- Multichoice DSTV payment server.

#### DSTV Smartcard Confirmation Key Business Rules

- Smartcard exists and is active.
- Smartcard details match to ones the customer would like to transact for.
- Normal Vending related exception messages are implemented as they would be for standard vending operation. (i.e Validity of Smartcard number for payment)

#### DSTV Smartcard Confirmation Workflow

The workflow for the DSTV Smartcard Confirmation process is as presented below .

**Table  DSTV Smartcard Confirmation Process Workflow**

| Step No | Client | PrepaidPlus Server |
|---------|--------|--------------------|
| 1 | The cashier selects the DSTV Payments option, and submits the customer’s smartcard number. The  POS will provide the following in addition to the information provided by the cashier; TerminalID, OperatorId, and ClientSaleID. | |
| 2 | | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. If the API key is successfully validated, PrepaidPlus sends a request to Multichoice DSTV payment server with the customer smartcard number for validation. |
| 3 | Where  POS receives a successful response from PrepaidPlus, the names, amount owed and the amount the customer would like to pay towards their account are then confirmed with the end customer. This response will have an entry box for the amount to be paid. | |

#### DSTV Smartcard Confirmation Happy Path

The DSTV Smartcard Confirmation Vend use case sequence diagram is illustrated below.

![dstvSmartcardHappyPath](/assets/dstvSmartConfirmationHappyPath.png)

<br>

###  DSTV Confirm Smartcard Methods

The ConfirmSmartcard Request is used to verify that the following are in place before accepting payment towards smartcard linked DSTV:

- The smartcard number is valid and is the correct one the buyer intends to make payment for, and also that the account is active.

The ConfirmSmartcard request either returns the customer details along with amounts owed, due dates and subscribed services, or returns an indication that the transaction cannot proceed, as well as an explanation of the cause of the failure.

**Send a ConfirmSmartcard Call Request**

Send a ConfirmSmartcard request to one of the following endpoints:

- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/trialvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/trialvend)

The ConfirmSmartcard call requires the following fields:

**Table: Confirm Smartcard call requirements**

| Argument        | Type   | Description                                                                                                      |
|-----------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey          | string | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password        | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId    | string | **Mandatory:** This is a unique client supplied reference number. Used to identify request. Supplied by: Caller.  |
| smartcardNumber | string | **Mandatory:** This is the customer smartcard number.                                                            |
| operatorId      | string | **Mandatory:** This is name of person/channel or id of POS making the transaction. Supplied by Caller.           |
| outletId        | string | **Mandatory:** This is the id of the store or channel the request is being made from. Supplied by Caller.        |

The Smartcards to be used:

**Table: Test Smartcard Numbers**

| SmartCard Number | Linked Account Details                                                                 |
|------------------|----------------------------------------------------------------------------------------|
| 1234567890       | Owner: JE Smith <br> Customer Number: 987654321 <br> Subscriptions: DSTV Premium <br> Due Date: 2016-06-29 <br> Amount Due: 100.00 |

**Making the ConfirmSmartcard Call**

The ConfirmSmartcard Request call is successful when you receive the following transaction details: customer name, meter number and purchase amount. If the call fails, you will receive one of the following faults/errors:

**Table: Confirm Smartcard Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |
| 102      | clientSaleId missing     | Missing a mandatory field - provider                                        |
| 103      | outletId missing         | Missing a mandatory field - provider                                        |
| 104      | Terminal Id missing      | Terminal Id is not provided.                                                |
| 105      | operatorId missing       | Missing a mandatory field - provider                                        |
| 110      | smartcardNumber missing  | Smartcard Number is not provided.                                           |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: MultiChoice Botswana – Etc) |
| 204      | Disabled Outlet          | The provided OutletId is currently disabled.                                |
| 205      | Invalid Outlet Id        | The provided OutletId was not found.                                        |

The above errors are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid.

**ConfirmSmartcard Response**

**Table: Confirm Smartcard Response**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| clientSaleId     | string | This is the client supplied reference number that uniquely identifies the transaction.                           |
| transactionAmount| number | Amount due to DSTV                                                                                               |
| customerName     | string | Smartcard holder's name                                                                                          |
| smartCard        | string | Smartcard number for which payment is being made.                                                               |
| amountDue        | number | Amount due to DSTV.                                                                                              |
| dueDate          | string | The DSTV payment due date                                                                                        |
| lookupId         | string | This is an id generated by PrepaidPlus to identify the customer query, it must be supplied in the payment request.|
| response         | string | Status.                                                                                                          |
| operatorId       | string | This is name of person/channel or id of POS making the transaction.                                              |
| outletId         | string | This is id of the store or channel the request is being made from.                                               |

**ConfirmSmartcard Vend Examples**

The example request and response demonstrate a ConfirmSmartcard Request call and response.

**Example Request**

This example uses the following fields and values:

````javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}");//Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
    "clientSaleId": "1234567890",//Replace with the actual clientSaleId
    "smartcardNumber": "1234567890",//Replace with the actual smartcardNumber
    "operatorId": "Web",//Replace with the actual operatorId
    "outletId": "{{ outlet-Id }}", //Replace with the actual outletId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/trialvend", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
````

**Example Response**

````javascript
{
    "code": "0",
    "clientSaleId": "1639066584",
    "transactionAmount": 100,
    "customerName": "JE Smith",
    "smartCard": "1234567890",
    "amountDue": 100,
    "dueDate": "2016-06-29",
    "lookupId": "278790",
    "response": "Successful"
}
````

**Example Fault**


````javascript
{
    "code": "06",
    "response": "Failure",
    "message": "Unexpected Error",
    "description": "An unexpected exception has occurred..",
    "transactionId": null,
    "clientSaleId": "1234567890",
    "status": 500
}
````
<br>

## Multichoice DSTV Smartcard Payment Use Case

#### DSTV Smartcard Payment Description

The Multichoice DSTV Smartcard Payment use case is used to pay outstanding DSTV subscription or pay additional monies into a selected account.

#### DSTV Smartcard Payment Desired Outcome

The customer pays for and has his smartcard linked DSTV account credited. In the event the account was suspended due to non-payment, service is restored.

#### DSTV Smartcard Payment Vend Preconditions

- The smartcard number shall be supplied.
- The desired payment amount shall be supplied.

#### DSTV Smartcard Payment Participants

- The customer
- The Vendor
- PrepaidPlus Server
- Multichoice DSTV payment server

#### DSTV Smartcard Payment Workflow

The workflow for the DSTV Smartcard Payment process is as presented below.

**Table : DSTV Smartcard Payment Workflow**

| Step No | Client | PrepaidPlus Server |
|---------|--------|--------------------|
| 1 | The cashier collects payment from the customer, and then captures the amount on the POS and submits the transaction details. The payment transaction details are transmitted to PrepaidPlus for processing. NB: Once  POS confirms the payment with PrepaidPlus, the sale cannot be rolled back. | |
| 2 | | PrepaidPlus receives confirmation of the payment from  POS. PrepaidPlus validates the merchant API Key and checks the available balance in the Merchant account (i.e. within PrepaidPlus system) to ensure there is sufficient balance to meet the amount requested for purchase. If the balance is insufficient, an error message is returned. If the above checks are positive, PrepaidPlus sends the payment request to the Multichoice DSTV payment server, and receives a confirmation once the payment is successfully processed. |
| 3 | | PrepaidPlus sends the customer payment details with a reference number from Multichoice DSTV to  POS. |
| 4 | POS prints the payment receipt details. | |

<br>

The DSTV Smartcard Payment use case sequence diagram is illustrated below.

**Fig : Smartcard Payment Happy Path**

![Smartcard Payment Happy Pathh](/assets/smartCardpaymentHappyPath.png)

<br>

### Subscription Payment Methods

Carries out the actual payment based on the positive outcome of the ConfirmSmartcard. The method returns a payment confirmation receipt.

**Send a SmartcardPayment Request**

Send a SmartcardPayment request to one of the following endpoints:

- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/creditvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/creditvend)

The SmartcardPayment Request requires the following fields:

**Table : SmartcardPayment Request requirements**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey           | string | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password         | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId     | string | **Mandatory:** This is the client supplied reference number that uniquely identifies the transaction.            |
| smartcardNumber  | string | **Mandatory:** This is the customer smartcard number.                                                            |
| lookupId         | string | **Mandatory:** This is an id generated by a prior confirmsmartcard call. It identifies the confirmsmartcard request the payment is being made for. |
| transactionAmount| string | **Mandatory:** This is the amount the customer is paying towards their account.                                  |
| outletId         | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |
| operatorId       | string | **Mandatory:** This is a unique Identifier of the person carrying out customer request.                          |

**Making the SmartcardPayment Request**

The SmartcardPayment call is successful when you receive a response that has the customer’s payment confirmation receipt. If the call fails, you will receive one of the following errors:

**Table : SmartcardPayment Request Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |
| 102      | clientSaleId missing     | Missing a mandatory field - provider                                        |
| 103      | outletId missing         | Missing a mandatory field - provider                                        |
| 104      | Terminal Id missing      | Terminal Id is not provided.                                                |
| 105      | operatorId missing       | Missing a mandatory field - provider                                        |
| 107      | transactionAmount missing| Transaction purchase amount is not provided.                                |
| 110      | smartcardNumber missing  | Smartcard Number is not provided.                                           |
| 111      | lookupId missing         | lookupId is not provided.                                                   |
| 202      | Insufficient Balance     | Merchant balance is less than sale amount.                                  |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: MultiChoice Botswana – Etc) |
| 204      | Disabled Outlet          | The provided OutletId is currently disabled.                                |
| 205      | Invalid Outlet Id        | The provided OutletId was not found.                                        |
| 206      | Minimum/Maximum amount error | Amount requested is either less or more than the allowed amount.           |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**SmartcardPayment Response Parameters**

**Table: SmartcardPayment Response**

| Argument             | Type   | Description                                                                                                      |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code                 | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| transaction_reference| int    | This is the DSTV payment Reference.                                                                              |
| customer_name        | string | Customer registered name                                                                                        |
| customer_surname     | string | Customer registered surname                                                                                     |
| customer_number      | int    | Customer number                                                                                                  |
| smartcard_number     | string | This is the customer smartcard number.                                                                          |
| clientSaleId         | string | This is the client supplied reference number that uniquely identifies the transaction. Needed for any enquiries with PrepaidPlus |
| amount               | string | Amount paid towards the DSTV account                                                                             |
| subscribed_services  | string | List of services customers has subscribed for                                                                   |
| provider             | string | Name of the service provider (MultiChoice Botswana)                                                             |
| transactionId        | string | Id of the transaction on PrepaidPlus system.                                                                    |
| transactionDateTime  | string | Date and time the transaction happened                                                                           |
| paymentDate          | string | Date and time the payment was made with DSTV.                                                                    |

**SmartcardPayment Request Examples**

The example request and response demonstrate a SmartcardPayment Request call and response.

**Example Request**

This example uses the following fields and values: Note: lookupId comes from prior confirm smartcard call.

````Javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}");// Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "clientSaleId": "1659008565",// Replace with the actual clientSaleId
  "smartcardNumber": "1234567890", // Replace with the actual smartcardNumber
  "operatorId": "Web",// Replace with the actual operatorId
  "outletId": "{{ outlet-Id }}",// Replace with the actual outletId
  "transactionAmount": 10,// Replace with the actual transactionAmount
  "lookupId": "9541224"// Replace with the actual lookupId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/creditvend", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
````

**Example Response**

````javascript
{
    "code": "0",
    "transaction_reference": 101269,
    "confirmation_number": "245877",
    "customer_name": "JE",
    "customer_surname": "Smith",
    "customer_number": 98765432,
    "smartcard_number": "1234567890",
    "amount": "100.00",
    "subscribed_services": [
    "DStv Compact Plus Bouquet IS20",
    "DStv HD Compact Plus Bouquet IS20"
    ],
    "transactionId": "8aTRULd0Z215tHw8ybKR",
    "clientSaleId": "1639103549",
    "response": "Successful",
    "provider": "MultiChoice Botswana",
    "transactionDateTime": "2021-12-10T02:32:41.089Z",
    "paymentDate": "2021-12-10T02:32:41.089Z"
}
````

**Example Fault**

````javascript
{
   "code": "06",
   "response": "Failure",
   "description": "An unexpected exception has occurred..",
   "transactionId": null,
   "clientSaleId": "1639103549",
   "smartcardNumber": "1234567890",
   "amount": 100,
   "status": 500
}
````

<br>

## Multichoice Last Response Use Case

The Vendor aims to confirm the status of a potentially successful payment after a failed or incomplete Smartcard Payment Request. The PrepaidPlus server checks whether the transaction was processed, retrieves the payment receipt (if successful), and returns it for final confirmation or abandonment.

#### Dstv Last Response Preconditions

A Smartcard Payment Request was initiated but failed due to timeout or network failure.
The clientSaleId and outletId from the initial payment attempt must be available.

#### Dstv Last Response Participants

- Customer
- Vendor
- PrepaidPlus Server
- Multichoice DSTV Payment Server

#### Dstv Last Response Key Business Rules

The API key and credentials must be valid and authorized.
The clientSaleId and outletId provided must correspond to the previous payment attempt.
If a payment was made successfully, the transaction details should be returned for receipt printing.

#### Dstv Last Response Workflow

The cashier submits a LastResponse Request with the clientSaleId and outletId from the failed payment attempt.
The PrepaidPlus server validates the API key and checks for the transaction’s status.
If a successful payment is found, PrepaidPlus returns the payment confirmation details.
If no payment was processed, PrepaidPlus returns an error indicating that the transaction could not be found.

#### Dstv Last Response Happy Path

The request successfully returns payment confirmation, including the customer’s name, amount paid, and transaction details. The cashier prints the receipt and the transaction is closed.
 
 <br>

### Multichoice DSTV LastResponse Methods

This method is called subsequent to an ongoing Smartcard Payment Request network timeout/connection failure or an exception. Its purpose is to check if a payment had been successfully made prior to abandoning the payment. In an event that the failed Smartcard Payment Request had resulted in a successful payment, the payment receipt is retrieved and returned for printing, otherwise the payment is abandoned.

**Send a LastResponse Request Call**

Send a LastResponse request to one of the following endpoints:

- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/advice](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/advice)

The LastResponse call requires the following fields:

**Table: Last Response call requirements**

| Argument     | Type   | Description                                                                                                      |
|--------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey       | string | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password     | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId | string | **Mandatory:** This is a customer supplied Id corresponding to a DSTV payment they would like to check status of. Obtained from DSTV payment call. |
| outletId     | string | **Mandatory:** This is the unique Identifier of the store or sale channel the purchase is being made from.       |

**Making the LastResponse Request call**

The LastResponse Request call is successful when you receive the payment confirmation receipt, or confirmation that no payment transaction had been processed. The implementing Vendor will determine the elapsed time interval to call the LastResponse. If the call fails, you will receive one of the following faults/errors:

**Table: Last Response Request Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |
| 102      | ClientSaleId is missing  | clientSaleId is not provided.                                               |
| 103      | OutletId is missing      | outletId is not provided                                                    |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: Multichoice – Etc) |
| 208      | Invalid clientSaleId     | Could not find a transaction for the supplied client sale transaction Id.   |
| 209      | No last response found   | The server did not process the requested transaction. No sale was made      |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**LastResponse Response Parameters**

**Table : Last Response Response parameters**

| Argument             | Type   | Description                                                                                                      |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code                 | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| transaction_reference| int    | This is the DSTV payment Reference.                                                                              |
| customer_name        | string | Customer registered name                                                                                        |
| customer_surname     | string | Customer registered surname                                                                                     |
| customer_number      | int    | Customer number                                                                                                  |
| smartcard_number     | string | This is the customer smartcard number.                                                                          |
| clientSaleId         | string | This is the client supplied reference number that uniquely identifies the transaction. Needed for any enquiries with PrepaidPlus |
| amount               | string | Amount paid towards the DSTV account                                                                             |
| subscribed_services  | string | List of services customers has subscribed for                                                                   |
| transactionId        | string | Id of the transaction on PrepaidPlus system.                                                                    |
| transactionDateTime  | string | Date and time the transaction happened                                                                           |
| provider             | string | Name of the service provider (MultiChoice Botswana)                                                             |

**LastResponse Request Examples**

The example request and response demonstrate a LastResponse Request call and response.

````javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}");//Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "clientSaleId": "1639111159"
  "outletId": "{{ outlet-Id }}"//Replace with the actual outletId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/dstv/advice", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
````

**Example Response**

This example above makes a request for clientSaleId:12349876 which for purposes of illustration had timed out, whilst the call had resulted in a successful sale.

````javascript
{
     "code": "0",
     "amount": "100.00",
     "customer_number": 98765432,
     "smartcard_number": "1234567890",
     "transaction_reference": 101271,
     "paymentDate": "2021-12-10 06:39:37",
     "transactionDateTime": "2021-12-10 06:49:29",
     "provider": "MultiChoice Botswana",
     "customer_name": "JE",
     "customer_surname": "Smith",
     "transactionId": "K3CHY6PGLSTASYIY96VV",
     "clientSaleId": "1639111159",
     "response": "Successful"
}
````

This response would close off a successful last response payment.

**Example Fault**

````javascript
{ 
"code": "209"
"clientSaleId": "16391111599"
"response: "Failure",
"message": " Sale not found.",
"description": " Sale not found."
}
````
