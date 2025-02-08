---
sidebar_position: 4
slug: /api-reference/pagination
---

# Pagination

## Available properties

- `limit`, Define the limit of items to be displayed per query, the maximum is 100.
- `page`, The page you want to show of the total items.

### Example use `limit` property

```cli
https://apitcg.com/api/pokemon/cards?limit=60
``` 

### Example use `page` property

```cli
https://apitcg.com/api/pokemon/cards?page=2
``` 

### Example full request

```cli
https://apitcg.com/api/pokemon/cards?name=charizard&limit=50&page=2
``` 



