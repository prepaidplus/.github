## Overview of API Documentation

This document provides detailed instructions and information on how to use the PrepaidPlus Web Services (PWS). It includes sections on getting started, use cases, workflows, and more.

### Navigation
 [Getting Started](#getting-started)
  - [How to Begin](#how-to-begin)
    - [Obtain PrepaidPlus API Access](#obtain-prepaidplus-api-access)
- [PWS Prepaid Electricity Use Cases and Workflows](#pws-prepaid-electricity-use-cases-and-workflows)
  - [Use Case Actors, Responsibilities and Collaborators](#use-case-actors-responsibilities-and-collaborators)
  - [Trial Credit Vend Use Case](#trial-credit-vend-use-case)
  - [Credit Vend Happy Path](#credit-vend-happy-path)
  - [Last Response Use Case](#last-response-use-case)
- [PWS MultiChoice DSTV Use Cases and Workflows](#pws-multichoice-dstv-use-cases-and-workflows)
  - [Multichoice DSTV Smartcard Confirmation Use Case](#multichoice-dstv-smartcard-confirmation-use-case)


## Getting Started

The following are instructions for what you should do to begin using PrepaidPlus Web Services.

PWS authenticates your API requests using your account’s API key and password. If you do not include your key when making an API request, or use one that is incorrect or outdated, PWS returns an error. Every account is provided with separate keys for testing and for running live transactions. All API requests exist in either development or production environment, and objects—keys in one environment cannot be manipulated by objects in the other.

Production API keys should be kept confidential and only stored on your own servers. Your account’s secret API key can perform any API request to PWS without restriction.

### How to Begin

#### Obtain PrepaidPlus API Access

Contact the PrepaidPlus Support Department to request API access. When access is granted, you will receive an API namespace, an API key and password. These are the pieces of information required for API access. The API key and password should be Base64 Encoded in an auth object as shown in Table 2.2 below. The resulting Base64 string should be sent in the Authorization header of every request.

| API Point  | API Key            | Service information (generic domain)       |
|------------|--------------------|--------------------------------------------|
| Development| GzVAeL9yDudeaNUZFITz | https://tps.prepaidplus.co.bw/             |
| Production | TBA                |                                            |

| Type       | Value              |
|------------|--------------------|
| ApiKey     | GzVAeL9yDudeaNUZFITz | Base64Encode(api_key:password) i.e Base64Encode(GzVAeL9yDudeaNUZFITz:Yjd83&j$%d) |
| Password   | Yjd83&j$%d         |
| Base64     | R3pWQWVMOXlEdWRlYU5VWkZJVHo6WWpkODMmaiQlZA== |

Note: Semicolon between apiKey and password is important

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

![Last Response Happy Path](../documentation/last%20resort%20%20happy%20path.png)
