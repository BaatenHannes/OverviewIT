# REST API

## Return types

[OVERZICHT](http://shengwangi.blogspot.com/2016/02/response-for-get-post-put-delete-in-rest.html)
[Link to Microsoft Documentations](https://docs.microsoft.com/en-us/aspnet/core/web-api/action-return-types?view=aspnetcore-2.2)



### POST

* 201 Created
* 400 Bad Request

Return the path to get the created item. You can also include the created item in the body.
```return CreatedAtAction(nameof(GetById), new { id = product.Id }, product);```


### PUT/PATCH

* 204 NoContent
* 400 Bad Request
* 404 Not Found

### GET BY ID

* 200 Ok
* 404 Not Found

### GET ALL

* 200 Ok (returns empty list if no objects are found)