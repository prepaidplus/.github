#  Merchant Balance

The Use Case and workflow are presented for each task. A use case is defined as a single task, performed by the end user of the system, which has some useful outcome to the end user.

It must be noted that the use cases and workflows are only meant to guide those involved in the development of the interface and are in no way meant to be a technical definition of how the interface will work. The technical definition of the interface is presented at Methods.

Merchant Balance use cases are used to describe the functionality exposed by the Merchant Balance API. Each use case is aimed at complying with specific requirements of the following use case actors:

- Customers
- Vendors
- PWS client (PrepaidPlus Server)
- Merchant Balance Server (Service Provider’s Server)

#### Download Postman Collection
You can download the Postman collection for this use case <a href="/assets/merchantBalancePostmanCollection" download>here</a>. <!-- Replace with the actual path to your Postman collection file -->

### Use Case Actors, Responsibilities and Collaborators

Table describes use case actors, their responsibilities and collaborators:

| Use case actor | Responsibilities                                                                 | Collaborators                           |
|----------------|----------------------------------------------------------------------------------|-----------------------------------------|
| Customer       | - Initiates balance inquiry use cases.                                           | - Vending Operator                      |
|                | - Provides necessary authentication information.                                 |                                         |
|                | - Receives balance information.                                                  |                                         |
| Vending Operator (Merchant) | - Verifies customer information.                                     | - PrepaidPlus Server                    |
|                | - Submits balance inquiry requests on behalf of customer.                        |                                         |
|                | - Performs operator specific tasks.                                              |                                         |
|                | - Hands balance information to customers.                                        |                                         |
|                | - Implements the “LastResponse” use case for use cases that might require this service. |                                         |
|                | - Handles customer queries.                                                      |                                         |
| PrepaidPlus Server | - Authenticates the Merchant Balance Server.                                  | - Merchant Balance Service Provider’s server |
|                | - Compiles and sends balance inquiry request messages to Merchant Balance server. |                                         |
|                | - Receives and formats balance inquiry response messages from the Merchant Balance server. |                                         |
|                | - Automatically initiates the “LastResponse” use case for use cases that might require this service. |                                         |
| Merchant Balance Service Provider’s server | - Authenticates the Merchant Balance clients.                          | - PrepaidPlus Server                    |
|                | - Complies with an appropriate balance inquiry response message based on its application business logic. |                                         |
|                | - Responds with a fault response message, if required.                           |                                         |

### Merchant Balance Methods

**Table: Supported Merchant Balance Methods**

| Operation              | Description |
|------------------------|-------------|
| GetMerchantBalance Request | Queries the merchant’s account balance at a point in time. The balance is updated every 60 seconds and includes the last deposit date and amount. |

## Get Merchant Balance Use Case

#### Get Merchant Balance Description

The Get Merchant Balance use case is used to query the merchant’s account balance at a point in time. The balance is updated every 60 seconds and includes the last deposit date and amount.

#### Get Merchant Balance Desired Outcome

The Vendor wants to retrieve the current balance of the merchant’s account, including the last deposit date and amount.

#### Get Merchant Balance Preconditions

- The API key and password shall be supplied.

#### Get Merchant Balance Participants

- The customer
- The Vendor/ Merchant
- PrepaidPlus Server
- Merchant Balance Server

#### Get Merchant Balance Key Business Rules

- The balance is not a live balance, but is updated every 60 seconds.
- The last deposit date is in ISO Date format, which does not include a time zone offset.

#### Get Merchant Balance Workflow

The workflow for the Get Merchant Balance process is as presented below in Table.

| Step No: | Client                                                                 | PrepaidPlus Server                                                                 |
|----------|------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| 1        | Customer submits a balance inquiry request.                            |                                                                                    |
| 2        | The Merchant (Vending Client) makes a valid and complete API call for balance inquiry. This call should also provide the API key of the merchant for authentication. |                                                                                    |
| 3        |                                                                        | PrepaidPlus validates the merchant API key. If valid, the transaction proceeds to subsequent steps, or else returns an API error. PrepaidPlus sends a request to the Merchant Balance server. |
| 4        | If the Merchant receives a successful response from PrepaidPlus, the balance information is then confirmed and provided to the customer. |                                                                      

#### Get Merchant Balance Happy Path 

The Get Merchant Balance use case sequence diagram is illustrated below.


### Merchant Balance Methods

This method is used to query the merchant’s account balance at a point in time. The following should be noted regarding the merchant balance:
- The merchant balance is not a live balance, but is updated every 60 seconds.
- The merchant balance shows the last updated balance and also includes the last deposit date and amount made into the merchant’s account. If the merchant has not made any deposit, the last deposit object will be null.
- The last deposit date is in ISO Date format, which does not include a time zone offset.

**Send a GetMerchantBalance Request Call**

Send a GetMerchantBalance request to one of the following endpoints:
- **Production:** Will be advised once testing and integration on development server completed
- **Sandbox:** [https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/merchant/getbalance](https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/merchant/getbalance)

The GetMerchantBalance call requires the following fields:

**Table: Get Merchant Balance call requirements**

| Argument | Type   | Description                                                                                                      |
|----------|--------|------------------------------------------------------------------------------------------------------------------|
| apiKey   | string | **Mandatory:** Secret APIKey authenticates the merchant. Passed in authorization header as part of Basic Authentication scheme. |
| password | string | **Mandatory:** Merchant password. Passed in authorization header as part of Basic Authentication scheme.         |

**Making the GetMerchantBalance Request**

The GetMerchantBalance Request call is successful when you receive the balance confirmation response. If the call fails, you will receive one of the following faults/errors:

**Table: GetMerchantBalance Request Errors**

| Error No | Short Message            | Description                                                                 |
|----------|--------------------------|-----------------------------------------------------------------------------|
| 01       | API key missing          | API Key not provided.                                                       |
| 02       | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03       | Invalid API Password     | Unable to authenticate supplied password.                                   |
| 04       | Disabled API key         | The provided API Key is disabled                                            |
| 06       | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11       | Authorisation failed     | Authorisation Header is not provided                                        |

The errors above are faults, which are fatal, call-level errors. When a fault occurs, nothing is processed because the entire call was invalid. Often the problem is that the API key was incorrect or there was a server error.

**GetMerchantBalance Response Parameters**

**Table : GetMerchantBalance Response parameters**

| Argument     | Type   | Description                                                                                                      |
|--------------|--------|------------------------------------------------------------------------------------------------------------------|
| code         | string | **Mandatory:** Response code indicating the outcome of the transaction, whether successful or failed.            |
| balance      | float  | **Mandatory:** Merchant's account balance                                                                        |
| lastDeposit  | element| **Mandatory:** The last deposit transaction made to hydrate the merchant’s account                                |
| response     | string | **Mandatory:** This is the response whether successful or failed                                                 |

**Table: lastDeposit**

These are details of the last deposit made in the merchant’s account.

| Argument | Type   | Description                                                                                                      |
|----------|--------|------------------------------------------------------------------------------------------------------------------|
| date     | string | **Mandatory:** Date and time deposit was made.                                                                   |
| channel  | string | **Mandatory:** This is the deposit channel used (Cash, EFT…)                                                     |
| amount   | int    | **Mandatory:** This is the deposit amount.                                                                       |

**GetMerchantBalance Request Examples**

The example request and response demonstrate a GetMerchantBalance Request call and response.

**Example Request**

This example uses the following fields and values:
(Note that the request has no body)

**Example Request**

This example uses the following fields and values: 
(Note that the request has no body)

````javascript

var myHeaders = new Headers();
myHeaders.append("Authorization", "Basic  {{ base64string }}");// Replace with your base64 encoded string
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({});

var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: raw,
  redirect: 'follow'
};

fetch("https://tps.prepaidplus.co.bw/apimanager/rest/basic/v1/merchant/getbalance", requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));
````

**Example Response**

````javascript
{
"code": "0",
"balance": 26798.592500000013,
"lastDeposit": {
"date": "YYYY-MM-DDT[hh][mm][ss]"
"channel": "EFT",
"amount": 20000
}
"response": "Successful"
}
````

This response would close off a successful GetMerchantBalance request.
**Example Fault**

````javascript
{

      "code": "01"
    	"response": "Failure",
    	"message": "API key missing.",
    	"description": " API Key not provided "
}
````
