---
title: Hupi Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: cURL
  - ruby
  - javascript

toc_footers:
  - <a href='https://github.com/hupi-analytics/hupilytics'>Hupilytics Source Repo</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

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

# Hupilytics

To track user actions on your site via hupi, you have to include this javascript libary called [hupilytics](https://github.com/hupi-analytics/hupilytics/blob/master/hupilytics.js) in your website.

All these tracked user actions are sent to our catchbox servers, the library takes care of pushing these events to our servers. You just need to have your hupi provided account/client name, siteId.

And example api key and client/account name will be like this shown on the right side.

> client = hupi

<aside class="notice">
It is better to include this js library at the bottom of your html page
</aside>

Now we move on to tracking specific actions.

## Action - Tracking a Page

To page track ecommerce pages like home page or category pages you should include `hupiPageTrack` function with proper arguments. The function signature is shown on the right

```javascript
hupiPageTrack(
  clientName, 
  siteId, 
  currencyShortCode,
  userId,
  productsDisplayed, 
  productsRecommended,
  LangShortCode);
```

The detailed explaination of arguments and their types is listed in the table below.
Here is an example js code with dummy values.

```javascript
// Example code with dummy values for logged in user
hupiPageTrack('testsite', 1, 'EUR', '10000-id', ['1', '2'], ['4', '5', '6'], 'FR');

// Example code with dummy values for user who is not logged in
hupiPageTrack('testsite', 1, 'EUR', '', ['1', '2'], ['4', '5', '6'], 'FR');
```
This pushes the track page event to catchbox.

### Params/Arguments Needed for hupiPageTrack function

Parameter | Example | Type | Description
--------- | ------- | ---- | -----------
clientName | 'hupi' | String | Name of the client in smallcase, usually provided by hupi to you.
siteId | 1 | Integer | If it is a staging/test site use 99, for production use 1. It is used for identifying your website. This is also usually provided by hupi.
currencyShortCode | 'EUR' | String | Currency used in your site, You can use 'EUR' for euros and 'USD' for american dollar
productsDisplayed | ['1', '2', '3', '4'] | Array of Strings | Id of all products that are displayed on the page except recommended products by hupi. If no products are shown, pass an empty array `[]`
productsRecommended | ['5', '6', '7'] | Array of Strings | Product ids which hupi recommended for you(see more about hupi recommendations here). If hupi recommendations are not displayed, then pass an empty array `[]`
LangShortCode | 'FR' | String | Human Language used on the site. Use a two character shortcode. For french `FR`, english `EN`, spanish `ES`
userId | '100' | String | UserId of the user which is available when the user is logged in. It should be a unique identifier. If not available in cases of not logged in, then pass empty string `''`

<aside class="notice">
This code should only be included after loading hupilytics js library.
</aside>

## Action - Tracking a Product Page/Click

To page track ecommerce product pages/clicks, you should include `hupiProductClickTrack` function with proper arguments. The function signature is shown on the right

```javascript
hupiProductClickTrack(
    clientName,
    siteId,
    currencyShortCode,
    userId,
    productsDisplayed,
    productsRecommended,
    LangShortCode,
    productId,
    productName,
    productPrice,
    productCategories);
```

The detailed explaination of arguments and their types is listed in the table below.
Here is an example js code with dummy values.

```javascript
// Example code with dummy values
hupiProductClickTrack('testsite', 1, 'EUR', '10000-id', ['1', '2'], ['4', '5', '6'], 'FR', '100-pid', '100-prodname', '10.8', ['prod', 'categories']);

```
This pushes the track page event to catchbox.
Include this in product page, If product is viewed via modal include it in modal javascript.

### Params/Arguments Needed for hupiProductClickTrack function

Parameter | Example | Type | Description
--------- | ------- | ---- | -----------
clientName | 'hupi' | String | Name of the client in smallcase, usually provided by hupi to you.
siteId | 1 | Integer | If it is a staging/test site use 99, for production use 1. It is used for identifying your website. This is also usually provided by hupi.
currencyShortCode | 'EUR' | String | Currency used in your site, You can use 'EUR' for euros and 'USD' for american dollar
productsDisplayed | ['1', '2', '3', '4'] | Array of Strings | Id of all products that are displayed on the page except recommended products by hupi. If no products are shown, pass an empty array `[]`
productsRecommended | ['5', '6', '7'] | Array of Strings | Product ids which hupi recommended for you(see more about hupi recommendations here). If hupi recommendations are not displayed, then pass an empty array `[]`
LangShortCode | 'FR' | String | Human Language used on the site. Use a two character shortcode. For french `FR`, english `EN`, spanish `ES`
userId | '100' | String | UserId of the user which is available when the user is logged in. It should be a unique identifier. If not available in cases of not logged in, then pass empty string `''`
productId | '1' | String | Unique Id of the product, should always be a string, convert to string even if it is an integer before passing to this funciton. Mandatory.
productName | 'prod name' | String | Name of the product
productPrice | '10' | String | Price of the product, just the value is enough without passing in currency(Currency is one of the params already)
productCategory | ['cat', 'dog'] | Array of Strings | List of categories product belongs to. If nothing, pass in an empty array `[]`.

<aside class="notice">
This code should only be included after loading hupilytics js library.
</aside>


This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

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
curl "http://example.com/api/kittens/2"
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

This endpoint retrieves a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

