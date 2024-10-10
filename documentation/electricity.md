<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
            background-color: #f4f4f9;
            color: #333;
        }
        .logo {
            display: block;
            margin-bottom: 20px;
        }
        .button {
            display: inline-block;
            padding: 10px 20px;
            font-size: 16px;
            margin: 10px 0;
            background: #063970;
            color: white;
            border-radius: 10px;
            text-decoration: none;
            transition: background 0.3s ease, transform 0.3s ease;
        }
        .button:hover {
            background: #052a5e;
            transform: scale(1.05);
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
            background-color: #fff;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 12px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        h2, h3 {
            color: #063970;
        }
        p {
            margin: 10px 0;
        }
    </style>
</head>
<body>

<img src="../documentation/Prepaidplus%20logo-01.png" alt="PrepaidPlus Logo" width="150" class="logo"/>

## PWS Prepaid Electricity Use Cases and Workflows

The Use Case and workflow are presented for each task. A use case is defined as a single task, performed by the end user of the system, which has some useful outcome to the end user.

It must be noted that the use cases and workflows are only meant to guide those involved in the development of the interface and are in no way meant to be a technical definition of how the interface will work. The technical definition of the interface is presented at Section 5 - Methods.

Vend use cases are used to describe the functionality exposed by the Vend protocol. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers
- Vendors
- Vend client
- Vend Server

### Use Case Actors, Responsibilities and Collaborators

Table 3.1 describes use case actors, their responsibilities and collaborators:

| Use case actor | Responsibilities                                                                 | Collaborators                           |
|----------------|----------------------------------------------------------------------------------|-----------------------------------------|
| Customer       | - Initiates customer use cases.                                                  | - Vending Operator – (Finclude)         |
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

### Trial Credit Vend Use Case

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

The workflow for the Trial Credit Vend process is as presented below in Table 4.2. The trial credit vend is optional, and is used in instances where the merchant seeks to confirm the details of the customer and that a recharge voucher can be sold for the meter number supplied.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | Customer will submit a valid and complete prepaid electricity purchase request. A complete and valid request includes the meter number and the amount of electricity required in Pula (i.e. no Thebe, with minimum amount of P1). |                                                                                    |
| 2        | The Merchant (Vending Client) then makes a valid and complete API call for prepaid electricity purchase. This call should also provide the API key of the merchant for authentication. The merchant must provide the following in addition to the information provided by the customer; TerminalID, OperatorId and ClientSaleID (The ClientSale ID uniquely identifies the transaction both on vending client and PWS). |                                                                                    |
| 3        |                                                                        | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. PrepaidPlus validates the meter number (i.e. does it fall in the format of the service provider’s meter numbers). If the meter number fails the validation, an error message is returned. PrepaidPlus also checks that the sale meets the minimum amount allowed for prepaid electricity. If the sale amount is below the permitted minimum amount, an error message is returned. PrepaidPlus checks the available balance in the Merchant account (i.e. within PrepaidPlus system) to ensure there is sufficient balance to meet the amount requested for purchase. If the balance is insufficient, an error message is returned. If all the checks above are positive, PrepaidPlus sends a request to the service provider with the customer meter number and amount. |
| 4        | If the Merchant receives a successful response from PrepaidPlus, the customer names and purchase amount is then confirmed. NB: Merchant can now proceed with sale. |                                                                                    |

### Credit Vend Happy Path

The Credit Vend use case sequence diagram is illustrated below.

![Credit Vend Happy Path](../documentation/trial%20vand%20happy%20path.png)

### Last Response Use Case

#### Last Response Description

The Last Response use case is used to ensure that the Vendor and PrepaidPlus share the same understanding of the outcome of the Credit Vend use case. The Last Response use case is required when an exception occurs while a credit vend transaction is being processed, such as communication that fails between a client and a server after a request has been sent and the client is waiting for a response.

This Use Case is implemented by the Vendor, and will be automatically called if the credit vend use case exceeds the Vendor configured duration without a valid response from PrepaidPlus. The specific duration is determined and set by the vendor, taking into consideration the unique Vendor circumstances as well as the reliability and bandwidth of the communications infrastructure. This Use Case is only applicable for Credit Vend. This is because the Credit Vend outcome is always assumed final and cannot be reversed. Vendors would have to determine if the Credit Vend use case has been processed by the PrepaidPlus server by issuing a “Last Response” request.

#### Last Response Desired Outcome

- If the referenced Last Response “ClientSaleId” exists on the PrepaidPlus Server, and PrepaidPlus determines the sale was successful, the Server will return the customer receipt for printing by the Merchant.
- If the referenced Last Response “ClientSaleID” exists on the PrepaidPlus Server, and PrepaidPlus determines the sale was not successful, the Server will return a message that the transaction was not successful. The vendor can initiate a new Credit Vend.
- If the referenced Last Response ClientSaleID does not exist on the PrepaidPlus Server, the PrepaidPlus Server will return an exception message to indicate that the referenced request does not exist. The Vendor can then continue with a new Credit Vend.
- The Vendor and PrepaidPlus share the same understanding of the outcome of the Credit Vend use case.

#### Last Response Preconditions

Last Response request messages should be issued when the vendor has experienced a failure on Credit Vend request.

#### Last Response Participants

- The Vendor
- PrepaidPlus Server

#### Last Response Workflow

The workflow for the Last Response process is as presented below in Table 3.4.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | In the event that the vendor does not receive a response from PrepaidPlus for a Credit Vend request, the vendor will automatically generate a Last Response request. |                                                                                    |
| 2        |                                                                        | If the referenced Last Response ClientSaleId ID exists on the PrepaidPlus Server, and has a corresponding Credit Token, then the Server will return the relevant customer receipt for that ClientSaleId. If the referenced Last Response ClientSaleId exists on the PrepaidPlus Server, but PrepaidPlus does not have the corresponding credit token from the prepaid electricity service provider, then PrepaidPlus initiates a Last Response request to the prepaid electricity service provider. |
| 3        |                                                                        | PrepaidPlus receives the credit token from the prepaid electricity service provider, and sends it to the Vendor, or PrepaidPlus receives the exception message from the prepaid electricity service provider and sends it to the Vendor. |
| 4        | The vendor prints out the Credit Vend customer receipt and hands it over to the customer. If the Credit Vend had not been successful, then the vendor prints out the exception message and continues to vend. |                                                                                    |
| 5        | NB: Both PrepaidPlus and the Merchant systems keep their own data for each sale for cross checking and cross referencing. |                                                                                    |

### Last Response Happy Path

The Last Response use case sequence diagram is illustrated in figure 3.3 below.

![Last Response Happy Path](/assets/last%20resort%20%20happy%20path.png)

### Table 6.6: CustVendDetail

This is the customer details as supplied by the prepaid electricity service provider: Name, address and contact number.

| Argument     | Type   | Description                                      |
|--------------|--------|--------------------------------------------------|
| name         | string | Mandatory: Name of Meter owner.                  |
| meterNumber  | string | Mandatory: This is the customer meter number.    |
| amtTendered  | int    | Mandatory: This is the purchase amount. (Minimum P1, and Maximum P5,000) These limits can be adjusted to the merchant’s requirements. |

### TrialCreditVend Examples

The example request and response demonstrate a TrialCreditVend Request call and response.

#### Example Request

This example uses the following fields and values:

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic base64string");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
    "meterNumber": "04040404040",
    "transactionAmount": 10,
    "terminalId": "Web",
    "clientSaleId": "1639124683",
    "outletId": "4958-01",
    "operatorId": "finclude-01"
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

    #### Example Response

    {
    "code": "0",
    "custVendDetail": {
        "name": "GODD-INVESTMENTS (PTY) LTD",
        "meterNumber": "04040404040",
        "amtTendered": "10"
    },
    "clientSaleId": "4958-01",
    "transactionAmount": "10",
    "response": "Successful"
}


 #### Example Fault
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


 7.1.2 CreditVend Request

The CreditVend request carries out the actual purchase based on the positive outcome of the TrialCreditVend or when used as a stand-alone. The method returns a prepaid electricity voucher.

### Send a CreditVend Request

Send a CreditVend request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/electricity/creditvend

The CreditVend Request requires the following fields:

| Argument         | Type   | Description                                                                                                      |
|------------------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey           | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header (Base 64 encoded) as part of Basic Authentication scheme. |
| password         | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |
| terminalId       | string | **Optional:** This is the unique Identifier of sale channel making the purchase                                   |
| clientSaleId     | string | **Mandatory:** This is the POS sale/transaction Id that uniquely identifies sale.                                 |
| meterNumber      | string | **Mandatory:** This is the customer meter number.                                                                |
| transactionAmount| int    | **Mandatory:** This is the purchase amount. Supplied by Cashier/Operator (Minimum P1)                             |
| outletId         | string | **Mandatory:** This is the unique Identifier of store or sale channel the purchase is being made from            |
| operatorId       | string | **Mandatory:** This is a unique Identifier of person carrying out customer request                               |

### Making the CreditVend Request

The CreditVendRequest call is successful when you receive a response that has the customer’s prepaid electricity voucher. If the call fails, you will receive one of the following errors:

| Error No | Short Message               | Description                                                                                       |
|----------|-----------------------------|---------------------------------------------------------------------------------------------------|
| 01       | API key missing             | API Key not provided.                                                                             |
| 02       | Invalid API key             | Unable to authenticate supplied API key.                                                          |
| 03       | Invalid API Password        | Unable to authenticate supplied password.                                                         |
| 04       | Disabled API key            | The provided API Key is disabled                                                                  |
| 06       | Unexpected Error            | An unexpected exception has occurred.                                                             |
| 11       | Authorisation failed        | Authorisation Header is not provided                                                              |
| 102      | clientSaleId missing        | Missing a mandatory field - provider                                                              |
| 103      | outletId missing            | Missing a mandatory field - provider                                                              |
| 104      | Terminal Id missing         | Terminal Id is not provided.                                                                      |
| 105      | operatorId missing          | Missing a mandatory field - provider                                                              |
| 106      | meterNumber missing         | Meter number is not provided.                                                                     |
| 107      | transactionAmount missing   | Transaction purchase amount is not provided.                                                      |
| 202      | Insufficient Balance        | Merchant balance is less than sale amount.                                                        |
| 203      | Vendor Error                | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: BPC, Mascom, Orange – Etc) |
| 204      | Disabled Outlet             | The provided OutletId is currently disabled.                                                      |
| 205      | Invalid Outlet Id           | The provided OutletId was not found.                                                              |
| 206      | MinimumMaximum amount error | Amount requested is either less or more than the allowed amount.                                   |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

### CreditVend Response Parameters

| Argument           | Type    | Description                                                                                       |
|--------------------|---------|---------------------------------------------------------------------------------------------------|
| code               | string  | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed. |
| meterNumber        | string  | **Mandatory:** This is the customer meter number.                                                 |
| transactionAmount  | int     | **Mandatory:** This is the purchase amount.                                                       |
| response           | element | **Mandatory:** This is response of from sale, whether successful of failed                        |
| transactionId      | string  | **Mandatory:** This is the PrepaidPlus transaction ID that uniquely identifies sale.              |
| creditVendReceipt  | element | **Mandatory:** This is the service providers (Botswana Power Corporation – BPC) receipt details for the transaction. |
| clientSaleId       | string  | **Mandatory:** This is the Merchant POS sale/transaction Id that uniquely identifies transaction. Supplied by Merchant. |

### Table 6.10: Credit Vend Receipt

| Argument         | Type    | Description                                                                                       |
|------------------|---------|---------------------------------------------------------------------------------------------------|
| receiptNo        | string  | **Mandatory:** Attribute: This is the BPC receipt number.                                         |
| msno             | string  | **Mandatory:** This is the customer meter number.                                                 |
| vatNo            | string  | **Mandatory:** BPC – VAT Number                                                                   |
| tt               | string  | **Mandatory:** BPC - Token technology                                                             |
| at               | string  | **Mandatory:** BPC – Algorithm Type                                                               |
| sgc              | string  | **Mandatory:** BPC – Supply group code                                                            |
| krn              | string  | **Mandatory:** BPC - Key revision number.                                                         |
| ti               | string  | **Mandatory:** BPC - Tariff Index                                                                 |
| desc             | string  | **Mandatory:** BPC – Transaction description.                                                     |
| tokenUnits       | float   | **Mandatory:** BPC – Total units purchased                                                        |
| amtTendered      | float   | **Mandatory:** BPC – Amount tendered to purchase token                                            |
| costUnits        | float   | **Mandatory:** BPC – Cost of units. (Tendered amount – units)                                     |
| govLevy          | float   | **Mandatory:** BPC – Tax - Government Levy (retained)                                             |
| standardCharge   | float   | **Mandatory:** BPC – Tax – Standard charge (retained)                                             |
| VAT              | float   | **Mandatory:** BPC – VAT retained                                                                 |
| stsCipher        | string  | **Mandatory:** BPC – Customer recharge token                                                      |
| keychangetoken1  |         | **Optional:** BPC – Key change token which must be provided to the customer if a value is returned and should always be displayed above keychangetoken2 and stsCipher |
| keychangetoken2  |         | **Optional:** BPC – Key change token which must be provided to the customer if a value is returned and should always be displayed above stsCipher and below keychangetoken1 |

### CreditVend Request Examples

The example request and response demonstrate a CreditVend Request call and response.

#### Example Request

This example uses the following fields and values:

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic base64string");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
    "meterNumber": "04040404040",
    "transactionAmount": 10,
    "terminalId": "Web",
    "clientSaleId": "16949595959",
    "outletId": "4958-01",
    "operatorId": "finclude-01",
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