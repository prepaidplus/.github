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



## PWS MultiChoice DSTV Use Cases and Workflows

PWS Multichoice DSTV use cases are used to describe the functionality exposed by the PWS Multichoice DSTV API. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers
- Vendors
- PWS client (PrepaidPlus Server)
- MultiChoice Server (Service Provider’s Server)

### Use Case Actors, Responsibilities and Collaborators

Table 4.1 describes use case actors, their responsibilities and collaborators:

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
| PrepaidPlus Server | - Authenticates the Multichoice Payment Server.                               | - Multichoice DSTV Server               |
|                | - Compiles and sends MultiChoice DSTV request messages to Multichoice payment server. |                                         |
|                | - Receives and formats MultiChoice DSTV response messages from the Multichoice payment server. |                                         |
| Multichoice DSTV Payment Server | - Authenticates the MultiChoice DSTV clients.                    | - MultiChoice DSTV clients              |
|                | - Complies with an appropriate MultiChoice DSTV response message based on its application business logic. |                                         |
|                | - Responds with a fault response message, if required.                           |                                         |

### Multichoice DSTV Smartcard Confirmation Use Case

#### Multichoice DSTV Smartcard Confirmation Description

The DSTV Smartcard Confirmation is used to confirm that the smartcard is valid and active, as well as confirming the subscription due date and amount.

#### DSTV Smartcard Confirmation Desired Outcome

The Vendor wants to confirm whether a payment can be raised successfully for a particular smartcard. The PrepaidPlus and Multichoice DSTV Payment servers perform all required validations and return confirmation of the smart card. These details will include smart card registered owner, and amount due. The PrepaidPlus server returns a positive, or failure response to the Vendor.

#### DSTV Smartcard Confirmation Preconditions

- The smartcard number shall be supplied.

#### DSTV Smartcard Confirmation Participants

- The customer
- The Vendor
- PrepaidPlus Server
- Multichoice DSTV payment server

#### DSTV Smartcard Confirmation Key Business Rules

- Smartcard exists and is active.
- Smartcard details match to ones the customer would like to transact for.
- Normal Vending related exception messages are implemented as they would be for standard vending operation. (i.e Validity of Smartcard number for payment)

#### DSTV Smartcard Confirmation Workflow

The workflow for the DSTV Smartcard Confirmation process is as presented below in Table 4.2.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | The cashier selects the DSTV Payments option, and submits the customer’s smartcard number. The Finclude POS will provide the following in addition to the information provided by the cashier; TerminalID, OperatorId, and ClientSaleID. |                                                                                    |
| 2        |                                                                        | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. If the API key is successfully validated, PrepaidPlus sends a request to Multichoice DSTV payment server with the customer smartcard number for validation. |
| 3        | Where Finclude POS receives a successful response from PrepaidPlus, the names, amount owed and the amount the customer would like to pay towards their account are then confirmed with the end customer. This response will have an entry box for the amount to be paid. |                                                                                    |

#### DSTV Smartcard Confirmation Happy Path

The DSTV Smartcard Confirmation Vend use case sequence diagram is illustrated below.

![DSTV Smartcard Confirmation Happy Path](../documentation/dstv%20Smart%20Confirmation%20Happy%20path.png)

### Multichoice DSTV Smartcard Payment Use Case

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

The workflow for the DSTV Smartcard Payment process is as presented below in Table 4.3.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | The cashier collects payment from the customer, and then captures the amount on the POS and submits the transaction details. The payment transaction details are transmitted to PrepaidPlus for processing. NB: Once Finclude POS confirms the payment with PrepaidPlus, the sale cannot be rolled back. |                                                                                    |
| 2        |                                                                        | PrepaidPlus receives confirmation of the payment from Finclude POS. PrepaidPlus validates the merchant API Key and checks the available balance in the Merchant account (i.e. within PrepaidPlus system) to ensure there is sufficient balance to meet the amount requested for purchase. If the balance is insufficient, an error message is returned. If the above check are positive, PrepaidPlus sends the payment request to the Multichoice DSTV payment server, and receives a confirmation once the payment is successfully processed. |
| 3        |                                                                        | PrepaidPlus sends the customer payment details with a reference number from Multichoice DSTV to Finclude POS. |
| 4        | Finclude POS prints the payment receipt details. |                                                                                    |

#### DSTV Smartcard Payment Happy Path

The DSTV Smartcard Payment use case sequence diagram is illustrated below.

![Smartcard Payment Happy Path](../documentation/smartcard%20happy%20path.png)

</body>
</html>