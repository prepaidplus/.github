## Overview of README.md

This README file provides comprehensive documentation for the PrepaidPlus API, including its key features, technology, target audience, and various use cases and workflows. The document is structured to guide developers through the process of integrating with the PrepaidPlus Web Services (PWS) and includes detailed descriptions of the API's functionality, supported methods, and error codes.

### Navigation

-[PrepaidPlus API Documentation](#prepaidplus-api-documentation)
  - [Key Features](#key-features)
  - [Technology](#technology)
  - [Target Audience](#target-audience)
  - [Overview](#overview)
  - [Definitions](#definitions)
  - [Date Format](#date-format)
-  [API Methods](documentation\api.md)

## PrepaidPlus API Documentation

The PrepaidPlus API is organized around REST. It provides a layer for the exchange of Value Added Services data between the cloud-based PrepaidPlus Vending system and external vendors that vend through PrepaidPlus. These programs include:

- Partnered POS systems
- PrepaidPlus in-house applications that don't need direct database access
- Third-party applications indirectly supported through PrepaidPlus

### Key Features

- **Resource-oriented URLs**: The API has predictable resource-oriented URLs.
- **JSON Support**: Accepts JSON-encoded request bodies and returns JSON-encoded responses.
- **Standard HTTP Practices**: Uses standard HTTP response codes.

### Technology

PrepaidPlus Web Services (PWS) is based on REST. The following standards are observed:

| Standard Name | Web Site Reference |
|---------------|---------------------|
| REST          | [Representational State Transfer](http://en.wikipedia.org/wiki/Representational_State_Transfer) |

Since PrepaidPlus Web Services are database-driven, it is important that the information you collect from your users is compatible with the fields in the database. We provide you with the list of commands and data types that are meaningful to the PrepaidPlus database. Through the use of commands and data types provided by the API, and by limiting the values that can be stored in each data type, your data will always be consistent with, and able to be stored within the PrepaidPlus database.

### Target Audience

This document is intended for developers of applications that will connect to the PrepaidPlus Web service.

### Overview

The following provides a brief description of what is included in PWS:

- **Getting Started** — includes steps for what you should do to begin using PWS. It includes general procedures for connecting to the PrepaidPlus API.
- **Use Cases and Workflows** – Provides a guide of how an end user performs a specific task on the system, which has a particular desired outcome.
- **Methods** — lists the supported calls in the API and the syntax, use, arguments, and response of each call.
- **Code Examples** — provides code examples for specific functions.
- **Error Code Listing** — identifies common errors and associated codes.

### Definitions

The following are definitions used in PrepaidPlus Web Services:

| Item              | Definition                                                                 |
|-------------------|-----------------------------------------------------------------------------|
| API               | Application Program Interface                                               |
| Vending Operator  | The merchant and their vending application that talks to the PrepaidPlus Server using the API |
| PrepaidPlus Server| The PrepaidPlus site that understands the API                               |
| PWS               | PrepaidPlus Web Service                                                     |
| RESTful API       | Also referred to as a RESTful web service or REST API -- is based on representational state transfer (REST) technology, an architectural style and approach to communications often used in web services development. |

### Date Format

All date fields in PWS use the following date format: YYYY-MM-DD HH:MM:SS

## Getting Started

The following are instructions for what you should do to begin using PrepaidPlus Web Services.

PWS authenticates your API requests using your account’s API key and password. If you do not include your key when making an API request, or use one that is incorrect or outdated, PWS returns an error. Every account is provided with separate keys for testing and for running live transactions. All API requests exist in either development or production environment, and objects—keys in one environment cannot be manipulated by objects in the other.

Production API keys should be kept confidential and only stored on your own servers. Your account’s secret API key can perform any API request to PWS without restriction.
