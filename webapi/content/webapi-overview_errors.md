
# HTTP Error Codes

|Response Code| Meaning|
|----|----|
|`400`| Invalid request (ID, ...) |
|`403`|	The user don't have access to this car. Usually an error with the vehicle ID.|
|`404`| Data not found. Can be that there is no data for this specific vehicle or a typo on the URL |
|`429`| Too Many Requests. The limit of requests have been exceed, see [rate limit](#rate-limit). |
|`500`| Internal server error. Sounds like there's a problem on the server. Take it easy, we're on it ;)|

## Error Message

Error codes are returned by all API when the answer is not HTTP-OK. It's displayed in {% if page.subsection == 'b2b' %}[Reference]({{ site.baseurl }}/webapi/b2b/api-reference/){% elsif page.subsection == "b2c" %}[Reference]({{ site.baseurl }}/webapi/b2c/api-reference/){% endif %} with the **Default** button on the right panel.

The structure of the error message will always be like:

```json
{
    "code": 40499,
    "uuid": "494f61d1-472a-4696-ac3c-2961496c3aaf",
    "message": "No data availble for such context.",
    "timestamp": "2020-01-01T00:00:00.000Z"
}
```

- `code` is an enhanced HTTP error code (the first 3 digits are HTTP standard error and the 2 last digits are dedicated error code for Web API). 
- `uuid` is the unique identifier of this message. It can be used to get in touch with support and help to understand why this error happened.
- `message` is a text explaining the nature of the error.
- `timestamp` is the date and time when the error happened.

> **Note:** If you encounter any unexpected error do not forget to save the error message. Contacting Stellantis support with the error message (code, uuid and timestamp), we will be able to investigate on this.

# Rate Limit

According to your subscription to Stellantis API for ex Groupe PSA brands (Citroën, DS, Peugeot, Opel and Vauxhall), they are limited amounts of API calls you can send during a period of time:
- The **day** limit is a sliding window of 24 hours.
- The **burst** limit is a maximum of instantaneous requests during an interval of 1 second.

## Prevent Limiting

The rate limit of your subscription should be sized to your need in terms of requests (burst & daily). 

However, it is possible to track your number of remaining call(s) looking at the header of the HTTP response.

```http
HTTP/1.1 200 OK
X-RateLimit-Limit-1: 100000
X-RateLimit-Remaining-1: 90502
X-RateLimit-Limit-2: 100
X-RateLimit-Remaining-2: 47
```

Field Name | Description
--- | ----
**X-RateLimit-Limit-1** | Number of calls allowed during the day limit.
**X-RateLimit-Limit-2** | Number of calls allowed during the burst limit.
**X-RateLimit-Remaining-1**  | Number of calls remaining before reaching the day limit. Equals to 0 when the limit is reached.
**X-RateLimit-Remaining-2**  | Number of calls remaining before reaching the burst limit. Equals to 0 when the limit is reached.

## Reaching The Limit

When you reach the limit, you will not be able to receive normal information from the API, you will only receive *HTTP 429* responses with the following HTTP headers:

```http
HTTP/1.1 429 TOO MANY REQUESTS
X-RateLimit-Limit-1: 100000
X-RateLimit-Remaining-1: 90502
X-RateLimit-Limit-2: 100
X-RateLimit-Remaining-2: 47
X-RateLimit-Reset: 1045
Retry-After: 1045
```


Field Name | Description
--- | ----
**X-RateLimit-Limit-1** | Number of calls allowed during the day limit.
**X-RateLimit-Limit-2** | Number of calls allowed during the burst limit.
**X-RateLimit-Remaining-1**  | Number of calls remaining before reaching the day limit. Equals to 0 when the limit is reached.
**X-RateLimit-Remaining-2**  | Number of calls remaining before reaching the burst limit. Equals to 0 when the limit is reached.
**X-RateLimit-Reset** & **Retry-After**  | Number of second(s) remaining before *X-RateLimit-Remaining* will not be equals to 0 anymore. These fields are displayed only if *X-RateLimit-Remaining* is equals to 0.

Using these headers, you can wait during the *Reset time* before sending another request to the API.

If the rate limit of your subscription does not fit to your needs, you should contact Stellantis in order to increase the ratios.
