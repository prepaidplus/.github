## PWS Prepaid Airtime Use Cases and Workflows

PWS Prepaid Airtime use cases are used to describe the functionality exposed by the Prepaid Airtime API. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers  
- Vendors  
- PWS client (PrepaidPlus Server)  
- Airtime Vending Server (Service Provider’s Server)  

#### Download Postman Collection

You can download the Postman collection for this use case <a href="/assets/airtimePostmanCollection.json" download>here</a>. <!-- Replace with the actual path to your Postman collection file -->

####  Use case actors, responsibilities and collaborators

The table describes use case actors, their responsibilities and collaborators.

**Table: Roles, responsibilities and collaborators**

| Use case actor   | Responsibilities                                                                 | Collaborators          |
|------------------|----------------------------------------------------------------------------------|------------------------|
| Customer         | - Initiates customer use cases. <br> - Tenders payment. <br> - Receives an airtime recharge voucher. | - Vending Operator     |
| Vending Operator | - Submits vending requests on behalf of customer. <br> - Performs operator specific tasks. <br> - Delivers airtime recharge voucher to customers. <br> - Handles customer queries. | - PrepaidPlus Server   |
| PrepaidPlus Server | - Authenticates the Prepaid Airtime Server. <br> - Compiles and sends Prepaid Airtime request messages to the Prepaid Airtime server. <br> - Receives and formats Prepaid Airtime response messages from the Prepaid Airtime server. | - Prepaid Airtime Server |
| Prepaid Airtime Server | - Authenticates the Prepaid Airtime clients. <br> - Complies with an appropriate Prepaid Airtime response message based on its application business logic. <br> - Responds with a fault response message, if required. | - Prepaid Airtime clients |

###  Prepaid Airtime Credit Request Use Case

#####  Prepaid Airtime Credit Request Description
The Prepaid Airtime Credit Request use case is used to purchase a pin recharge of a defined amount for a specific mobile network operator.

#####  Prepaid Airtime Credit Request Desired Outcome
The customer specifies the details of the pin recharge they require and pays for it (mobile network operator and amount).

#####  Prepaid Airtime Credit Request Preconditions
- The mobile network operator shall be supplied.
- The amount shall be supplied.

#####  Prepaid Airtime Credit Request Participants
- The customer
- The Vendor
- PrepaidPlus Server
- Prepaid Airtime server

#####  Prepaid Airtime Credit Request Workflow
The workflow for the prepaid airtime credit request process is as presented below.

**Table: Prepaid Airtime Credit Request Workflow**

| Step No | Client | PrepaidPlus Server |
|---------|--------|--------------------|
| 1 | The cashier collects payment from the customer, and selects the desired Mobile Network Operator for which a recharge voucher is required. Thereafter, the cashier selects and submits the required recharge value. <br> NB: Once the request is confirmed with PrepaidPlus, the sale cannot be rolled back. | |
| 2 | | PrepaidPlus receives confirmation of the prepaid airtime credit request from the merchant. PrepaidPlus validates the merchant API Key and available balance. |
| 3 | | The transaction is processed on the prepaid airtime server, and a prepaid airtime PIN recharge voucher is dispatched once the transaction is successfully processed. |
| 4 | The merchant receives and delivers the prepaid airtime recharge voucher details to the customer. | |

#####  Prepaid Airtime Credit Request Happy Path
The prepaid airtime credit request use case sequence diagram is illustrated below.

**Fig: Prepaid Airtime Credit Request Happy Path**

![Credit Vend Happy Path](/assets/airtimeHappypath.png)



### Prepaid Airtime Methods

####  Airtime Credit Request

The method returns a prepaid recharge voucher for the selected provider.

**Send an Airtime Request**

Send an Airtime Credit request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-voucher/creditvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-voucher/creditvend)

The AirtimeCreditVend Request requires the following fields:

**Table: Airtime Credit Vend Request requirements**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey           | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header as part of Basic Authentication scheme. |
| password         | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| outletId         | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |
| operatorId       | string | **Mandatory:** This is a unique Identifier of the person carrying out customer request.                          |
| terminalId       | string | **Mandatory:** This is an identifier for the device or channel carrying out the transaction.                     |
| clientSaleId     | string | **Mandatory:** This is the Sale/transaction Id that uniquely identifies sale from client side and is shared with PrepaidPlus for reference. |
| provider         | string | **Mandatory:** This is the airtime provider. It’s a finite set Mascom, Orange, beMobile. Provider must be one of above with no padded empty characters. |
| denomination     | int    | **Mandatory:** This is the airtime credit purchase amount. Refer to Table below for denominations on offer.      |

**Table: Airtime Denominations – Testing Server**

| Provider | Denomination |
|----------|--------------|
| Orange   | 10           |

**Making the AirtimeCredit Request**

The AirtimeCredit call is successful when you receive a response with an airtime credit voucher for the selected provider and denomination. If the call fails, you will receive one of the following errors:

**Table : AirtimeCredit Request Errors**

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
| 108      | Provider missing         | Missing a mandatory field - provider                                        |
| 109      | denomination missing     | Missing a mandatory field – Transaction Amount                              |
| 202      | Insufficient Balance     | Merchant balance is less than sale amount.                                  |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: Mascom, Orange – Etc) |
| 211      | Airtime Vending Denied   | Merchant not setup to sell Airtime.                                         |
| 212      | No Vouchers to Sell      | PrepaidPlus has run out of vouchers of the selected denomination.           |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**AirtimeCredit Response Parameters**

**Table: AirtimeCredit Response**

| Argument             | Type   | Description                                                                                                      |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code                 | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| airtimeID            | string | **Mandatory:** Unique product identifier, generated by server                                                    |
| provider             | int    | **Mandatory:** Airtime Provider, possible values – [Mascom, Orange, beMobile]. Case is important                 |
| response             | element| **Mandatory:** This is the response from the sale, whether successful or failed                                   |
| transactionId        | string | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale. Supplied by PrepaidPlus.    |
| transactionAmount    | element| **Mandatory:** This is the denomination amount for the transaction.                                              |
| transactionDateTime  | string | **Mandatory:** Server time the transaction happened                                                              |
| clientSaleId         | string | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction. Supplied by Merchant. |
| voucherSerialNumber  | string | **Mandatory:** Voucher serial number as supplied by Provider – not guaranteed to be unique                       |
| voucherPinNumber     | string | Voucher Pin Number as supplied by Provider                                                                       |

**AirtimeCredit Request Examples**

The example request and response demonstrate an Airtime Credit Request call and response. The example query demonstrates the use of the 'Authorization' header which uses Basic Authentication. The string passed is  (Base 64 encoded) APIkey as user and merchant password.

**Example Request**

This example uses the following fields and values:

````javascript

var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic{{ base64string }}");//Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "clientSaleId": "1639124683",//Replace with your actual clientSaleId
  "provider": "Orange",//Replace with your actual provider
  "denomination": 10,//Replace with your actual denomination
  "terminalId": "Web",//Replace with your actual terminalId
  "outletId": "{{ outlet-Id }}",//Replace with your actual outletId
  "operatorId": "fino"//Replace with your actual operatorId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-voucher/creditvend", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));

````
**Example Response**

````javascript 
{
"code": "0",
    	"airtimeID": 101309,
    	"provider": "ORANGE",
    	"response": "Successful",
    	"transactionAmount": 10,
    	"transactionDateTime": "2021-12-10T09:47:48.325Z",
    	"transactionId": "vU0FyUW4kSGVIwBKTsce",
    	"voucherSerialNumber": "1000227",
    	"voucherPinNumber": " 9917 28",
    	"product": "Orange P10",
    	"clientSaleId": "1639124683"
}
````
**Example Fault**
````javascript

{
"code": "211"
"clientSaleId": "16391111599"
"response": "Failure",
"message": " Airtime Vending Denied ",
"description": "Merchant not setup to sell Airtime."
}

````
####  Airtime LastResponse

This method is called subsequent to an ongoing Airtime Purchase Request network timeout/connection failure or an exception. Its purpose is to check if a payment had been successfully made prior to abandoning the payment. In an event that the failed Airtime Purchase Request had resulted in a successful payment, the payment receipt is retrieved and returned for printing, otherwise the payment is abandoned.

**Send a LastResponse Request Call**

Send a LastResponse request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/advice](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/advice)

The LastResponse call requires the following fields:

**Table: Last Response call requirements**

| Argument     | Type   | Description                                                                                                      |
|--------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey       | string | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password     | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId | string | **Mandatory:** This is a customer supplied Id corresponding to an Airtime purchase they would like to check status of. Obtained from Airtime purchase call. |
| outletId     | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |

**Making the LastResponse Request**

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
| 102      | clientSaleId is missing  | clientSaleId is not provided.                                               |
| 103      | OutletId is missing      | outletId is not provided                                                    |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: Mascom, Orange – Etc) |
| 208      | Invalid clientSaleId     | Could not find a transaction for the supplied client sale transaction Id.   |
| 209      | Sale not found           | The server did not process the requested transaction. No sale was made      |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**LastResponse Response Parameters**

**Table: Last Response Response parameters**

| Argument             | Type   | Description                                                                                                      |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code                 | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| airtimeID            | string | **Mandatory:** Unique product identifier, generated by server                                                    |
| provider             | int    | **Mandatory:** Airtime Provider, possible values – [Mascom, Orange, beMobile]. Case is important                 |
| response             | element| **Mandatory:** This is the response from the sale, whether successful or failed                                   |
| transactionId        | string | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale. Supplied by PrepaidPlus.    |
| transactionAmount    | element| **Mandatory:** This is the denomination amount for the transaction.                                              |
| transactionDateTime  | string | **Mandatory:** Server time the transaction happened                                                              |
| clientSaleId         | string | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction. Supplied by Merchant. |
| voucherSerialNumber  | string | **Mandatory:** Voucher serial number as supplied by Provider – not guaranteed to be unique                       |
| voucherPinNumber     | string | Voucher Pin Number as supplied by Provider                                                                       |
| provider             | int    | **Mandatory:** Airtime Provider, possible values – [Mascom, Orange, beMobile]. Case is important                 |

**LastResponse Request Examples**

The example request and response demonstrate a LastResponse Request call and response.

**Example Request**

This example uses the following fields and values:

````javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}");//Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
"clientSaleId": "1639111159"//Replace with the actual clientSaleId
"outletId": "{{ outlet-Id }}" //Replace with the actual outletId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/advice", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));

````
**Example Response**
This example above makes a request for clientSaleId:889898989889 which for purposes of illustration had timed out, whilst the call had resulted in a successful sale.

````javascript
{
      "code": "0",
    	"airtimeID": 102611,
    	"provider": "ORANGE",
    	"transactionAmount": 10,
    	"voucherPinNumber": "914 446",
    	"voucherSerialNumber": "1000228",
    	"clientSaleId": "889898989889",
    	"transactionId": "tiGNZ2gUCiW4JKJpZ4b0",
    	"transactionStatus": "Successful",
    	"saleDate": "2022-01-20 13:43:52",
    	"transactionDateTime": "2022-01-20 13:52:59"
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

<br>

### Prepaid Pinless Airtime Credit Request Use Case

#### Prepaid Pinless Airtime Credit Request Description
The Prepaid Pinless Airtime Credit Request use case is used to purchase a direct recharge of a defined amount for a specific mobile network operator without the need for a PIN.

#### Prepaid Pinless Airtime Credit Request Desired Outcome
The customer specifies the details of the direct recharge they require and pays for it (mobile network operator, amount, and mobile number).

#### Prepaid Pinless Airtime Credit Request Preconditions
- The mobile network operator shall be supplied.
- The amount shall be supplied.
- The mobile number to be recharged shall be supplied.

#### Prepaid Pinless Airtime Credit Request Participants
- The customer
- The Vendor
- PrepaidPlus Server
- Prepaid Airtime server

#### Prepaid Pinless Airtime Credit Request Workflow
The workflow for the prepaid pinless airtime credit request process is as presented below.

**Table: Prepaid Pinless Airtime Credit Request Workflow**

| Step No | Client | PrepaidPlus Server |
|---------|--------|--------------------|
| 1 | The cashier collects payment from the customer, and selects the desired Mobile Network Operator for which a direct recharge is required. Thereafter, the cashier selects and submits the required recharge value and the customer's mobile number. <br> NB: Once the request is confirmed with PrepaidPlus, the sale cannot be rolled back. | |
| 2 | | PrepaidPlus receives confirmation of the prepaid pinless airtime credit request from the merchant. PrepaidPlus validates the merchant API Key and available balance. |
| 3 | | The transaction is processed on the prepaid airtime server, and a direct recharge is dispatched to the customer's mobile number once the transaction is successfully processed. |
| 4 | The merchant confirms the successful recharge to the customer. | |

##### Prepaid Pinless Airtime Credit Request Happy Path
The prepaid pinless airtime credit request use case sequence diagram is illustrated below.

**Fig: Prepaid Pinless Airtime Credit Request Happy Path**



###  Prepaid Pinless Airtime Methods

####  Pinless Airtime Credit Request

The method automatically recharges a provided number. The service is available for Orange, beMobile, and Mascom.

**Send a Pinless Airtime Request**

Send an Airtime Pinless Credit request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/creditvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/creditvend)

The AirtimePinless CreditVend Request requires the following fields:

**Table: Airtime Credit Vend Request requirements**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey           | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header as part of Basic Authentication scheme. |
| password         | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| outletId         | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |
| outletName       | string | **Mandatory:** This is the name of store or sale channel the purchase is being made from.                        |
| operatorId       | string | **Mandatory:** This is a unique Identifier of the person carrying out customer request.                          |
| terminalId       | string | **Mandatory:** This is an identifier for the device or channel carrying out the transaction.                     |
| clientSaleId     | string | **Mandatory:** This is the Sale/transaction Id that uniquely identifies sale from client side and is shared with PrepaidPlus for reference. |
| provider         | string | **Mandatory:** This is the airtime provider. It’s a finite set Mascom Pinless, Orange Pinless, beMobile Pinless. Provider must be one of above with no padded empty characters. |
| cellNumber       | int    | **Mandatory:** This is the cellphone number to be recharged.                                                     |
| transactionAmount| int    | **Mandatory:** This is the airtime credit purchase amount. Refer to Table below for denominations on offer.      |

**Table: Airtime Denominations – Testing Server**

| Provider        | Denomination |
|-----------------|--------------|
| Orange Pinless  | 2 - 500      |
| Mascom Pinless  | 2 - 1000     |
| beMobile Pinless| 2 - 1000     |

**Making the Pinless AirtimeCredit Request**

The AirtimeCredit call is successful when you receive a response with airtime credit voucher for the selected provider and denomination. If the call fails, you will receive one of the following errors:

**Table: Pinless AirtimeCredit Request Errors**

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
| 108      | Provider missing         | Missing a mandatory field - provider                                        |
| 109      | transactionAmount missing| Missing a mandatory field – Transaction Amount                              |
| 202      | Insufficient Balance     | Merchant balance is less than sale amount.                                  |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: Mascom, Orange – Etc) |
| 211      | Airtime Vending Denied   | Merchant not setup to sell Airtime.                                         |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**Pinless AirtimeCredit Response Parameters**

**Table : AirtimeCredit Response**

| Argument             | Type   | Description                                                                                                      |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code                 | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| provider             | string | Airtime Provider, possible values – [Mascom Pinless, Orange Pinless, beMobile Pinless].                          |
| transaction_reference| string | Unique product reference, generated by server                                                                    |
| providerReference    | string | Sale reference from the provider selected for purchase                                                           |
| msisdn               | string | Cellphone number to be recharged                                                                                 |
| response             | element| **Mandatory:** This is the response from the sale, whether successful or failed                                   |
| transactionId        | string | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale. Supplied by PrepaidPlus.    |
| transactionAmount    | element| **Mandatory:** This is the denomination amount for the transaction.                                              |
| transactionDateTime  | string | **Mandatory:** Server time the transaction happened                                                              |
| clientSaleId         | string | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction. Supplied by Merchant. |

**PinlessAirtimeCredit Request Examples**

The example request and response demonstrate a Pinless Airtime Credit Request call and response. The example query demonstrates the use of the 'Authorization' header which uses Basic Authentication. The string passed is  (Base 64 encoded) APIkey as user and merchant password.


````javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}");//Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
    "clientSaleId": "1656881928",//Replace with the actual clientSaleId
    "provider": "Mascom Pinless",//Replace with the actual provider
    "transactionAmount": 10,//Replace with the actual transactionAmount
    "cellNumber": "75636865",//Replace with the actual cellNumber
    "merchantName": "Fino",//Replace with the actual merchantName
    "outletName": "Lesedi",//Replace with the actual outletName
    "operatorId": "Lesedi",//Replace with the actual operatorId
    "terminalId": "Web",//Replace with the actual terminalId
    "outletId": "{{ outlet-Id }}"//Replace with the actual outletId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/creditvend", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
````

**Example Response**

````javascript
{
        "code": "0",
    		"transaction_reference": 467331585,
    		"provider": "Mascom Pinless",
    		"providerReference": "2607331585",
    		"msisdn": "75636865",
    		"amount": 10,
    		"clientSaleId": "1656881928",
    		"transactionDateTime": "2022-07-12T06:44:38.440Z",
    		"response": "Successful",
    		"transactionId": "RcgBZcAyiWHCGVJt5h38"
}
````

**Example Fault**

````javascript
{
"code": "211"
"clientSaleId": "16391111599"
"response": "Failure",
"message": " Airtime Vending Denied ",
"description": "Merchant not setup to sell Airtime."
}

````
#### Pinless Airtime LastResponse

This method is called subsequent to an ongoing Pinless Airtime Purchase Request network timeout/connection failure or an exception. Its purpose is to check if a payment had been successfully made prior to abandoning the payment. In an event that the failed Pinless Airtime Purchase Request had resulted in a successful payment, the payment receipt is retrieved and returned for printing, otherwise the payment is abandoned.

**Send a LastResponse Request Call**

Send a LastResponse request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/creditvendLastTransaction](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/creditvendLastTransaction)

The LastResponse call requires the following fields:

**Table: Last Response call requirements**

| Argument     | Type   | Description                                                                                                      |
|--------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey       | string | **Mandatory:** Secret API Key authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password     | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId | string | **Mandatory:** This is a customer supplied Id corresponding to an Airtime purchase they would like to check status of. Obtained from Airtime purchase call. |
| outletId     | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |

**Making the LastResponse Request**

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
| 102      | clientSaleId is missing  | clientSaleId is not provided.                                               |
| 103      | OutletId is missing      | outletId is not provided                                                    |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: Mascom, Orange – Etc) |
| 208      | Invalid clientSaleId     | Could not find a transaction for the supplied client sale transaction Id.   |
| 209      | Sale not found           | The server did not process the requested transaction. No sale was made      |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**LastResponse Response Parameters**

**Table: Last Response Response parameters**

| Argument             | Type   | Description                                                                                                      |
|----------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code                 | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| provider             | string | Airtime Provider, possible values – [Mascom Pinless, Orange Pinless, beMobile Pinless].                          |
| transaction_reference| string | Unique product reference, generated by server                                                                    |
| providerReference    | string | Sale reference from the provider selected for purchase                                                           |
| msisdn               | string | Cellphone number to be recharged                                                                                 |
| response             | element| **Mandatory:** This is the response from the sale, whether successful or failed                                   |
| transactionId        | string | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale. Supplied by PrepaidPlus.    |
| transactionAmount    | element| **Mandatory:** This is the denomination amount for the transaction.                                              |
| transactionDateTime  | string | **Mandatory:** Server time the transaction happened                                                              |
| clientSaleId         | string | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction. Supplied by Merchant. |

**LastResponse Request Examples**

The example request and response demonstrate a LastResponse Request call and response.

**Example Request**

This example uses the following fields and values:

````javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}");//Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "clientSaleId": "1639111159"//Replace with the actual clientSaleId
"outletId": "{{ outlet-Id }}" //Replace with the actual outletId
});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/airtime-pinless/advice", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));

````

**Example Response**

This example above makes a request for clientSaleId:1656881928 which for purposes of illustration had timed out, whilst the call had resulted in a successful sale.


````javascript
{
      "code": "0",
    	"transaction_reference": 467331585,
    	"provider": "Mascom Pinless",
    	"providerReference": "2607331585",
    	"msisdn": "75636865",
    	"amount": 10,
    	"clientSaleId": "1656881928",
    	"transactionDateTime": "2022-07-12T06:44:38.440Z",
    	"response": "Successful",
    	"transactionId": "RcgBZcAyiWHCGVJt5h38"
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
