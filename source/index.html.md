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

To page track a simple ecommerce page like home page or category pages you should include this code shown on the right after including the `hupilytics.js` library.


```javascript
var pageTrack = function(clientName, siteId, currencyShortCode, productsDisplayed, productsRecommended, LangShortCode){
  var _paq = _paq || [];
  (function()
  {
  var u = "https://api.catchbox.hupi.io/v2/" + clientName + "/hupilytics";
  _paq.push(["setTrackerUrl", u]);  // Required
  _paq.push(["setSiteId", siteId]);  // Required: must be an integer


  // ! Required ! Our API needs the current timestamp for the page
  function current_ts()
  {
  // needed for IE8 compat, see http://bit.ly/1NLevPT
  if (!Date.now) {
  Date.now = function() {
  return new Date().getTime();
  };
  }
  return Math.floor(Date.now() / 1000);
  }
  // ! Required ! Our API needs the current timestamp for the page
  _paq.push(["setCustomVariable", 1, "current_ts", current_ts(), "page"]);
  _paq.push(["setCustomVariable", 43, "currency", currencyShortCode, "page"]); // You can use 'EUR' for euros and 'USD' for american dollar
  _paq.push(["setCustomVariable", 30, "products_impression", productsDisplayed, "page" ]); // All products shown on page excluding recommendations
  _paq.push(["setCustomVariable",40,"products_recommendation",productsRecommended,"page"]); // Only if page contains recommendations, this variable should be set with the list of recommendations
  _paq.push(["setCustomVariable", 42, "lang",LangShortCode, "page"]); // FR/EN - 2 char shortcode
  var d=document, g=d.createElement("script"), s=d.getElementsByTagName("script")[0];
  g.type="text/javascript";
  g.defer=true;
  g.async=true;
  g.src=u;
  s.parentNode.insertBefore(g,s);
  })();

  // To actually push the tracking data to hupi catchbox
  _paq.push(["trackPageView"]);
  _paq.push(["enableLinkTracking"]);
}
pageTrack(<CLIENTNAME>, <SITEID>, <CURRENCY-SHORTCODE>, <PRODUCTS-DISPLAYED-ON-PAGE>, <PRODUCT-IDS-RECOMMENDED-ON-PAGE>, <LANGUAGE-SHORT-CODE>)
```

This pushes the track page event to catchbox
### Variables Needed for pageTrack function

Parameter | example | Description
--------- | ------- | -----------
CLIENTNAME | 'hupi' | It should always be set
SITEID | 1 | If it is a staging/test site use 99, for production use 1. It is used for identifying your website.
CURRENCY-SHORTCODE | 'EUR' | Currency used in your site, You can use 'EUR' for euros and 'USD' for american dollar
PRODUCTS-DISPLAYED-ON-PAGE | ['1', '2', '3', '4'] | Id of all products that are displayed on the page except recommended products by hupi. If no products are shown, pass an empty array `[]`
PRODUCT-IDS-RECOMMENDED-ON-PAGE | ['5', '6', '7'] | Product ids which hupi recommended for you(see more about hupi recommendations here). If hupi recommendations are not displayed, then pass an empty array `[]`
LANGUAGE-SHORT-CODE | 'FR' | Human Language used on the site. Use a two character shortcode. For french 'FR', english 'EN', spanish 'ES'

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

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
curl "http://example.com/api/kittens/2"
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
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

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

