# Overview

The SIM Swap API provides a programmable interface for developers and other users (capabilities consumers) to request the last date of a SIM swap performed on the mobile line, or, to check whether a SIM swap has been performed during a past period.


## 1\. Introduction

The SIM Swap API performs real-time checks on the activation date of a SIM card on the mobile network. It reveals if an individual mobile phone number (MSISDN) has been ported to another SIM card.

The SIM Swap API is useful to prevent fraud by reducing the risk of account takeover fraud by strengthening SIM based authentication processes such as SMS One-time passwords. Fraudsters are using Simswap techniques to intercept SMS messages and reset passwords or receive verification codes that allow them to access protected accounts.

The SIM Swap API is used by an application to get information about a mobile line latest simswap date. It can be easily integrated and used through this secured API and 
allows SPs (Service Provider) to get this information an easy & secured way. The API provides management of 2 resources answering 2 distinct questions:
- When did the last SIM swap occur?
- Has a SIM swap occurred during last n hours?

Depending on the network provider implementation, legal enforcement, etc... either one or both resources could be implemented. 

__Note__:  SIM swap is covered as well in the Mobile Connect (MC) ATP (Account-Takeover-Protection) process. The MC pattern is exposed in another Camara API from the SIM Swap API family. Network providers are free to implement the MC version, this version, or both.



## 2\. Quick Start

The usage of the API is based on distinct resources.

Before starting to use the API, the developer needs to know about the specified details below:


**API service endpoint**

2 endpoints are defined in the SIM Swap API:

- /sim-swap/v0/retrieve-date : a MSISDN is passed in the request, and the last SIM swap date is provided in the response
- /sim-swap/v0/check : a MSISDN and a max age are passed in the request, and a boolean is provided in the response, telling whether the MSISDN has been 'swapped' to another SIM within the max age period.

**Authentication**

SIM Swap API considers 2 security models: Three-legged access tokens with dedicated scopes, and OAuth 2.0 client credentials, which is applicable for server to server use cases involving trusted partners or when protected user data is not involved.

**Identifier for the the mobile line to be checked**

In order to identify the mobile line to be checked, a MSISDN must be provided. 


Sample API invocations are presented in Section 4.6.

## 3\. Authentication and Authorization

The SIM Swap API could  make use of the OAUTH 2.0 client credentials grant which is applicable for server to server use cases involving trusted partners
or clients without any protected user data involved.
In this method the API invoker client is registered as a confidential client with an authorization grant type of client_credentials [[1]](#1).
The SIM Swap API could also be protected through a 3-legged process, in order to be combined with end user consent gathering, before any personal information is provided.

Depending on local legal regulation, Network provider may require the 3-legged process to provide SIM swap information.



## 4\. API Documentation

### 4.1 API Version

0.4.0

### 4.2 Details

#### 4.2.1 Endpoint Definition

Following table defines API endpoints of exposed REST based for SIM Swap API operations.

| **Endpoint** | **Operation** | **Description** |
| -------- | --------- | ----------- |
| POST /sim-swap/v0/retrieve-date | **Request last SIM swap date** | Create request in order to get the latest SIM swap date |
| POST /sim-swap/v0/check | **SIM Swap** | Create request to check if a SIM swap occurred during a defined period (like last 48hours) |



#### 4.2.2 SIM Swap API Resource Operations
<br>

| **Request last simswap date** |
| -------------------------- |
| **HTTP Request**<br> POST /sim-swap/v0/retrieve-date<br>**Query Parameters**<br> No query parameters are defined.<br>**Path Parameters**<br> No path parameter are defined <br>**Request Body Parameters**<br> **msisdn**: Subscriber number in E.164 format (starting with country code). Optionally prefixed with '+'.|
 |**Response**<br> **200: Contains information about sim swap change**<br>  Response body: <br>**latestSimChange** : timestamp of latest SIM swap performed. <br> **400:** **INVALID_ARGUMENT**<br> **401:** **UNAUTHENTICATED** <br> **403:** **PERMISSION_DENIED** <br> **409:** **Conflict** <br> **500:** **INTERNAL** <br> **503:** **UNAVAILABLE**<br> **504:** **TIMEOUT** <br>|

<br>


| **SIM Swap** |
| -------------------------- |
| **HTTP Request**<br> POST /sim-swap/v0/check<br>**Query Parameters**<br> No query parameters are defined.<br>**Path Parameters**<br> No path parameter are defined <br>**Request Body Parameters**<br> **msisdn**: Subscriber number in E.164 format (starting with country code). Optionally prefixed with '+'.<br> **maxAge**: Delay in hours to be checked for simswap.
|**Response**<br> **200: Contains information about SIM swap check**<br>  Response body: <br>**swapped** : Indicates whether the SIM card has been swapped during the delay. Should be valued only if an age is passed in the request. <br> **400:** **INVALID_ARGUMENT**<br> **401:** **UNAUTHENTICATED** <br> **403:** **PERMISSION_DENIED** <br> **409:** **Conflict** <br> **500:** **INTERNAL** <br> **503:** **UNAVAILABLE**<br> **504:** **TIMEOUT** <br>|

<br>
 
### 4.3 Errors

Since CAMARA SIM Swap API is based on REST design principles and blueprints, well defined HTTP status codes and families specified by community are followed [[2]](#2).

Details of HTTP based error/exception codes for SIM Swap API are described in Section 4.2 of each API REST based method.
Following table provides an overview of common error names, codes, and messages applicable to SIM Swap API.

| No | Error Name | Error Code | Error Message |
| --- | ---------- | ---------- | ------------- |
|1	|400 |	INVALID_ARGUMENT |	"Client specified an invalid argument, request body or query param" |
|2	|401 |	UNAUTHENTICATED |	"Request not authenticated due to missing, invalid, or expired credentials" |
|3	|403 |	PERMISSION_DENIED |	"Client does not have sufficient permissions to perform this action" |
|4	|500 |	INTERNAL | "Server error" |
|5	|503 |	UNAVAILABLE | "Service unavailable" |
|6	|504 |	TIMEOUT | "Request timeout exceeded. Try later." |

<br>
 
### 4.4 Policies

N/A
<br>
### 4.5 Code Snippets
<br>
<span class="colour" style="color:rgb(36, 41, 47)">Snippets elaborates REST based API call with "*curl"* to request  </span>


Please note, the credentials for API authentication purposes need to be adjusted based on target security system configuration.

| Snippet 1. Request last simswap date  |
| ----------------------------------------------- |
| curl -X 'POST' `https://sample-base-url/sim-swap/v0/retrieve-date`   <br>    -H 'accept: application/json' <br>    -H 'Content-Type: application/json'<br>    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...."<br>    -d '{ "msisdn": "+33068741256"}'  |
| response will be: <br> 200 <br>   -d '{ "latestSimChange": "2020-10-11T13:33:26.618Z" }'|
<br>

| Snippet 2. Request last simswap date  |
| ----------------------------------------------- |
| curl -X 'POST' `https://sample-base-url/sim-swap/v0/check`   <br>    -H 'accept: application/json' <br>    -H 'Content-Type: application/json'<br>    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...."<br>    -d '{ "msisdn": "+33068741256", <br> "maxAge": 240}'  |
| response will be: <br> 200 <br>   -d '{ "swapped": "false" }'|
<br>



### 4.6 FAQ's

(FAQs will be added in a later version of the documentation)

### 4.7 Terms

N/A

### 4.8 Release Notes

N/A


## References


<a name="1">[1] [CAMARA Commonalities : Authentication and Authorization Concept for Service APIs](https://github.com/camaraproject/WorkingGroups/blob/main/Commonalities/documentation/Working/CAMARA-AuthN-AuthZ-Concept.md) <br>
<a name="2">[2] [HTTP Status codes spec](https://restfulapi.net/http-status-codes/) <br>
