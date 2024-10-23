# Faults & Errors

**Overview**

A fault is a fatal, call-level error. Faults differ from errors, which cause only part of a call to fail. When a fault occurs, the call fails completely and nothing is processed. Examples of faults include an incorrect/malformed request, invalid authentication, and a server error.

**Table : Successful transaction response**

| Error Code | Short Message | Description            |
|------------|---------------|------------------------|
| 0          | Successful    | Request was successful.|

**Table: System Errors**

| Error Code | Short Message            | Description                                                                 |
|------------|--------------------------|-----------------------------------------------------------------------------|
| 01         | API key missing          | API Key not provided.                                                       |
| 02         | Invalid API key          | Unable to authenticate supplied API key.                                    |
| 03         | Disabled API key         | The provided API Key is disabled                                            |
| 04         | Invalid Credentials      | Unable to authenticate supplied Authorisation values.                       |
| 06         | Unexpected Error         | An unexpected exception has occurred.                                       |
| 11         | Authorisation failed     | Authorisation Header is not provided                                        |

**Table: Mandatory Fields**

| Error Code | Short Message                | Description                                                                 |
|------------|------------------------------|-----------------------------------------------------------------------------|
| 102        | ClientSaleId is missing      | clientSaleId is not provided.                                               |
| 103        | OutletId is missing          | outletId is not provided                                                    |
| 104        | Terminal Id missing          | Terminal Id is not provided.                                                |
| 105        | OperatorId missing           | operatorId is not provided.                                                 |
| 106        | meterNumber missing          | Meter number is not provided.                                               |
| 107        | transactionAmount missing    | Transaction purchase amount is not provided.                                |
| 108        | Provider missing             | Missing a mandatory field - provider                                        |
| 109        | Denomination missing         | Missing a mandatory field - denomination                                    |
| 110        | SmartcardNumber missing      | Missing a mandatory field - smartcardNumber                                 |
| 111        | lookupId missing             | Missing a mandatory field - lookupId                                        |

**Table: Business Case Exceptions**

| Error Code | Short Message                | Description                                                                 |
|------------|------------------------------|-----------------------------------------------------------------------------|
| 201        | TransactionAmount not numeric| Purchase amount must be numeric.                                            |
| 202        | Insufficient Balance         | Merchant balance is less than sale amount.                                  |
| 203        | Vendor Error                 | Pass-on Business Error returned by Vendor. Description in the details field. (Vendors: BPC, Mascom, Orange – Etc) |
| 204        | Disabled Outlet              | The provided OutletId is currently disabled.                                |
| 205        | Invalid Outlet Id            | The provided OutletId was not found.                                        |
| 206        | MinimumMaximum amount error  | Amount requested is either less or more than the allowed amount.            |
| 208        | Invalid clientSaleId         | Could not find a transaction for the supplied client sale transaction Id.   |
| 209        | Sale not found               | The server did not process the requested transaction. No sale was made.     |
| 210        | Invalid outletId             | outletId provided was not found under merchant.                             |
| 211        | Airtime Vending Denied       | Merchant not setup to sell Airtime.                                         |
| 212        | No Vouchers to Sell          | PrepaidPlus has run out of vouchers of the selected denomination.           |