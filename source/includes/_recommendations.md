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

Hupi has various recommendation endpoints for different pages types.
