---
title: API DynamiCore

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - html

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
    "total": 1,
    "data": [
      {
        "reference": "a6ace6d3-4b9e-4c43-adff-451fd3bd12c1",
        "id": "424",
        "type": "client",
        "field": "acceptance_bc"
      }
    ]
  }
}
```
You must first upload the document [`here`](?shell#upload-documents). So that later it generates the document to sign.

This endpoint is used to create a document that will be signed.

To sign the document you must load the [`widget`](?html#widget).

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

## Widget

The signing widget is a tool that you can embed in your application so that stakeholders sign or approve documents in an iframe without leaving your website.

Note: It is not necessary for stakeholders to be registered in Mifiel In order to sign or approve a document.

To integrate it in your application, copy this `<script src="https://www.mifiel.com/sign-snippet-v1.0.0.min.js"></script>` and paste it before closing the </ body > tag in your page.

Each signer or reviewer of a document has a unique identifier to access the document. Make sure to insert the stakeholder’s widget id (which you received when creating the document) so that the stakeholders visualize the correct document in the signing widget.

Note: If not the html code block [`click here!`](?html#widget).

```html
<div id="mifiel-js"></div>
<script type="text/javascript">
  window.mifiel.widget({
    widgetId: 'widgetId',
    successBtnText: 'Proceed to next step',
    onSuccess: {
      // here the code you want to execute after the signer successfully sign and click the button could be an url  'mifiel.com' or a function() {}
      callToAction: function() {
        alert('signed document');
      },
    }
    onError: {
      // here the code you want to execute after an error occurs, could be an url  'mifiel.com' or a function() {}
      callToAction: 'https://www.mifiel.com',
    },
  });
</script>
</body>
```

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
    "total": 1,
    "data": [
      {
        "id": "a6ace6d3-4b9e-4c43-adff-451fd3bd12c1",
        "send_mail": true,
        "signed_hash": null,
        "name": null,
        "signed_by_all": true,
        "signed": true,
        "signed_at": "2022-02-25T23:31:19.950Z",
        "created_at": "2022-02-25T23:27:17.758Z",
        "burned_at": null,
        "status": [
          1,
          "SIGNED"
        ],
        "external_id": "Mifiel-00000000000000020",
        "remind_every": 0,
        "expires_at": null,
        "days_to_expire": null,
        "created_by": 1233,
        "state": "completed",
        "original_hash": "593b66c90b1364d9967e0050d7259872dcf9404a9b612992297d3ccb5760f6af",
        "manual_close": false,
        "encrypted": false,
        "allow_business": true,
        "file_file_name": "1635453902313.52099.pdf",
        "archived": false,
        "krs": false,
        "owner": {
          "id": 1233,
          "email": "ejsouto@liberfin.mx",
          "name": "Medici & Satoshi SAPI DE CV",
          "is_company": false
        },
        "creator": {
          "id": 1233,
          "email": "ejsouto@liberfin.mx",
          "name": "Medici & Satoshi SAPI DE CV"
        },
        "callback_url": "https://connector.dynamicore.io/webhooks/00b51d0d9d714ffabf12a626e335a485",
        "sign_callback_url": "https://connector.dynamicore.io/webhooks/00b51d0d9d714ffabf12a626e335a485",
        "already_signed": [
          {
            "id": 1233,
            "email": "ejsouto@liberfin.mx",
            "name": "Medici & Satoshi SAPI DE CV"
          }
        ],
        "has_not_signed": [],
        "permissions": {},
        "file": "/api/v1/documents/a6ace6d3-4b9e-4c43-adff-451fd3bd12c1/file",
        "file_download": "/api/v1/documents/a6ace6d3-4b9e-4c43-adff-451fd3bd12c1/file?download=true",
        "file_signed": "/api/v1/documents/a6ace6d3-4b9e-4c43-adff-451fd3bd12c1/file_signed",
        "file_signed_download": "/api/v1/documents/a6ace6d3-4b9e-4c43-adff-451fd3bd12c1/file_signed?download=true",
        "file_zipped": "/api/v1/documents/a6ace6d3-4b9e-4c43-adff-451fd3bd12c1/zip",
        "file_xml": "/api/v1/documents/a6ace6d3-4b9e-4c43-adff-451fd3bd12c1/xml",
        "signers": [
          {
            "id": "33e14ec6-2f3b-42a2-98e5-5a51af2201a3",
            "name": "ivan",
            "email": "ivan.peralta@dynamicore.io",
            "tax_id": "AAA010102AAA",
            "role": "signer",
            "field": null,
            "signed": true,
            "widget_id": null,
            "current": false,
            "last_reminded_at": "2022-02-25T23:27:23.675Z",
            "features": {
              "tax_id_validation": true
            },
            "customizations": {
              "show_tos": false,
              "show_tutorial": false
            },
            "sat_auth": false
          },
          {
            "id": "d2c4f8cf-6678-419e-a6df-3407f1651ed2",
            "name": "carter",
            "email": "carter.ipb16@gmail.com",
            "tax_id": "MAS1906288W3",
            "role": "signer",
            "field": null,
            "signed": true,
            "widget_id": null,
            "current": false,
            "last_reminded_at": "2022-02-25T23:27:23.567Z",
            "features": {
              "tax_id_validation": true
            },
            "customizations": {
              "show_tos": false,
              "show_tutorial": false
            },
            "sat_auth": false
          }
        ],
        "viewers": [
          {
            "id": "434ded55-8cc8-44f2-9d2b-567fdfbe09b8",
            "email": "ivan.peralta.b@outlook.com",
            "name": "ipb"
          }
        ],
        "signatures": [
          {
            "id": "d2c4f8cf-6678-419e-a6df-3407f1651ed2",
            "name": "carter",
            "email": "carter.ipb16@gmail.com",
            "signed": true,
            "signed_at": "2022-02-25T23:31:19.932Z",
            "certificate_number": "1AF2",
            "certificate": {
              "subject": {
                "CN": "Medici & Satoshi SAPI DE CV",
                "name": "Medici & Satoshi SAPI DE CV",
                "O": "Medici & Satoshi SAPI DE CV",
                "C": "MX",
                "emailAddress": "oswaldo@medici-satoshi.com",
                "x500UniqueIdentifier": "MAS1906288W3 / LOGO861010GN7",
                "serialNumber": "LOGO861010HDFPRS08"
              },
              "issuer": {
                "C": "MX",
                "O": "Mifiel",
                "CN": "Mifiel Intermediate",
                "emailAddress": "info@mifiel.com"
              }
            },
            "tax_id": "MAS1906288W3",
            "signature": "b38da91acd4f9cea35ac5485d5ceb91967a20d27ac0cff84c95c80cc26130fa5a4c9a3a9b48871b75cf09280b854cd8100445f3c70669a46b207dd2d15ed6637966532035560b475622e84db459848764c6155bde941f4cbce0e1655e978c11648316db3dc2a6c26e361063a54d89c0c6f324c1453587c2c47957ec0665d46ebbef3a984022d7b0543f559d002c75ab7dfc6820845a4699392053172466343259b38dc0f86204b8832ad97c8721a5fa71589f3e4035f35efcfde1a6ad9aaf1351733eca1f068292f74bb5086048a5cbd693991e44da00021134bc36d9aa84580d338afa86c38aba6ebeb8423782b8acc31c4592b6a7f1d31331a9e042fd74b98",
            "user": {
              "id": 1233,
              "email": "ejsouto@liberfin.mx",
              "name": "Medici & Satoshi SAPI DE CV"
            }
          },
          {
            "id": "33e14ec6-2f3b-42a2-98e5-5a51af2201a3",
            "name": "ivan",
            "email": "ivan.peralta@dynamicore.io",
            "signed": true,
            "signed_at": "2022-02-25T23:28:10.016Z",
            "certificate_number": "1AF3",
            "certificate": {
              "subject": {
                "CN": "Racks SAPI",
                "name": "Racks SAPI",
                "O": "Racks SAPI",
                "C": "MX",
                "emailAddress": "oswaldo@racks.app",
                "x500UniqueIdentifier": "AAA010102AAA / AAAA010102AAA",
                "serialNumber": "AAAA010101HDFPRS08"
              },
              "issuer": {
                "C": "MX",
                "O": "Mifiel",
                "CN": "Mifiel Intermediate",
                "emailAddress": "info@mifiel.com"
              }
            },
            "tax_id": "AAA010102AAA",
            "signature": "2b122d9827e7277b41ca888ad39a6c719292ce53b12ef5e51611919d553856782cb64445e428fcf78356ed9c676691c8270bd9ee769d777f3fcf83d603a10391d61f45d1d291de6f9c058cd38e0db0fd6f970673f45109471084e34bc6bb510d0aa51a849ec472fdf320304a208a44352e678c8666eb4dc55c5204e4939b379a544c240acab4aba43bdfb8f8f91826582f80a8c21c999028045835d7b117398d889bcb5afa37a34c7264c0e34565a5b49eda419ee9858e025de20334c5ad0b8056dad549645fe100af7ab6d9693d542cbb59812052ecdf07621d74aa21e5e35776fb63e249c4732b7140b6905866a298b507b44dd9cd1279c4b4ef4cb83d4513"
          }
        ]
      }
    ]
  }
}
```

This endpoint is used to get Document data.

### HTTP Request

`GET https://api.dynamicore.io/marketplace/apps/mifiel/documents/:id`

# Conekta

## Create Order

```shell
curl --request POST 'https://api.dynamicore.io/marketplace/apps/conekta/order/new_order' \
--header 'Authorization: {{hmacAuth}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "account": 326,
    "operation": 184,
    "config": {},
    "customer_info": {
      "name": "other prueba"
    },
    "items": {
      "name": "Box of Cohiba S1s",
      "unit_price": 1000,
      "quantity": 1
    },
    "payment_method": {
          "type": "oxxo_cash"
    }
}'
```

> RESPONSE:

```json
{
  "status": "success",
  "message": {
    "code": 1,
    "total": 1,
    "data": [
      {
        "id": "ord_2rQPaj3neXQQmH7fp",
        "channel": "oxxo_cash",
        "amount": 1000,
        "operation": 184,
        "account": 326,
        "company": 2941,
        "active": "0",
        "config": {}
      }
    ]
  }
}
```

This endpoint is used to create order.

### HTTP Request

`POST https://api.dynamicore.io/marketplace/apps/conekta/order/new_order`

### Query Parameters

| Parameter | Required | Type   | Description                    | Example    |
| --------- | -------- | ------ | ------------------------------ | ---------- |
| account    |     Y    | Integer | Account              | 1          |
| operation     |     Y    | Integer | Operation   | 184 | 
| customer_info       |     Y    | Object | Object with customer information. Telephone and email is optional     | {"name": "Demo", "phone":"", "email":""} |
| items     |     Y    | Object | Object with item information. The price per unit expressed in cents.            | { "name": "Box of Cohiba S1s", "unit_price": 1000, "quantity": 1 }         |
| payment_method      |     Y    | Object | Object with payment method information         | { "type": "oxxo_cash" }|
| config    |     O    | Object | Config              | {}          |

## Get Order

```shell
curl --request GET 'https://api.dynamicore.io/marketplace/apps/conekta/order/get_order/:id' \
--header 'Authorization: {{hmacAuth}}'
```

> RESPONSE:

```json
 {
  "status": "success",
  "message": {
    "code": 1,
    "total": 1,
    "data": [
      {
        "livemode": false,
        "amount": 900,
        "currency": "MXN",
        "payment_status": "paid",
        "amount_refunded": 0,
        "customer_info": {
          "email": "correo@dominio.com",
          "phone": "55-5555-5555",
          "name": "other prueba",
          "object": "customer_info"
        },
        "object": "order",
        "id": "ord_2rLDf1e77o2mKHjY3",
        "metadata": {},
        "created_at": 1645230551,
        "updated_at": 1645230582,
        "line_items": {
          "object": "list",
          "has_more": false,
          "total": 1,
          "data": [
            {
              "name": "Box of Cohiba S1s",
              "unit_price": 900,
              "quantity": 1,
              "object": "line_item",
              "id": "line_item_2rLDf1e77o2mKHjY1",
              "parent_id": "ord_2rLDf1e77o2mKHjY3",
              "metadata": {},
              "antifraud_info": {}
            }
          ]
        },
        "charges": {
          "object": "list",
          "has_more": false,
          "total": 1,
          "data": [
            {
              "id": "621039d741de2719c7477354",
              "livemode": false,
              "created_at": 1645230551,
              "currency": "MXN",
              "payment_method": {
                "service_name": "OxxoPay",
                "barcode_url": "https://s3.amazonaws.com/cash_payment_barcodes/sandbox_reference.png",
                "object": "cash_payment",
                "type": "oxxo",
                "expires_at": 1647648000,
                "store_name": "OXXO",
                "reference": "98000012409960"
              },
              "object": "charge",
              "description": "Payment from order",
              "status": "paid",
              "amount": 900,
              "paid_at": 1645230582,
              "fee": 41,
              "customer_id": "",
              "order_id": "ord_2rLDf1e77o2mKHjY3"
            }
          ]
        }
      }
    ]
  }
}
```

This endpoint is used to get Order data.

### HTTP Request

`GET https://api.dynamicore.io/marketplace/apps/conekta/order/get_order/:id`
