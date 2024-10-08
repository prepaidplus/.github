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
 
 # PrepaidPlus
 
## Introduction to PrepaidPlus Web Services

The PrepaidPlus API is organised around REST. It provides a layer for the exchange of Value Added Services data between the cloud-based PrepaidPlus Vending system and external vendors that vend through PrepaidPlus. These programs include partnered POS systems, PrepaidPlus in-house applications that don't need direct database access, and third-party applications indirectly supported through PrepaidPlus.

The API has predictable resource-oriented URLs, accepts JSON-encoded request bodies, returns JSON-encoded responses, and uses standard HTTP response codes, authentication, and verbs.

The PrepaidPlus API differs for every account as we release new versions and tailor functionality. Future documents will be provided online and will be customised to your version of the API, with your test key and data.

### Technology

PrepaidPlus Web Services (PWS) is based on REST. The following standards are observed:

| Standard Name | Web Site Reference |
|---------------|--------------------|
| REST          | [Learn More](http://en.wikipedia.org/wiki/Representational_State_Transfer) |

Since PrepaidPlus Web Services are database-driven, it is important that the information you collect from your users is compatible with the fields in the database. We provide you with the list of commands and data types that are meaningful to the PrepaidPlus database. Through the use of commands and data types provided by the API, and by limiting the values that can be stored in each data type, your data will always be consistent with, and able to be stored within the PrepaidPlus database.

### Target Audience

This document is intended for developers of applications that will connect to the PrepaidPlus Web service.

### Overview

The following provides a brief description of what is included in PWS.

- **Getting Started** — includes steps for what you should do to begin using PWS. It includes general procedures for connecting to the PrepaidPlus API.
- **Use Cases and Workflows** – Provides a guide of how an end user performs a specific task on the system, which has a particular desired outcome.
- **Methods** — lists the supported calls in the API and the syntax, use, arguments, and response of each call.
- **Code Examples** — provides code examples for specific functions.
- **Error Code Listing** — identifies common errors and associated codes.

### Definitions

The following are definitions used in PrepaidPlus Web Services.

| Item                | Definition                                                                                   |
|---------------------|----------------------------------------------------------------------------------------------|
| API                 | Application Program Interface                                                                |
| Vending Operator    | The merchant and their vending application that talks to the PrepaidPlus Server using the API |
| PrepaidPlus Server  | The PrepaidPlus site that understands the API                                                |
| PWS                 | PrepaidPlus Web Service                                                                      |
| RESTful API         | Also referred to as a RESTful web service or REST API -- is based on representational state transfer (REST) technology, an architectural style and approach to communications often used in web services development. |

### Date Format

All date fields in PWS use the following date format: YYYY-MM-DD HH:MM:SS

## Getting Started

The following are instructions for what you should do to begin using PrepaidPlus Web Services.

PWS authenticates your API requests using your account’s API key and password. If you do not include your key when making an API request, or use one that is incorrect or outdated, PWS returns an error. Every account is provided with separate keys for testing and for running live transactions. All API requests exist in either development or production environment, and objects—keys in one environment cannot be manipulated by objects in the other.

Production API keys should be kept confidential and only stored on your own servers. Your account’s secret API key can perform any API request to PWS without restriction.

### How to Begin

#### Obtain PrepaidPlus API Access

Contact the PrepaidPlus Support Department to request API access. When access is granted, you will receive an API namespace, an API key and password. These are the pieces of information required for API access. The API key and password should be Base64 Encoded in an auth object as shown in Table 2.2 below. The resulting Base64 string should be sent in the Authorisation header of every request.

| API Point  | API Key             | Service information (generic domain)       |
|------------|---------------------|--------------------------------------------|
| Development| [Your Development Key] | https://tps.prepaidplus.co.bw/             |
| Production | [Your Production Key] |                                            |

| Type       | Value               |                                                                 |
|------------|---------------------|-----------------------------------------------------------------|
| ApiKey     | [Your API Key]      | Base64Encode (_key:password } i.e Base64Encode (api_key:password) |
| Password   | [Your Password]     |                                                                 |
| Base64     | [Your Base64 String]|                                                                 |

Note: Semicolon between apiKey and password is important

## Use Cases and Workflows

This document outlines various use cases, workflows, and customer confirmation processes across multiple utility services, such as Water Utilities, Prepaid Electricity, MultiChoice (DSTV), and Prepaid Airtime. Buttons will direct users to different sections for easy access to the relevant information. The details provided include necessary preconditions, desired outcomes, and participants in each process.

### Water Utilities Cooperation Customer Confirmation

The WUC Customer Confirmation verifies that the customer number and contract number are valid, active, and that the amount due is accurate.

**Desired Outcome:** The Vendor seeks confirmation that a payment can be raised for a particular customer and contract number. The WUC and PrepaidPlus servers validate the details, including customer name, plot, and due amount, returning a success or failure response.

**Preconditions:** Customer’s phone number, customer number, and contract number must be provided.

<a href="/documentation/WaterUtilities.md" class="button">WATER UTILITIES</a>

### Prepaid Electricity Use Cases and Workflows

The Prepaid Electricity section describes the processes involved in purchasing prepaid electricity via the Vend protocol. It outlines the functionality for customers, vendors, and the Vend system.

**Use Case Actors:**

- Customers: End users purchasing electricity.
- Vendors: Facilitate the electricity purchase.
- Vend Client: The application managing electricity purchases.
- Vend Server: The server processing transactions.

<a href="/documentation/api.md" class="button">PREPAID ELECTRICITY</a>

### MultiChoice DSTV Use Cases and Workflows

The DSTV use cases cover purchasing and managing DSTV services through the PWS Multichoice DSTV API. This section explains how customers, vendors, and the PWS client and MultiChoice servers interact.

**Use Case Actors:**

- Customers: Users purchasing DSTV subscriptions.
- Vendors: Manage the transactions for DSTV services.
- PWS Client: PrepaidPlus server handling communication.
- MultiChoice Server: Service provider's server managing DSTV accounts.

<a href="/documentation/Multichoice .md" class="button">MULTICHOICE</a>

### Prepaid Airtime Credit Request

The Prepaid Airtime section covers the workflow for purchasing mobile airtime pins for different mobile network operators.

**Desired Outcome:** The customer specifies the mobile network operator and the airtime amount they need, which is processed and paid for by the vendor.

**Preconditions:** Mobile network operator and amount must be supplied.

**Use Case Participants:**

- Customer: Purchases airtime.
- Vendor: Processes the request.
- PrepaidPlus Server: Validates and processes the purchase.
- Prepaid Airtime Server: Generates the airtime pin.

<a href="/documentation/Airtime.md" class="button">PREPAID AIRTIME</a>

</body>
</html>