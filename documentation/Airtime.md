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

## PWS Prepaid Airtime Use Cases and Workflows

PWS Prepaid Airtime use cases are used to describe the functionality exposed by the Prepaid Airtime API. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers
- Vendors
- PWS client (PrepaidPlus Server)
- Airtime Vending Server (Service Provider’s Server)

### Use Case Actors, Responsibilities and Collaborators

Table 5.1 describes use case actors, their responsibilities and collaborators:

| Use case actor | Responsibilities                                                                 | Collaborators                           |
|----------------|----------------------------------------------------------------------------------|-----------------------------------------|
| Customer       | - Initiates customer use cases.                                                  | - Vending Operator                      |
|                | - Tenders payment.                                                               |                                         |
|                | - Receives an airtime recharge voucher.                                          |                                         |
| Vending Operator | - Submits vending requests on behalf of customer.                               | - PrepaidPlus Server                    |
|                | - Performs operator specific tasks.                                              |                                         |
|                | - Delivers airtime recharge voucher to customers.                                |                                         |
|                | - Handles customer queries.                                                      |                                         |
| PrepaidPlus Server | - Authenticates the Prepaid Airtime Server.                                    | - Prepaid Airtime Server                |
|                | - Compiles and sends Prepaid Airtime request messages to the Prepaid Airtime server. |                                         |
|                | - Receives and formats Prepaid Airtime response messages from the Prepaid Airtime server. |                                         |
| Prepaid Airtime Server | - Authenticates the Prepaid Airtime clients.                              | - Prepaid Airtime clients               |
|                | - Complies with an appropriate Prepaid Airtime response message based on its application business logic. |                                         |
|                | - Responds with a fault response message, if required.                           |                                         |

### Prepaid Airtime Credit Request Use Case

#### Prepaid Airtime Credit Request Description

The Prepaid Airtime Credit Request use case is used to purchase a pin recharge of a defined amount for a specific mobile network operator.

#### Prepaid Airtime Credit Request Desired Outcome

The customer specifies the details of the pin recharge they require and pays for it (mobile network operator and amount).

#### Prepaid Airtime Credit Request Preconditions

- The mobile network operator shall be supplied.
- The amount shall be supplied.

#### Prepaid Airtime Credit Request Participants

- The customer
- The Vendor
- PrepaidPlus Server
- Prepaid Airtime server

#### Prepaid Airtime Credit Request Workflow

The workflow for the prepaid airtime credit request process is as presented below.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | The cashier collects payment from the customer, and selects the desired Mobile Network Operator for which a recharge voucher is required. Thereafter, the cashier selects and submits the required recharge value. NB: Once the request is confirmed with PrepaidPlus, the sale cannot be rolled back. |                                                                                    |
| 2        |                                                                        | PrepaidPlus receives confirmation of the prepaid airtime credit request from the merchant. PrepaidPlus validates the merchant API Key and available balance. |
| 3        |                                                                        | The transaction is processed on the prepaid airtime server, and a prepaid airtime PIN recharge voucher is dispatched once the transaction is successfully processed. |
| 4        | The merchant receives and delivers the prepaid airtime recharge voucher details to the customer. |                                                                                    |

#### Prepaid Airtime Credit Request Happy Path

The prepaid airtime credit request use case sequence diagram is illustrated below.

![Prepaid Airtime Credit Request Happy Path](./Airtime%20Happy%20Path.png)

## Methods

This section details the supported calls in the API. Either one of the following store codes are to be used in the calls in the test environment.

| Outlet Id | Outlet Name |
|-----------|-------------|
| 4958-01   | Finclude    |

### Prepaid Electricity Methods

| Operation             | Description                                                                                                                                                                                                 |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| TrialCreditVend Request | Verifies the meter number and if the customer account is in good standing, i.e not blocked or suspended. It also ensures there is enough credit/ balance for the transaction to be a success. The TrialCreditVend returns the customer details and a proceed or fail indication. |
| CreditVend Request    | Confirms purchase of a prepaid electricity voucher. It can be used in concert with the positive outcome of the TrialCreditVend or on its own. The method returns a prepaid electricity voucher.              |
| LastResponse Request  | This method is called subsequent to an ongoing CreditVend Request network timeout/connection failure or an exception. Its purpose is to check if a voucher had been successfully sold prior to abandoning the sale. In an event that the failed CreditVend Request had resulted in a successful sale the voucher is retrieved and returned for printing, otherwise the sale is abandoned. |
</body>
</html>