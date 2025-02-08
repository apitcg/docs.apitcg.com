---
sidebar_position: 2
slug: /authentication
---

# Authentication

The API TCG uses, API Key to authenticate requests. Sign up for an account at the [API TCG Platform](https://apitcg.com/platform) to get your API key.

Authentication to the API is performed via the ```x-api-key``` header. Provide your API key in the headers of all requests to authenticate to the API TCG.



## Example request
```curl
curl --location 'https://apitcg.com/api/one-piece/cards?name=luffy' \
--header 'x-api-key: MY-API-KEY' 
```