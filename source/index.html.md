---
title: API DynamiCore

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  # - <a href='#'>Sign Up for a Developer Key</a>
  #- <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the DynamiCore API! You can use our API to access DynamiCore API endpoints, which can get information on various accounts in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

Message hash key authentication (HMAC) is a specific construct for calculating a message authentication code, which involves a cryptographic hash function in combination with a cryptographic secret key.

Like any MAC, it can be used to simultaneously verify data integrity and authentication of a message. The SHA-512 cryptographic hash function is used.

The hash function breaks a message into a block of a fixed size and iterates over them with a compression function. The cryptographic strength of HMAC depends on the cryptographic power of the underlying hash function, the size of its hash output and quality of the key.

> To authorize, use this code:

```javascript
function getPath(url) {
  var pathRegex = /.+?\:\/\/.+?(\/.+?)(?:#|\?|$)/;
  var result = url.match(pathRegex);
  return result && result.length > 1 ? result[1] : "";
}

function getQueryString(url) {
  var arrSplit = url.split("?");
  return arrSplit.length > 1 ? url.substring(url.indexOf("?") + 1) : "";
}

var _requestData;

function getAuthHeader(httpMethod, requestUrl, _requestBody) {
  var CLIENT_KEY = "CLIENT_KEY";
  var hash = "SECRET_KEY";
  var SECRET_KEY = CryptoJS.enc.Hex.stringify(CryptoJS.SHA512(hash));
  var AUTH_TYPE = "Ictineo";
  var requestBody = "";
  var requestPath = getPath(requestUrl);
  var queryString = getQueryString(requestUrl);
  if (httpMethod == "GET" || !_requestBody) {
    requestBody = "";
  } else {
    requestBody = JSON.stringify(JSON.parse(_requestBody));
  }

  var timestamp = new Date().getTime();
  var requestData = [
    timestamp,
    httpMethod,
    requestPath,
    queryString,
    requestBody,
  ].join("");
  requestData = requestData.trim();
  _requestData = requestData;
  var hmacDigest = CryptoJS.enc.Hex.stringify(
    CryptoJS.HmacSHA256(requestData, SECRET_KEY)
  );
  var authHeader =
    AUTH_TYPE + " " + CLIENT_KEY + ":" + timestamp + ":" + hmacDigest;
  return authHeader;
}

postman.setEnvironmentVariable(
  "hmacAuthHeader",
  getAuthHeader(request["method"], request["url"], request["data"])
);
postman.setEnvironmentVariable("hmac_d", _requestData);
```

# Types

## Types of Clients

```shell
curl "https://api.dynamicore.io/v1/clients/types"
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d name="Persona Fisica" \
  -d description="Template para registro de personas fisicas en SOFOM" \
  -d "template[0][]"="Lopez" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to create client types

### HTTP Request

`POST https://api.dynamicore.io/v1/clients/types`

### Parameters

| Parameter     | Required | Type   | Description                | Example                                             |
| ------------- | -------- | ------ | -------------------------- | --------------------------------------------------- |
| name          | Y        | String | Name of client type        | Persona Fisica                                      |
| description   | O        | String | Description of client type | Template para registro de personas fisicas en SOFOM |
| pii[lastname] | Y        | String | Lastname of client         | Lopez                                               |

## Types of Products

```shell
curl "https://api.dynamicore.io/v1/products/types"
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to create product types

### HTTP Request

`POST https://api.dynamicore.io/v1/products/types`

### Parameters

| Parameter     | Required | Type   | Description                 | Example                                             |
| ------------- | -------- | ------ | --------------------------- | --------------------------------------------------- |
| name          | Y        | String | Name of product type        | Persona Fisica                                      |
| description   | O        | String | Description of product type | Template para registro de personas fisicas en SOFOM |
| pii[lastname] | Y        | String | Lastname of client          | Lopez                                               |

<!--## Types of Email

```shell
curl "https://api.dynamicore.io/v1/emails/types"
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint retrieves a specific account.

### HTTP Request

`GET https://api.dynamicore.io/v1/emails/types`

### URL Parameters

| Parameter | Description                       |
| --------- | --------------------------------- |
| ID        | The ID of the account to retrieve |
-->

# Catalogs

## Catalog of Type of Client

```shell
curl "https://api.dynamicore.io/v1/catalog/fields/client"
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint retrieves a catalog of client templates.

### HTTP Request

`GET https://api.dynamicore.io/v1/catalog/fields/client`

## Catalog of product templates

```shell
curl "https://api.dynamicore.io/v1/catalog/fields/product"
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint retrieves a catalog of product templates.

### HTTP Request

`GET https://api.dynamicore.io/v1/catalog/fields/product`

# Clients

## Create Client

```shell
curl https://api.dynamicore.io/private/clients \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d client_type="17" \
  -d "pii[firstname]"="Rafael" \
  -d "pii[lastname]"="Lopez" \
  -d "pii[rfc]"="RALO101086GT4" \
  -d "pii[email]"="email@google.com"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to create client.

### HTTP Request

`POST https://api.dynamicore.io/private/clients`

### Parameters

| Parameter      | Required | Type   | Description         | Example          |
| -------------- | -------- | ------ | ------------------- | ---------------- |
| client_type    | Y        | String | Type of client      | 17               |
| pii[firstname] | Y        | String | Firstname of client | Rafael           |
| pii[lastname]  | Y        | String | Lastname of client  | Lopez            |
| pii[rfc]       | Y        | String | RFC of client       | RALO101086GT4    |
| pii[email]     | Y        | String | Email of client     | email@google.com |

## Update Client

```shell
curl https://api.dynamicore.io/private/clients \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d id="1" \
  -d "pii[firstname]"="Rafael" \
  -d "pii[lastname]"="Lopez" \
  -d "pii[rfc]"="RALO101086GT4" \
  -d "pii[email]"="email@google.com"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to update client.

### HTTP Request

`PUT https://api.dynamicore.io/private/clients`

### Parameters

| Parameter      | Required | Type   | Description         | Example          |
| -------------- | -------- | ------ | ------------------- | ---------------- |
| id             | Y        | String | ID Clients          | 1                |
| pii[firstname] | O        | String | Firstname of client | Rafael           |
| pii[lastname]  | O        | String | Lastname of client  | Lopez            |
| pii[rfc]       | O        | String | RFC of client       | RALO101086GT4    |
| pii[email]     | O        | String | Email of client     | email@google.com |

## List of Clients

```shell
curl https://api.dynamicore.io/private/clients \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d id="1"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to list of clients.

### HTTP Request

`GET https://api.dynamicore.io/private/clients`

### Query Parameters

| Parameter | Required | Type   | Description       | Example |
| --------- | -------- | ------ | ----------------- | ------- |
| id        | O        | String | Client Identifier | 1       |

## Upload Documents

```shell
curl https://api.dynamicore.io/files/users/upload/:client_id/:pii[acta_constitutiva] \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  --form 'file=@"/path/acta_constitutiva.pdf"'
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to upload documents to client.

### HTTP Request

`POST https://api.dynamicore.io/files/users/upload/:client_id/:pii[acta_constitutiva]`

### FormData Parameters

| Parameter      | Required | Type             | Description         | Example                        |
| -------------- | -------- | ---------------- | ------------------- | ------------------------------ |
| file           | Y        | Array of files   |                     | acta_constitutiva.pdf, rfc.pdf |

## Get SAT

```shell
curl https://api.dynamicore.io/v1/clients/:client_id/sat \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d cer="data:application/x-x509-ca-cert;base64"
  -d key="data:application/x-iwork-keynote-sffkey;base64"
  -d pas="******"
  -d rfc="RAFO830910GT7"
  -d type="efirma"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to get SAT data.

### HTTP Request

`GET https://api.dynamicore.io/v1/clients/:client_id/sat`

## Get Credit Score

```shell
curl https://api.dynamicore.io/v1/clients/:client_id/score \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to get Credit Score.

### HTTP Request

`GET https://api.dynamicore.io/v1/clients/:client_id/score`

# Accounts

## Create Account

```shell
curl https://api.dynamicore.io/v1/accounts \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d client_id="1" \
  -d product="6" \
  -d enabled="1" \
  -d currency="484" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to create account.

### HTTP Request

`POST https://api.dynamicore.io/v1/accounts`

### Parameters

| Parameter | Required | Type   | Description                                              | Example |
| --------- | -------- | ------ | -------------------------------------------------------- | ------- |
| client_id | Y        | String | Client Identifier                                        | 1       |
| product   | Y        | String | Product Identifier                                       | 6       |
| enabled   | Y        | String | Enabled of product. . Values: Active -> 1, Inactive -> 0 | 1       |
| currency  | Y        | String | Product currency                                         | 484     |

## Customer accounts

```shell
curl https://api.dynamicore.io/v1/accounts \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d client_id="1" \
  -d id="1"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to customer accounts.

### HTTP Request

`GET https://api.dynamicore.io/v1/accounts`

### Query Parameters

| Parameter | Required | Type   | Description        | Example |
| --------- | -------- | ------ | ------------------ | ------- |
| id        | Y        | String | Account Identifier | 1       |
| client_id | Y        | String | Client Identifier  | 1       |

## Account transactions

```shell
curl https://api.dynamicore.io/v1/accounts/:account_id/transactions \
  -H "accept: application/json" \
  -H "Content-Type: application/json"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to get account transactions.

### HTTP Request

`GET https://api.dynamicore.io/v1/accounts/:account_id/transactions`

## Account payments

```shell
curl https://api.dynamicore.io/v1/accounts/:account_id/payments \
  -H "accept: application/json" \
  -H "Content-Type: application/json"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to get account payments.

### HTTP Request

`GET https://api.dynamicore.io/v1/accounts/:account_id/payments`

# MARKETPLACE

# SAT

## Load Keys

```shell
curl https://api.dynamicore.io/internal/sat/sat_key \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d client=1 \
  -d type="ciec" \
  -d pas="" \
  -d key="" \
  -d cer="" \
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to keys for SAT consult.

### HTTP Request

`POST https://api.dynamicore.io/internal/sat/sat_key`

### Parameters

| Parameter | Required | Type   | Description                                              | Example |
| --------- | -------- | ------ | -------------------------------------------------------- | ------- |
| client    | Y        | Number | Client Identifier                                        | 1       |
| type      | Y        | String | Type of keys for SAT consult. Values: ciec(You need to send the pas parameter), efirma(You need to send the pas, key and cer parameters)       | ciec    |
| pas       | Y        | String | Password of keys                                         |         |
| key       | O        | String | Private key. File of type key in Base64                  | file.key|
| cer       | O        | String | Certificate. File of type cer in Base64                  | file.cer|

## Sync Data 

```shell
curl https://api.dynamicore.io/internal/sat/sat_key \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d client=1 \
  -d start="2021-01-01"
  -d end: "2021-11-31"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to sync of the SAT data. Its execute is async to obtain data you should used the endpoint Get Data.

### HTTP Request

`POST https://api.dynamicore.io/internal/sat/sat_key`

### Parameters

| Parameter | Required | Type   | Description                    | Example    |
| --------- | -------- | ------ | ------------------------------ | ---------- |
| client    | Y        | Number | Client Identifier              | 1          |
| start     | Y        | String | Start of date to obtain data   | 2021-01-01 | 
| end       | Y        | String | End of date to obtain data     | 2021-11-31 |

## Get Data

```shell
curl https://api.dynamicore.io/private/sat/data \
  -H "accept: application/json" \
  -H "Content-Type: application/json"
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to get data of SAT.

### HTTP Request

`GET https://api.dynamicore.io/private/sat/data`

### Query Parameters

| Parameter | Required | Type   | Description                    | Example    |
| --------- | -------- | ------ | ------------------------------ | ---------- |
| client    |     Y    | Number | Client Identifier              | 1          |
| start     |     O    | String | Start of date to obtain data   | 2021-01-01 | 
| end       |     O    | String | End of date to obtain data     | 2021-11-31 |
| limit     |     O    | String | Limit query results            | 50         |
| page      |     0    | String | Paginate query results         | 1|

# Mifiel

## Create Document

```shell
curl --request POST 'https://api.dynamicore.io/marketplace/apps/mifiel/documents' \
--header 'Authorization: {{hmacAuth}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "id": 424,
  "type": "client",
  "field": "acceptance_bc",
  "signatories": [
    { "name": "Demo", "email": "demo@demo.com", "tax_id": "DEMO1A" },
  ],
  "external_id": "Mifiel-0000000000000000",
  "day_to_expire": 1,
  "message_for_signers": "Message",
  "remind_every": 0,
  "send_invites": "true",
  "viewers": [{"email": "demo@demo.com","name": "demo"}]
}'
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to create a document that will be signed 

### HTTP Request

`POST https://api.dynamicore.io/marketplace/apps/mifiel/documents`

### Parameters

| Parameter      | Required | Type   | Description         | Example          |
| -------------- | -------- | ------ | ------------------- | ---------------- |
| id             | Y        | String | id of client        | 424               |
| type           | Y        | String | type                | client           |
| field          | Y        | String | field               | acceptance_bc    |
| signatories    | Y        | Array of objects | A list of Signatory Object       |  [{"email":"","name": "","tax_id": ""}]    |
| external_id     | O        | String | Your unique identifier  | Mifiel-0000000000000001 |
| day_to_expire   | O        | Integer | Number of days the document expires  | 1 |
| send_invites    | O        | boolean | Set True if you want Mifiel to email invitations to attendees. If your participants will sign in the widget embedded within your application send False.  | "true" |
| remind_every   | O        | Integer | Signing reminders will be sent to those who have not signed (if send_mail is enabled)  | 0 |
| viewers   | O        | Array of objects | List of emails from whom they will receive notifications (if enabled) every time someone signs and they will receive a copy of the signed document.  | [{"email":"","name": ""}]  |

## Get Document

```shell
curl --request GET 'https://api.dynamicore.io/marketplace/apps/mifiel/documents/:id' \
--header 'Authorization: {{hmacAuth}}'
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to get Document data.

### HTTP Request

`GET https://api.dynamicore.io/marketplace/apps/mifiel/documents/:id`

## Delete Document

```shell
curl --request DELETE 'https://api.dynamicore.io/marketplace/apps/mifiel/documents/:id' \
--header 'Authorization: {{hmacAuth}}'
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 0,
    "data": []
  }
}
```

This endpoint is used to delete the document.

### HTTP Request

`DELETE https://api.dynamicore.io/marketplace/apps/mifiel/documents/:id`