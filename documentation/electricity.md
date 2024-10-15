

## PWS Prepaid Electricity Use Cases and Workflows

The Use Case and workflow are presented for each task. A use case is defined as a single task, performed by the end user of the system, which has some useful outcome to the end user.

It must be noted that the use cases and workflows are only meant to guide those involved in the development of the interface and are in no way meant to be a technical definition of how the interface will work. The technical definition of the interface is presented at  Methods.

Vend use cases are used to describe the functionality exposed by the Vend protocol. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers
- Vendors
- Vend client
- Vend Server


#### Download Postman Collection
You can download the Postman collection for this use case <a href="/assets/electricityPostmanCollection.json" download>here</a>. <!-- Replace with the actual path to your Postman collection file -->
### Use Case Actors, Responsibilities and Collaborators

Table  describes use case actors, their responsibilities and collaborators:

| Use case actor | Responsibilities                                                                 | Collaborators                           |
|----------------|----------------------------------------------------------------------------------|-----------------------------------------|
| Customer       | - Initiates customer use cases.                                                  | - Vending Operator         |
|                | - Provides transaction information.                                              |                                         |
|                | - Tenders payment.                                                               |                                         |
|                | - Receives requested token receipt.                                              |                                         |
| Vending Operator (Merchant) | - Verifies customer information.                                     | - PrepaidPlus Server                    |
|                | - Submits vending requests on behalf of customer.                                |                                         |
|                | - Performs operator specific tasks.                                              |                                         |
|                | - Hands generated tokens to customers.                                           |                                         |
|                | - Implements the “LastResponse” use case for use cases that might require this service. |                                         |
|                | - Handles customer queries.                                                      |                                         |
| PrepaidPlus Server | - Authenticates the Vend Server.                                              | - Prepaid Electricity Service Provider’s server |
|                | - Compiles and sends Vend request messages to Vend server.                       |                                         |
|                | - Receives and formats Vend response messages from the Vend server.              |                                         |
|                | - Automatically initiates the “LastResponse” use case for use cases that might require this service. |                                         |
| Prepaid Electricity Service Provider’s server | - Authenticates the Vend clients.                                      | - PrepaidPlus Server                    |
|                | - Complies with an appropriate Vend response message based on its application business logic. |                                         |
|                | - Responds with a fault response message, if required.                           |                                         |

### Prepaid Electricity Methods


**Table : Supported Prepaid Electricity Methods**

| Operation              | Description |
|------------------------|-------------|
| TrialCreditVend Request | Verifies the meter number and if the customer account is in good standing, i.e not blocked or suspended. It also ensures there is enough credit/balance for the transaction to be a success. The TrialCreditVend returns the customer details and a proceed or fail indication. |
| CreditVend Request      | Confirms purchase of a prepaid electricity voucher. It can be used in concert with the positive outcome of the TrialCreditVend or on its own. The method returns a prepaid electricity voucher. |
| LastResponse Request    | This method is called subsequent to an ongoing CreditVend Request network timeout/connection failure or an exception. Its purpose is to check if a voucher had been successfully sold prior to abandoning the sale. In an event that the failed CreditVend Request had resulted in a successful sale the voucher is retrieved and returned for printing, otherwise the sale is abandoned. |


## Trial Credit Vend Use Case

#### Trial Credit Vend Description

The Trial Credit Vend use case functions exactly like the Credit Vend use case, except that the server does not generate a “valid” prepaid electricity credit token. It is used to confirm that the meter is valid and can vend, as well as to confirm that there is sufficient balance available to fulfill the transaction.

#### Trial Credit Vend Desired Outcome

The Vendor wants to confirm whether a token can be generated successfully for a particular meter. The PrepaidPlus and Service Provider’s Electricity vending servers perform all required validations for a real transaction but a token is not created for the transaction. The PrepaidPlus server returns a positive, or failure response to the Vendor.

#### Trial Credit Vend Preconditions

- The meter number shall be supplied.
- The sale amount shall be supplied.

#### Trial Credit Vend Participants

- The customer
- The Vendor/ Merchant
- PrepaidPlus Server
- Service Provider’s Electricity vending server

#### Trial Credit Vend Key Business Rules

- Sufficient credit availability must be verified.
- No actual tokens are generated.
- Normal Vending related exception messages are implemented as they would be for standard vending operation. (i.e Validity of Meter number for vending, and availability of sufficient balance)

#### Trial Credit Vend Workflow

The workflow for the Trial Credit Vend process is as presented below in Table . The trial credit vend is optional, and is used in instances where the merchant seeks to confirm the details of the customer and that a recharge voucher can be sold for the meter number supplied.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | Customer will submit a valid and complete prepaid electricity purchase request. A complete and valid request includes the meter number and the amount of electricity required in Pula (i.e. no Thebe, with minimum amount of P1). |                                                                                    |
| 2        | The Merchant (Vending Client) then makes a valid and complete API call for prepaid electricity purchase. This call should also provide the API key of the merchant for authentication. The merchant must provide the following in addition to the information provided by the customer; TerminalID, OperatorId and ClientSaleID (The ClientSale ID uniquely identifies the transaction both on vending client and PWS). |                                                                                    |
| 3        |                                                                        | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. PrepaidPlus validates the meter number (i.e. does it fall in the format of the service provider’s meter numbers). If the meter number fails the validation, an error message is returned. PrepaidPlus also checks that the sale meets the minimum amount allowed for prepaid electricity. If the sale amount is below the permitted minimum amount, an error message is returned. PrepaidPlus checks the available balance in the Merchant account (i.e. within PrepaidPlus system) to ensure there is sufficient balance to meet the amount requested for purchase. If the balance is insufficient, an error message is returned. If all the checks above are positive, PrepaidPlus sends a request to the service provider with the customer meter number and amount. |
| 4        | If the Merchant receives a successful response from PrepaidPlus, the customer names and purchase amount is then confirmed. NB: Merchant can now proceed with sale. |                                                                      
<br>              |

### 	Trial Credit Vend Happy Path 

The Trial Credit Vend use case sequence diagram is illustrated below.

![Credit Vend Happy Path](/assets/trialVendHappyPath.png)
<br>

 ## TrialCreditVend Request

The TrialCreditVend Request is used to verify that the following are in place before generating an electricity token:
- The meter number is valid and is the correct one the buyer intends to make purchase for, and also that the account has not been blocked.
- The merchant used to purchase the electricity token has enough positive credit to fulfil the sale at hand.

The TrialCreditVend request either returns the customer details, or returns an indication that the transaction has failed, as well as an explanation of the cause of the failure.

**Send a TrialCreditVend Call Request**

Send a TrialCreditVend request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/trialvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/trialvend)

The TrialCreditVend call requires the following fields:

**Table : Trial Credit Vend call requirements**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey           | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password         | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| terminalId       | string | **Mandatory:** This is the unique Identifier of sale channel making the purchase.                                 |
| clientSaleId     | string | **Mandatory:** This is the sale/transaction Id that uniquely identifies sale.                                     |
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| transactionAmount| int    | **Mandatory:** This is the purchase amount. Supplied by Cashier/Operator (Minimum P1).                            |
| outletId         | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |
| operatorId       | string | **Mandatory:** This is a unique Identifier of the person carrying out customer request.                          |

The Meters to be used for the test environment are the following:

**Table: Test Meter Numbers**

| Meter Number    | EndPoint Response                                      |
|-----------------|--------------------------------------------------------|
| 04040404040     | Successful Vend Response – Business Customer Meter     |
| 94949494949     | Successful Vend Response – Domestic Customer Meter     |
| 04040404453     | Blocked customer                                       |
| 04040406698     | Meter not found                                        |
| Any Other Meter | Failed Luhn Check                                      |

**Making the TrialCreditVend Call**

The TrialCreditVend Request call is successful when you receive the following transaction details: customer name, meter number and purchase amount. If the call fails, you will receive one of the following faults/errors:

**Table: Trial Credit Vend Errors**

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
| 106      | meterNumber missing      | Meter number is not provided.                                               |
| 107      | transactionAmount missing| Transaction purchase amount is not provided.                                |
| 202      | Insufficient Balance     | Merchant balance is less than sale amount.                                  |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: BPC, Mascom, Orange – Etc) |
| 204      | Disabled Outlet          | The provided OutletId is currently disabled.                                |
| 205      | Invalid Outlet Id        | The provided OutletId was not found.                                        |
| 206      | Minimum/Maximum amount error | Amount requested is either less or more than the allowed amount.           |

The above errors are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**Trial Credit Vend Response**

**Table : Trial Credit Vend Response**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| transactionAmount| int    | **Mandatory:** This is the purchase amount. (Minimum P1, and Maximum P5,000). These limits can be adjusted to the merchant’s requirements. |
| custVendDetail   | element| **Mandatory:** This is the customer details; Name, address and contact number.                                    |
| clientSaleId     | string | **Mandatory:** This is the Merchant sale/transaction Id that uniquely identifies transaction. Supplied by merchant’s POS Configuration. |

**Table: CustVendDetail**

This is the customer details as supplied by the prepaid electricity service provider: Name, address and contact number.

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| name             | string | **Mandatory:** Name of Meter owner.                                                                              |
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| amtTendered      | int    | **Mandatory:** This is the purchase amount. (Minimum P1, and Maximum P5,000). These limits can be adjusted to the merchant’s requirements. |

**TrialCreditVend Examples**

The example request and response demonstrate a TrialCreditVend Request call and response.

**Example Request**

This example uses the following fields and values:
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}"); // Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
    "meterNumber": "84456251565655", // Replace with the actual meter number
    "transactionAmount": 10, // Replace with the actual transaction amount
    "terminalId": "web", // Replace with the actual terminal ID
    "clientSaleId": "841055445601", // Replace with the actual client sale ID
    "outletId": "{{ outlet-Id }}", // Replace with the actual outlet ID
    "operatorId": "84966-01" // Replace with the actual operator ID
});

var requestOptions = {
    method: 'POST',
    headers: myHeaders,
    body: raw,
    redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/trialvend", requestOptions)
    .then(response => response.text())
    .then(result => console.log(result))
    .catch(error => console.log('error', error));
```

**Example Response**
```javascript
{
    "code": "0",
    "custVendDetail": {
        "name": "John Doe",
        "meterNumber": "04040404040", 
        "amtTendered": 10 
    },
    "clientSaleId": "841055445601", 
    "transactionAmount": 10, 
    "response": "Successful"
}
```

**Example Fault**
```javascript
{
    "code": "103",
    "response": "Failure",
    "message": "OutletId is missing",
    "description": "OutletId is not provided",
    "transactionId": null,
    "meterNumber": "04040404040", 
    "amount": 10, 
    "status": 500
}
```
<br>


## Credit Vend Use Case

#### Credit Vend Description
The credit vend token use case is used to purchase prepaid electricity credit tokens.

####  Credit Vend Desired Outcome
The customer pays for and receives the purchased credit token.

####  Credit Vend Preconditions
- The meter number shall be supplied.
- The sale amount shall be supplied.

####  Credit Vend Participants
- The customer
- The Vendor/Merchant
- PrepaidPlus Server
- Service Provider’s Electricity vending server

####  Credit Vend Workflow
The workflow for the Credit Vend process is as presented below in Table .

**Table: Credit Vend Workflow**

| Step No | Client | PrepaidPlus Server |
|---------|--------|--------------------|
| 1 | Customer submits a valid and complete prepaid electricity purchase request. A complete and valid request includes the meter number and the amount of electricity required in Pula (i.e. minimum amount of P1 with no Thebe). | |
| 2 | The Merchant (Vending Client) then makes a valid and complete API call to confirm prepaid electricity purchase. This call should provide the API key for the merchant for authentication, and will also provide the following in addition to the information provided by the customer; TerminalID, OperatorID and ClientSaleID. NB: Once Merchant confirms the sale with PrepaidPlus, the sale cannot be rolled back. | |
| 3 | | PrepaidPlus receives confirmation of the purchase from Merchant. PrepaidPlus validates the merchant API key. If valid, proceeds to subsequent steps, or else returns an API error. PrepaidPlus validates the meter number (i.e. does it fall in the format of the service provider’s meter numbers). If the meter number fails the validation, an error message is returned. PrepaidPlus also checks that the sale meets the minimum amount allowed for prepaid electricity. If the sale amount is below the permitted minimum amount, an error message is returned. PrepaidPlus checks the available balance in the Merchant account (i.e. within PrepaidPlus system) to ensure there is sufficient balance to meet the amount requested for purchase. If the balance is insufficient, an error message is returned. If all the checks above are positive, PrepaidPlus sends a request to the service provider with the customer meter number and amount. |
| 4 | | PrepaidPlus sends the customer voucher details with a voucher number to the Merchant. |
| 5 | Merchant formats and returns the voucher details to the customer. | |

The sale is confirmed in both systems, unless there is an error that warrants a “Last Response” call. Refer to Section  below for last response call.

####  Credit Vend Happy Path
The Credit Vend use case sequence diagram is illustrated below.

![Credit Vend Happy Path](/assets/creditVendHappyPath.png)


<br>

##  CreditVend Request

The CreditVend request carries out the actual purchase based on the positive outcome of the TrialCreditVend or when used as a stand-alone. The method returns a prepaid electricity voucher.

**Send a CreditVend Request**

Send a CreditVend request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/creditvend](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/creditvend)

The CreditVend Request requires the following fields:

**Table: Credit Vend Request requirements**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey           | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password         | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| terminalId       | string | **Optional:** This is the unique Identifier of sale channel making the purchase.                                  |
| clientSaleId     | string | **Mandatory:** This is the POS sale/transaction Id that uniquely identifies sale.                                 |
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| transactionAmount| int    | **Mandatory:** This is the purchase amount. Supplied by Cashier/Operator (Minimum P1).                            |
| outletId         | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from.           |
| operatorId       | string | **Mandatory:** This is a unique Identifier of the person carrying out customer request.                          |

**Making the CreditVend Request**

The CreditVendRequest call is successful when you receive a response that has the customer’s prepaid electricity voucher. If the call fails, you will receive one of the following errors:

**Table: Credit Vend Request Errors**

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
| 106      | meterNumber missing      | Meter number is not provided.                                               |
| 107      | transactionAmount missing| Transaction purchase amount is not provided.                                |
| 202      | Insufficient Balance     | Merchant balance is less than sale amount.                                  |
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: BPC, Mascom, Orange – Etc) |
| 204      | Disabled Outlet          | The provided OutletId is currently disabled.                                |
| 205      | Invalid Outlet Id        | The provided OutletId was not found.                                        |
| 206      | Minimum/Maximum amount error | Amount requested is either less or more than the allowed amount.           |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**CreditVend Response Parameters**

**Table: Credit Vend Response**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code             | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| transactionAmount| int    | **Mandatory:** This is the purchase amount.                                                                      |
| response         | element| **Mandatory:** This is the response from the sale, whether successful or failed.                                 |
| transactionId    | string | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale.                             |
| creditVendReceipt| element| **Mandatory:** This is the service providers (Botswana Power Corporation – BPC) receipt details for the transaction. |
| clientSaleId     | string | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction. Supplied by Merchant. |

**Table: Credit Vend Receipt**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| receiptNo        | string | **Mandatory:** Attribute: This is the BPC receipt number.                                                        |
| msno             | string | **Mandatory:** This is the customer meter number.                                                                |
| vatNo            | string | **Mandatory:** BPC – VAT Number                                                                                  |
| tt               | string | **Mandatory:** BPC - Token technology                                                                            |
| at               | string | **Mandatory:** BPC – Algorithm Type                                                                              |
| sgc              | string | **Mandatory:** BPC – Supply group code                                                                           |
| krn              | string | **Mandatory:** BPC - Key revision number.                                                                        |
| ti               | string | **Mandatory:** BPC - Tariff Index                                                                                |
| desc             | string | **Mandatory:** BPC – Transaction description.                                                                    |
| tokenUnits       | float  | **Mandatory:** BPC – Total units purchased                                                                       |
| amtTendered      | float  | **Mandatory:** BPC – Amount tendered to purchase token                                                           |
| costUnits        | float  | **Mandatory:** BPC – Cost of units. (Tendered amount – units)                                                    |
| govLevy          | float  | **Mandatory:** BPC – Tax - Government Levy (retained)                                                            |
| standardCharge   | float  | **Mandatory:** BPC – Tax – Standard charge (retained)                                                            |
| VAT              | float  | **Mandatory:** BPC – VAT retained                                                                                |
| stsCipher        | string | **Mandatory:** BPC – Customer recharge token                                                                     |
| keychangetoken1  |        | **Optional:** BPC – Key change token which must be provided to the customer if a value is returned and should always be displayed above keychangetoken2 and stsCipher |
| keychangetoken2  |        | **Optional:** BPC – Key change token which must be provided to the customer if a value is returned and should always be displayed above stsCipher and below keychangetoken1 |

**CreditVend Request Examples**

The example request and response demonstrate a CreditVend Request call and response.

**Example Request**

This example uses the following fields and values:
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}"); // Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
    "meterNumber": "8515669544744", 
    "transactionAmount": 100, // Replace with actual transaction amount
    "terminalId": "84166444", // Replace with actual terminal ID
    "clientSaleId": "846362626", // Replace with actual client sale ID
    "outletId": "{{outlet-Id }}", // Replace with actual outlet ID
    "operatorId": "8455-02" // Replace with actual operator ID
});

var requestOptions = {
    method: 'POST',
    headers: myHeaders,
    body: raw,
    redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/creditvend", requestOptions)
    .then(response => response.text())
    .then(result => console.log(result))
    .catch(error => console.log('error', error));
```

**Example Response**
```javascript
{
    "code": "0",
    "creditVendReceipt": {
        "clientId": "PP0000", 
        "account": "{{ account }}",
        "location": "123456", 
        "name": "John Doe", 
        "date": "2020-03-02 14:32:14", 
        "receiptNo": "846912871", 
        "amtTendered": 100, 
        "costUnits": 50, 
        "standardCharge": 5, 
        "governmentLevy": 2, 
        "vat": 10,
        "desc": "Credit Token",
        "tariff": "Business",
        "tariffBreakdown": [
            {
                "units": 25, 
                "rate": 2 
            },
            {
                "units": 25, 
                "rate": 2 
            }
        ],
        "tokenUnits": 100,
        "meterType": "Prepaid", 
        "krn": "1",
        "ti": "6", 
        "meterNumber":"8515669544744", 
        "sgc": "300000", 
        "keychangetoken1": "8675 7587 7584 6834",
        "keychangetoken2": "0968 7794 1029 9035", 
        "stsCipher": "3871 0157 9312 8553 5068r" 
    },
    "transactionId": "1s1d55df5g5g5dfg5g5g5d", 
    "clientSaleId": "846362626", 
    "transactionAmount": 100,  
    "response": "Successful"
}
```

**Example Fault**
```javascript
{
    "code": "123", 
    "response": "Failure",
    "message": "MinimumMaximum amount error",
    "description": "Amount requested is either less or more than the allowed amount."
}
```

<br>


## Last Response Use Case

#### Last Response Description
The Last Response use case is used to ensure that the Vendor and PrepaidPlus share the same understanding of the outcome of the Credit Vend use case. The Last Response use case is required when an exception occurs while a credit vend transaction is being processed, such as communication that fails between a client and a server after a request has been sent and the client is waiting for a response.

This Use Case is implemented by the Vendor, and will be automatically called if the credit vend use case exceeds the Vendor configured duration without a valid response from PrepaidPlus. The specific duration is determined and set by the vendor, taking into consideration the unique Vendor circumstances as well as the reliability and bandwidth of the communications infrastructure. This Use Case is only applicable for Credit Vend. This is because the Credit Vend outcome is always assumed final and cannot be reversed. Vendors would have to determine if the Credit Vend use case has been processed by the PrepaidPlus server by issuing a “Last Response” request.

#### Last Response Desired Outcome
- If the referenced Last Response “ClientSaleId” exists on the PrepaidPlus Server, and PrepaidPlus determines the sale was successful, the Server will return the customer receipt for printing by the Merchant.
- If the referenced Last Response “ClientSaleID” exists on the PrepaidPlus Server, and PrepaidPlus determines the sale was not successful, the Server will return a message that the transaction was not successful. The vendor can initiate a new Credit Vend.
- If the referenced Last Response ClientSaleID does not exist on the PrepaidPlus Server, the PrepaidPlus Server will return an exception message to indicate that the referenced request does not exist. The Vendor can then continue with a new Credit Vend.
- The Vendor and PrepaidPlus share the same understanding of the outcome of the Credit Vend use case.

####  Last Response Preconditions
Last Response request messages should be issued when the vendor has experienced a failure on Credit Vend request.

####  Last Response Participants
- The Vendor
- PrepaidPlus Server

####  Last Response Workflow
The workflow for the Last Response process is as presented below in Table 3.4.

**Table : Last Response Workflow**

| Step No | Client | PrepaidPlus Server |
|---------|--------|--------------------|
| 1 | In the event that the vendor does not receive a response from PrepaidPlus for a Credit Vend request, the vendor will automatically generate a Last Response request. | |
| 2 | | If the referenced Last Response ClientSaleId ID exists on the PrepaidPlus Server, and has a corresponding Credit Token, then the Server will return the relevant customer receipt for that ClientSaleId. If the referenced Last Response ClientSaleId exists on the PrepaidPlus Server, but PrepaidPlus does not have the corresponding credit token from the prepaid electricity service provider, then PrepaidPlus initiates a Last Response request to the prepaid electricity service provider. |
| 3 | | PrepaidPlus receives the credit token from the prepaid electricity service provider, and sends it to the Vendor, or PrepaidPlus receives the exception message from the prepaid electricity service provider and sends it to the Vendor. |
| 4 | The vendor prints out the Credit Vend customer receipt and hands it over to the customer. If the Credit Vend had not been successful, then the vendor prints out the exception message and continues to vend. | |
| 5 | NB: Both PrepaidPlus and the Merchant systems keep their own data for each sale for cross-checking and cross-referencing. | |
<br>

####  Last Response Happy Path
The Last Response use case sequence diagram is illustrated in figure  below.

**Fig: Last Response Happy Path**

![Last Response Happy Path](/assets/lastResponseHappyPath.png)

<br>

##  LastResponse Request

This method is called subsequent to an ongoing CreditVend Request network timeout/connection failure or an exception. Its purpose is to check if a voucher had been successfully sold prior to abandoning the sale. In an event that the failed CreditVend Request had resulted in a successful sale, the voucher is retrieved and returned for printing, otherwise the sale is abandoned.

**Send a LastResponse Request Call**

Send a LastResponse request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/advice](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/advice)

The LastResponse call requires the following fields:

**Table: Last Response call requirements**

| Argument     | Type   | Description                                                                                                      |
|--------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey       | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password     | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| clientSaleId | string | **Mandatory:** This is the Sale/transaction Id that uniquely identifies the transaction. Supplied by Merchant.    |
| outletId     | string | **Mandatory:** This is the unique Identifier of the store or sale channel the purchase is being made from.       |

**Making the LastResponse Request call**

The LastResponse Request call is successful when you receive the customer’s pre-paid voucher, or confirmation that no sale transaction had been processed. The implementing Vendor will determine the elapsed time interval to call the LastResponse. If the call fails, you will receive one of the following faults/errors:

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
| 203      | Vendor Error             | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: BPC – Etc) |
| 208      | Invalid clientSaleId     | Could not find a transaction for the supplied client sale transaction Id.   |
| 209      | No last response found   | The server did not process the requested transaction. No sale was made      |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**LastResponse Response Parameters**

**Table : Last Response Response parameters**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| code             | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| transactionAmount| int    | **Mandatory:** This is the purchase amount.                                                                      |
| response         | element| **Mandatory:** This is the response from the sale effort, whether successful or failed.                          |
| transactionId    | string | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale.                             |
| creditVendReceipt| element| **Mandatory:** This is the service providers receipt details for the transaction.                                |
| clientSaleId     | string | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction.                |

**Table 6.14: Credit Vend Receipt**

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| receiptNo        | string | **Mandatory:** Attribute: This is the BPC receipt number.                                                        |
| msno             | string | **Mandatory:** This is the customer meter number.                                                                |
| vatNo            | string | **Mandatory:** BPC – VAT Number                                                                                  |
| tt               | string | **Mandatory:** BPC - Token technology                                                                            |
| at               | string | **Mandatory:** BPC – Algorithm Type                                                                              |
| sgc              | string | **Mandatory:** BPC – Supply group code                                                                           |
| krn              | string | **Mandatory:** BPC - Key revision number.                                                                        |
| ti               | string | **Mandatory:** BPC - Tariff Index                                                                                |
| desc             | string | **Mandatory:** BPC – Transaction description.                                                                    |
| tokenUnits       | float  | **Mandatory:** BPC – Total units purchased                                                                       |
| amtTendered      | float  | **Mandatory:** BPC – Amount tendered to purchase token                                                           |
| costUnits        | float  | **Mandatory:** BPC – Cost of units. (Tendered amount – units)                                                    |
| govLevy          | float  | **Mandatory:** BPC – Tax - Government Levy (retained)                                                            |
| standardCharge   | float  | **Mandatory:** BPC – Tax – Standard charge (retained)                                                            |
| VAT              | float  | **Mandatory:** BPC – VAT retained                                                                                |
| stsCipher        | string | **Mandatory:** BPC – Customer recharge token                                                                     |
| keychangetoken1  |        | **Optional:** BPC – Key change token which must be provided to the customer if a value is returned and should always be displayed above keychangetoken2 and stsCipher |
| keychangetoken2  |        | **Optional:** BPC – Key change token which must be provided to the customer if a value is returned and should always be displayed above stsCipher and below keychangetoken1 |

**LastResponse Request Examples**

The example request and response demonstrate a LastResponse Request call and response.

```javascript
// Create headers
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic {{ base64string }}"); // Replace with actual Base64 encoded API key and password
myHeaders.append("Content-Type", "application/json");

// Create request body
var raw = JSON.stringify({
    "clientSaleId": "745843218", // Replace with actual client sale ID
    "outletId": "{{ Outlet-Id }}" // Replace with actual outlet ID
});

// Create request options
var requestOptions = {
    method: 'POST',
    headers: myHeaders,
    body: raw,
    redirect: 'follow'
};

// Send request
fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/advice", requestOptions)
    .then(response => response.text())
    .then(result => console.log(result))
    .catch(error => console.log('error', error));
```

**Example Response**


**Response**
```javascript
{
    "code": "0",
    "creditVendReceipt": {
        "clientId": "PP000", 
        "account": "{{ account }}", 
        "location": "52387", 
        "name": "DIPAMPIRI DINEO", 
        "date": "2020-03-28 13:37:12", 
        "receiptNo": "545582658", 
        "amtTendered": 10, 
        "costUnits": 8.19, 
        "standardCharge": 0.43, 
        "governmentLevy": 0.43, 
        "vat": 0.95, 
        "desc": "Credit Token",
        "tariff": "Domestic",
        "tokenUnits": "9.6kWh", 
        "meterType": "02", 
        "krn": "1",
        "ti": "1", 
        "meterNumber": "94949494949", 
        "sgc": "200000", 
        "keychangetoken1": "8675 7587 7584 6834", 
        "keychangetoken2": "0968 7794 1029 9035", 
        "stsCipher": "3871 0157 9312 8553 5068"
    },
    "transactionId": "45sdr4fr4ft5r55t5t5t", 
    "clientSaleId": "745843218", 
    "transactionAmount": 10, 
    "response": "Successful"
}
```


**Example Fault**
```javascript
{
    "code": "02",
    "response": "Failure",
    "message": "Invalid API Key",
    "description": "Unable to authenticate supplied API key."
}
```