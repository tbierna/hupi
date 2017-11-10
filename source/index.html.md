---
title: Hupi Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript
  - shell: cURL
  - ruby


toc_footers:
  - <a href='https://github.com/hupi-analytics/hupilytics'>Hupilytics Source Repo</a>

includes:
  - recommendations

search: true
---

# Introduction

Hupi Integration Guide. [TO BE UPDATED SOON]


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

## Action - Tracking a Add to Cart

To page track ecommerce add to cart event, you should include `hupiAddToCartTrack` function with proper arguments. The function signature is shown on the right

```javascript
hupiAddToCartTrack(
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
    productCategories,
    productQuantity,
    totalCartAmount
    );
```

The detailed explaination of arguments and their types is listed in the table below.
Here is an example js code with dummy values.

```javascript
// Example code with dummy values
hupiAddToCartTrack('testsite', 1, 'EUR', '10000-id', ['1', '2'], ['4', '5', '6'], 'FR', '100-pid', '100-prodname', '10.8', ['prod', 'categories'], 1, '100.2');

```
This pushes the track page event to catchbox.

### Params/Arguments Needed for hupiAddToCartTrack function

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
productQuantity | 1 | Integer | Quantity of a product. Should be atleast 1.
totalCartAmount | '1000.4' | String | Total price of products in cart. Should be a string.

<aside class="notice">
This code should only be included after loading hupilytics js library.
</aside>

## Action - Remove from Cart

To track ecommerce Remove from cart, when a user removes a product from his cart, this event should be triggered. You should call this function 'hupiRemoveFromCartTrack'
This function has the same arguments as `hupiAddToCartTrack`, so for reference on usage look above.

## Action - Track hupi Recommendation Click

To track product clicks on hupi recommendations, you need to call this function `hupiRecoTrack` with proper arguments.

The function signature is shown on the right

```javascript
hupiRecoTrack(
    hupiEndpointName,
    productId,
    productName)
```

```javascript
// Example code with dummy values
hupiRecoTrack('hupi_reco_sim', 'pid', 'name');

```
This pushes the track page event to catchbox.

<aside class="notice">
This code should be included as onclick event call in html code of the product link. It is advised to delay 400ms before navigating to the clicked product, so that hupi will receive the event before page refreshes with clicked product.
</aside>

### Params/Arguments Needed for RecoTrack function

Parameter | Example | Type | Description
--------- | ------- | ---- | -----------
hupiEndpointName | 'hupi_reco_sim' | String | The name of the hupi endpoint from which you fetched the products to display
productId | 'pid' | String | Product Id, unique identifier for product which was clicked
productName | 'name' | String | Name of the product clicked

## Action - Tracking an Order

To page track ecommerce purchases, you should include `hupiOrderTrack` function with proper arguments. The function signature is shown on the right

```javascript
hupiOrderTrack(
    clientName,
    siteId,
    currencyShortCode,
    userId,
    productsDisplayed,
    productsRecommended,
    LangShortCode,
    orderId,
    orderRevenueTotal,
    orderSub,
    orderTaxAmount,
    orderShippingAmount,
    orderDiscountOffered,
    productsInCart
    );
```

The detailed explaination of arguments and their types is listed in the table below.
Here is an example js code with dummy values.

```javascript
// Example code with dummy values. 
hupiOrderTrack('testsite', 1, 'EUR', '10000-id', ['1', '2'], ['4', '5', '6'], 'FR', '100orderid', 100.0, 0, 20.0, 10.0, true, [['pid', 'name', ['cat1', 'cat2'], '10.2', 1]]);

```
This pushes the track page event to catchbox.
<aside>productsInCart is an array of products(2d array), and each product is an array with (product id, name, category list, price, quantity)</aside>

### Params/Arguments Needed for hupiOrderTrack function

Parameter | Example | Type | Description
--------- | ------- | ---- | -----------
clientName | 'hupi' | String | Name of the client in smallcase, usually provided by hupi to you.
siteId | 1 | Integer | If it is a staging/test site use 99, for production use 1. It is used for identifying your website. This is also usually provided by hupi.
currencyShortCode | 'EUR' | String | Currency used in your site, You can use 'EUR' for euros and 'USD' for american dollar
productsDisplayed | ['1', '2', '3', '4'] | Array of Strings | Id of all products that are displayed on the page except recommended products by hupi. If no products are shown, pass an empty array `[]`
productsRecommended | ['5', '6', '7'] | Array of Strings | Product ids which hupi recommended for you(see more about hupi recommendations here). If hupi recommendations are not displayed, then pass an empty array `[]`
LangShortCode | 'FR' | String | Human Language used on the site. Use a two character shortcode. For french `FR`, english `EN`, spanish `ES`
userId | '100' | String | UserId of the user which is available when the user is logged in. It should be a unique identifier. If not available in cases of not logged in, then pass empty string `''`
orderId | '1' | String | (required) Unique Order ID
orderRevenueTotal | 100000.001 | Integer/Float | (required) Order Revenue grand total (includes tax, shipping, and subtracted discount), must be an integer or a float
orderSub | 10 | Integer  | Order sub total (excludes shipping and excludes taxes), must be an integer. If can't be valid, put 0.
orderTaxAmount | 1.0 | Integer/Float | Tax. If doesn't apply, put 0.
orderShippingAmount | 10.0 | Integer/Float | Shipping costs. 0 if not there.
orderDiscountOffered | 0 | Integer/Float | boolean (set to false for unspecified parameter)
productsInCart | [['pid1', 'name1', ['cat1', 'cat2'], 10.2, 1],['pid2', 'name2', ['cat1', 'cat2'], 10.2, 1]] | Array of String Arrays | All products in cart, it should be a 2d array, each inside array represents a product. Inside array has the following values(productId - String, productName - String, productCategory - Array of Strings, productPrice - String, product Quantity - Integer)

<aside class="notice">
This code should only be included after loading hupilytics js library.
</aside>


