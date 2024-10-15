# PrepaidPlus Web Services

## Introduction to PrepaidPlus Web Services

The PrepaidPlus API is organized around REST. It provides a layer for the exchange of Value Added Services data between the cloud-based PrepaidPlus Vending system and external vendors that vend through PrepaidPlus. These programs include partnered POS systems, PrepaidPlus in-house applications that don't need direct database access, and third-party applications indirectly supported through PrepaidPlus.

The API has predictable resource-oriented URLs, accepts JSON-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

The PrepaidPlus API differs for every account as we release new versions and tailor functionality. Future documents will be provided online and will be customized to your version of the API, with your test key and data.

## Technology

PrepaidPlus Web Services (PWS) is based on REST. The following standards are observed:

| Standard Name | Web Site Reference |
|---------------|--------------------|
| REST          | [Learn More](http://en.wikipedia.org/wiki/Representational_State_Transfer) |

Since PrepaidPlus Web Services are database-driven, it is important that the information you collect from your users is compatible with the fields in the database. We provide you with the list of commands and data types that are meaningful to the PrepaidPlus database. Through the use of commands and data types provided by the API, and by limiting the values that can be stored in each data type, your data will always be consistent with, and able to be stored within the PrepaidPlus database.

## Target Audience

This document is intended for developers of applications that will connect to the PrepaidPlus Web service.

## Overview

The following provides a brief description of what is included in PWS:

- **Getting Started** — Steps for what you should do to begin using PWS. It includes general procedures for connecting to the PrepaidPlus API.
- **Use Cases and Workflows** — Provides a guide on how an end user performs a specific task on the system, which has a particular desired outcome.
- **Methods** — Lists the supported calls in the API and the syntax, use, arguments, and response of each call.
- **Code Examples** — Provides code examples for specific functions.
- **Error Code Listing** — Identifies common errors and associated codes.

## Definitions

The following are definitions used in PrepaidPlus Web Services:

| Item              | Definition                                                                                   |
|-------------------|----------------------------------------------------------------------------------------------|
| API               | Application Program Interface                                                                |
| Vending Operator  | The merchant and their vending application that talks to the PrepaidPlus Server using the API |
| PrepaidPlus Server| The PrepaidPlus site that understands the API                                                |
| PWS               | PrepaidPlus Web Service                                                                      |
| RESTful API       | Also referred to as a RESTful web service or REST API — is based on representational state transfer (REST) technology, an architectural style and approach to communications often used in web services development. |

### Date Format

All date fields in PWS use the following date format: YYYY-MM-DD HH:MM:SS

## Getting Started

The following are instructions for what you should do to begin using PrepaidPlus Web Services.

PWS authenticates your API requests using your account’s API key and password. If you do not include your key when making an API request, or use one that is incorrect or outdated, PWS returns an error. Every account is provided with separate keys for testing and for running live transactions. All API requests exist in either development or production environment, and objects—keys in one environment cannot be manipulated by objects in the other.

Production API keys should be kept confidential and only stored on your own servers. Your account’s secret API key can perform any API request to PWS without restriction.

### How to Begin

#### Obtain PrepaidPlus API Access

Contact the PrepaidPlus Support Department to request API access. When access is granted, you will receive an API namespace, an API key, and password. These are the pieces of information required for API access. The API key and password should be Base64 Encoded in an auth object as shown in Table  below. The resulting Base64 string should be sent in the Authorization header of every request.

| API Point  | API Key             | Service information (generic domain)       |
|------------|---------------------|--------------------------------------------|
| Development| {{ Your Development Key }} |             |
| Production | {{ Your Production Key }} |                                            |

| Type       | Value               |                                                                 |
|------------|---------------------|-----------------------------------------------------------------|
| ApiKey     | {{ api_key }}      | Base64Encode ( {{_api_key }} : {{ password }} )|
| Password   | {{ password }}     |                                                                 |
| Base64     |  {{ base64string }}|                                                                 |

**Note:** Colon between `apiKey` and `password` is important.

## SERVICES 
<table>
  <tr>
    <td>
      <a href="/documentation/electricity.md">
        <kbd> <br> ELECTRICITY <br> </kbd>
      </a>
    </td>
    <td>
      <a href="/documentation/multichoice.md">
        <kbd> <br> MULTICHOICE <br> </kbd>
      </a>
    </td>
    <td>
      <a href="/documentation/airtime.md">
        <kbd> <br> AIRTIME <br> </kbd>
      </a>
    </td>
     <td>
      <a href="/documentation/merchantBalance.md">
        <kbd> <br> MERCHANT BALANCE <br> </kbd>
      </a>
    </td>
     <td>
      <a href="/documentation/faultsAndErrors.md">
        <kbd> <br> FAULTS & ERRORS  <br> </kbd>
      </a>
    </td>
  </tr>
</table>
