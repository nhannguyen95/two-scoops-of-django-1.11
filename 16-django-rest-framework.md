
|Request purpose|HTTP Method|Rough SQL equivalent|
|---|---|---|
|Create a new resource                                |POST `/books/{id}`  |INSERT|
|Read an existing resource                            |GET `/books/{id}`   |SELECT|
|Read all resources of a kind                         |GET `/books`        |SELECT|
|Update an existing resource                          |PUT `/books/{id}`   |UPDATE|
|Update part of an existing resource                  |PATCH               |UPDATE|
|Delete an existing resource                          |DELETE `/books/{id}`|DELETE|
|Returns same HTTP headers as GET, but no body content|HEAD                |-     |
|Returns the supported HTTP methods for the given URL |OPTIONS             |-     |
|Echo back the request                                |                    |      |

---

If you're implementing a **read-only API**, only need to implement **GET** methods.

If you're implementing a **read-write API**, should use **GET, POST, PUT, DELETE**.

---

By definition, GET, PUT and DELETE are idempotent. POST and PATCH are not.

---

|HTTP Status Code|Success/Failure|Meaning|
|---|---|---|
|200 OK                |S       |GET - Return resource, PUT - Provide status message or return resource                                                    |
|201 Created           |S       |POST - Provide status message or return newly created resource                                                            |
|204 No Content        |S       |PUT or DELETE - Response to successful update or delete request                                                           |
|304 Not Modified      |Redirect|ALL - Indicates no changes since the last request. Used for checking Last_modified and ETag headers to improve performance|
|400 Bad Request       |F       |ALL - Return error messages, including form validation errors                                                             |
|401 Unauthorized      |F       |ALL - Authentication required but user did not provide credentials or provided invalid ones                               |
|403 Forbidden         |F       |ALL - User attempted to access restricted content                                                                         |
|404 Not Found         |F       |ALL - Resource is not found                                                                                               |
|405 Method Not Allowed|F       |ALL - An unallowed HTTP method was attempted                                                                              |
|410 Gone              |F       |ALL - A requested resource is no longer available and won’t be available in the future. Used when an API is shut down in favor of a newer version of an API. Mobile applications can test for this condition, and if it occurs, tell the user to upgrade.|
|429 Too Many Requests |F       |ALL - The user has sent too many requests in a given amount of time. Intended for use with rate limiting                  |

---

`DEFAULT_PERMISSION_CLASSES` should be `IsAdminUser` by default.

---

Don't use Sequential Keys (i.e. default model primary keys `id` or `pk`) as Public Id for objects.

Use UUID instead to look up our records.

---

Preferable API modules structure and naming:

```
flavors/ 
├── api/  # Place all API components into this package
│   ├── __init__.py
│   ├── authentication.py
│   ├── parsers.py
│   ├── permissions.py
│   ├── renderers.py
│   ├── serializers.py
│   ├── views.py
│   ├── viewsets.py  # Viewsets belong in their own module
├── __init__.py
├── urls.py  # Place routers here
```
---

Regardless of which architectural approach you take, it’s a good idea to try to keep as much logic as possible out of API views.

Remember, at the end of the day, API views are just another type of view.

---

Good pratice to abbreviate the urls of API with the version number:
- `/app/v1/flavors`
- `/app/v2/flavors`

---

Instead of designing:
- `/api/cones/`. GET, POST
- `/api/cones/:uuid/`. GET, PUT, DELETE
- `/api/scoops/`. GET, POST
- `/api/scoops/:uuid/`. GET, PUT, DELETE

We can reason the logic of our code that there exists an one to many relationship between cones and scoops, so we can redesign and at these 2 APIs:
- `/api/cones/:uuid/scoops`. GET, POST
- `/api/cones/:uuid/scoops/:uuid/`. GET, PUT, DELETE

