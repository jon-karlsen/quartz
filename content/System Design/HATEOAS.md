---
title: Hypermedia as the Engine of Application State
tags:
    - system-design
    - hateoas
    - rest-api
---

_Hypermedia as the Engine of Application State_ is a constraint of [[REST API]] architecture wherein a client interacts with a network application entirely through hypermedia provided dynamically by application servers. This can make APIs more self-descriptive, discoverable, and adaptable to changes.

In practice, this means that a REST API response, in addition to data, links to related resources. The client navigates the API by following these links rather than having to know the API structure in advance.

For example, when retrieving a user record, the response might include links to view that user's orders, update their profile, or perform other related actions:

```json
{
  "id": 123,
  "name": "Jane Smith",
  "email": "jane@example.com",
  "links": [
    {"rel": "self", "href": "/users/123"},
    {"rel": "orders", "href": "/users/123/orders"},
    {"rel": "update", "href": "/users/123", "method": "PUT"},
    {"rel": "delete", "href": "/users/123", "method": "DELETE"}
  ]
}
```

