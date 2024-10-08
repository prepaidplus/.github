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

</body>
</html>