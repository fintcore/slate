---
title: API Enovabancorp

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  #- ruby
  #- python
  - javascript

toc_footers:
 # - <a href='#'>Sign Up for a Developer Key</a>
  #- <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Enovabancorp API! You can use our API to access Enovabancorp API endpoints, which can get information on various accounts in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Accounts

## Create Account

<!-- ```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
``` -->

```shell
curl "https://api.enovabancorp.com/v1/accounts"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
  {
    "id": 439,
    "tipo": "debito",
    "cuenta": 452,
    "moneda": 1,
    "referencia": "ae3d2531-ac31-406b-86dc-a0d6171c27gt",
    "limites": {},
    "hash": null,
    "ultimo_hash": null
  }
```

This endpoint create accounts

### HTTP Request

`POST https://api.enovabancorp.com/v1/accounts`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
group | false | If set to true, the result will also include cats.
currency | true | If set to false, the result will include kittens that have already been adopted.
product | true | 
person | true |
reference | true |
name | true |
extras | optional |

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->

## Get All Accounts

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "https://api.enovabancorp.com/v1/accounts"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 438,
    "tipo": "debito",
    "cuenta": 451,
    "moneda": 1,
    "referencia": "STOREID_1453",
    "limites": {},
    "hash": null,
    "ultimo_hash": null,
    "saldo_disponible": 0.01,
    "grupo": 2,
    "extras": {
        "nombre": "Condominio"
    }
  },
  {
    "id": 178,
    "tipo": "debito",
    "cuenta": 165,
    "moneda": 1,
    "referencia": "1-002",
    "limites": null,
    "hash": "",
    "ultimo_hash": null,
    "saldo_disponible": 0,
    "grupo": 3,
    "extras": {
        "nombre": "Condominio Pantitlan"
    }
  }
]
```

This endpoint retrieves all accounts.

### HTTP Request

`GET https://api.enovabancorp.com/v1/accounts`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
group | ooptional | 

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->

## Get a Specific Account

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "https://api.enovabancorp.com/v1/accounts/178"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 178,
  "tipo": "debito",
  "cuenta": 165,
  "moneda": 1,
  "referencia": "1-002",
  "limites": null,
  "hash": "",
  "ultimo_hash": null,
  "saldo_disponible": 0,
  "grupo": 3,
  "extras": {
      "nombre": "Condominio Pantitlan"
  }
}
```

This endpoint retrieves a specific account.

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`GET https://api.enovabancorp.com/v1/accounts/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the account to retrieve

## Delete a Specific Account

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "https://api.enovabancorp.com/v1/accounts/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific account.

### HTTP Request

`DELETE https://api.enovabancorp.com/v1/accounts/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

