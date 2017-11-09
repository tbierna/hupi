# Hupi Recommendations


To include hupi recommendations in your site, you need to call our data-retriever api. 

<aside class="notice">You have to call our data-retriever api from your server only. We don't support calls direct calls from browser yet.</aside>

## Implementation Strategies

To display recommendations we recommend two strategies synchronous and asynchronous.

### ASynchronous Loading
We will start with the asynchronous strategy as shown in the picture below.

![alt text](images/asynchronous.png)

To explain in detail, when browser is loading your website page, you make an ajax call to a custom endpoint/route, this route will actually call hupi endpoint from server and hupi responds with recommendations in allowed formats and then the route action will parse this data and respond with either html or json based on your implementation to display the recommended products.

<aside class="warning"> If you follow this strategy, you need to call hupilytics tracking function after fetching recommendations as you won't have recommendations available until you return from your custom endpoint.</aside>

So, to implement this strategy you need to add a custom route action which calls hupi data-retriever from your server.

### Synchronous Loading

We will start with synchronous strategy as shown in the picture below.

![alt text](images/synchronous.png)

This is the simplest strategy to implement. To elaborate, when your browser requests for a page, you call hupi endpoint to get recommendations and you generate your page along with these recommendations and respond with the html page containing recommendations.

<aside class="notice">Hupi responds with recommendations within 100-200 ms.</aside>

## Dealing with Exceptions

When you make http request from your server to hupi, hupi responds with recommendations in a very short.  It is recommended to handle these exceptions in your code for fluid experience to your users.

These are the following exceptions that could occur.

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The endpoint is for admin only
404 | Not Found -- The specified endpoint could not be found.
405 | Method Not Allowed -- You tried to access a endpoint with an invalid method.
406 | Not Acceptable -- You requested a format that isn't json.
429 | Too Many Requests -- You're requesting too many times! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

But in order to protect your server from exception circumstances where hupi is taking longer than expected to respond with recommendations, you also are recommended to include a timeout when making a call to hupi server. The max timeout for response we recommend is 500ms.

## Endpoints

Hupi supports many recommendation services, hence for each service, we have an endpoint where the client can request for recommendations based on certain filters.

Those filters can be `visitor_id`(unique id assigned by hupilytics library), `product_id`(called `id_demande` in our filters), `basket`(all product ids in a cart/panier) or `category_id`.

<aside> Each endpoint is linked to a certain page types. Like category endpoint should always be linked to category page. This is done because each endpoints needs a specific set of filters.</aside>

### How to make a request

You have to make a http post request to our data retriever server with proper headers and body, and our responds with the results in `json` format.

An example curl request is shown on the right side(click on 'Curl/shell' table to see the example). All the required variables are wrapped around `<>`

```shell
curl -X POST \
  http://api.dataretriever.hupi.io/private/<CLIENT>/<ENDPOINT> \
  -H 'accept-version: v1' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'x-api-token: <apitoken>' \
  -d '{
  "client":"<CLIENT>",
  "render_type":"cursor",
  "filters":"{\"id_demande\":\"<product-id>\", \"visitor_id\":\"<visitor_id_from_pk_id>\"}"
}'
```

As you can see from the curl request shown, there are certain variables you need to make request to our api. Here we explain the request and variables in more detail.

#### Request URI

The request uri is of the following format `http://api.dataretriever.hupi.io/private/<CLIENT>/<ENDPOINT>`

You need to know the `CLIENT` name which is provided by hupi, it is usually the same as the one used in hupilytics tracking.

`ENDPOINT` is the name of the endpoint, the endpoint you are requesting recommendations from.

#### Request Headers

The headers needed are list below

1. Accept-version which is of the form 'accept-version: v1'
2. Content Type - the type of content you are requesting. We support only json for now. So it will be of form 'content-type: application/json'
3. Cache control - We don't want to use the same reco as before. So cache is disabled. It is of form 'cache-control: no-cache'
4. X-API-TOKEN - It is for authenticating and it's value is provided by hupi to you. It is unique per client. An example token is 'x-api-token: abcdefghijklmnopqrstuvwxyz'

So the variable needed in header is 'x-api-token', it is provided by hupi to you.

#### Request Body

The request body is of JSON type. And example is shown on the right

```javascript
{
  "client":"<CLIENT>",
  "render_type":"cursor",
  "filters":"{\"id_demande\":\"<product-id>\", \"visitor_id\":\"<visitor_id_from_pk_id>\"}"
}
```

As you can see the variables needed for body are CLIENT, which is the same as in the uri, and the variables in filters which vary based on endpoint. But as for the example above, they are 'product_id' and 'visitor_id'.

#### Request Type

POST

<aside> All Endpoints have the same header, but uri changes based on endpoint name and body filter also changes based on endpoint</aside>

### Endpoint Types

All endpoints have one common filter called `visitor_id`. It is set by the hupilytics js library when it is loaded and is unique for each user browser. It is set when user visits your site for the first and is the same whatever page he visits afterwards. It is available from browser by calling this function `hupiVisitorId()`. This function is part of hupilytics js library.

<aside> In first time visit of the website for a user, if you are following `synchronous` strategy, you won't have visitor_id set for the site, as it was never loaded on browser. In this case, just pass in a dummy value for visitor_id filter like `dummy`</aside>

#### hupi_reco_sim

The endpoint name is `hupi_reco_sim`. So the url will be `http://api.dataretriever.hupi.io/private/<CLIENT>/hupi_reco_sim`
And endpoint for recommending similar products. If a user visits a product page, you can use this endpoint to display similar products.

The endpoint needs two filters, `visitor_id` and `id_demande`(product_id). Both are of type 'String'

And example filter is shown on the right side(click on javascript tab)

```javascript
"filters":"{\"id_demande\":\"1000\", \"visitor_id\":\"b5d47b2a84e24c\"}"
```

#### hupi_reco_cart

The endpoint name is `hupi_reco_cart`. So the url will be `http://api.dataretriever.hupi.io/private/<CLIENT>/hupi_reco_cart`
An endpoint for recommending products on a cart/panier page. If a user navigates to checkout page, you can use this endpoint to display products which also could be added to cart along with existing in cart.

The endpoint needs two filters, `visitor_id` and `basket`. `basket` is array of product ids which are in cart.

And example filter is shown on the right

```javascript
"filters":"{\"visitor_id\": \"b5b288d6cdddca25\",\"basket\":\"[2402,640]\"}"
```

#### hupi_reco_home

The endpoint name is `hupi_reco_home`. So the url will be `http://api.dataretriever.hupi.io/private/<CLIENT>/hupi_reco_home`

An endpoint for recommending products on a home page. Based on user click history, we recommend what products he could click, so you use this endpoint to display products.

The endpoint needs only one filter `visitor_id`.

And example filter is shown on the right

```javascript
"filters":"{\"visitor_id\": \"b5b288d6cdddca25\"}"
```

#### hupi_reco_category

The endpoint name is `hupi_reco_category`. So the url will be `http://api.dataretriever.hupi.io/private/<CLIENT>/hupi_reco_category`

An endpoint for recommending products on a category page. Based on user click history in that category, we recommend what products he could click, so you use this endpoint to display products.

The endpoint needs two filters `visitor_id` and `category_id`(Category id is optional and should be a string).

And example filter is shown on the right

```javascript
"filters":"{\"visitor_id\": \"b5b288d6cdddca25\", \"category_id\": \"2\"}"
```


#### hupi_reco_sub_category

The endpoint name is `hupi_reco_sub_category`. So the url will be `http://api.dataretriever.hupi.io/private/<CLIENT>/hupi_reco_sub_category`

An endpoint for recommending products on a sub-category page. Based on user click history in that sub-category, we recommend what products he could click, so you use this endpoint to display products.

This endpoint is similar to category endpoint.

The endpoint needs two filters `visitor_id` and `sub_category_id`(Category id is optional and should be a string).

And example filter is shown on the right

```javascript
"filters":"{\"visitor_id\": \"b5b288d6cdddca25\", \"sub_category_id\": \"2\"}"
```

## Request Response Format

The response will be in ‘json’ format. We support two formats and both these format's should be supported by your implementation.

As shown on the right. Each key of ‘idR’ inside the ‘data’ array of response contains the recommended product i.e., PRODUCT-ID or array of product ids inside value of 'idRs' key.

You should parse this json and get product ids and then fetch the products of those ids and generate html code to display these product recommendations.

<aside> Product id can be either an integer or a string(Usually it is a string), so both these cases should handled as well as the cases where the ‘data’ array responds with an empty array.</aside>

You need to support for all endpoints the two formats.

### Format 1
product id in `idR` key

```javascript
// Format 1
{
  "data": [
    {
      "idR": <PROD_RECO>
    },
    {
      "idR": <PROD_RECO>
    },
    {
      "idR": <PROD_RECO>
    },
    {
      "idR": <PROD_RECO>
    }
  ]
}
```
### Format 2
Product ids in value array of  `idRs` key

```javascript
// Format 2
{
 "data": [
   {
     "idRs": [<PROD_RECO>, <PROD_RECO>]
   }
 ]
}
```

We usually return 8 product ids by default.



