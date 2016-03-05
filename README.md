**Since I am no longer working with Angular, I am no longer maintaining this project**


# Angular Mockable HTTP Provider

**NOTE: Version 0.3+ is designed to work with Angular 1.3.x, use version 0.2.x if you are using angular 1.2.x.å**

This is not designed as a replacement to $httpBackend as this will never do some of the things $httpBackend can (like verifying the number of times a request was called or verifying there are no more expected calls left).

The primary thing right now that this component can do that I could not find any easy way to do with $httpBackend is to be able to add delay to the mocked responses. I use DalekJS instead of Karma to unit test my UI (why I don't use Karma is a whole other conversion outside the scope of this component) and only certain things can be tested properly if there is a delay in the mocked response. For example, I might display some sort of loading indicator when fetching data so if the data is returned near instant like it is with $httpBackend, I can't verify that type of functionality.  The only thing I could find with adding delays to response with $httpBackend would add the delay to all responses which is not very practical.

It should also be known this component in specifically designed for testing and as such things like performance and what not will never be something worried about (it is highly recommended against using this in production).

# Examples

To use this, include the javascript file and add the httpMockable module to you application like:

```javascript
angular.module('app', ['httpMockable']);
```

The way to add a mocked response is by using the register command of the httpMocker service.  The signature of that method is as follows:

```javascript
httpMocker.register(method, url, response, options)
```

The method parameter is a string and any HTTP method (GET, POST, PUT, PATCH, DELETE, ... you get the point).  You can also use JSONP there too.

The url parameter is a string with the url that should match in order to have the mocked response returned.  Note that if the options have a payload defined, that must also match in order to return the mocked response.

The response parameter is a string of the mocked response to be returned.

The options parameter is an object that can container the following options:

- statusCode (int): The HTTP response to code return with the mocked response (defaults to 200)
- headers (string): Any headers you with to be returned with the mocked response (defaults to empty string)
- payload (string): The expected payload with the request in order to be match with the mocked response (defaults to undefined)
- delay (int): The number of millisecond to delay the response (defaults to 0)

Here is an example of defined a mocked response:

```javascript
  httpMocker.register('GET', '/api/v1/users_custom_response?itemsPerPage=3', JSON.stringify({
    status: "success",
    httpStatusCode: 200,
    data: [{
      id: 1,
      firstName: 'Test',
      lastName: 'User1',
      username: 'testuser1'
    }, {
      id: 2,
      firstName: 'Test',
      lastName: 'User2',
      username: 'testuser2'
    }, {
      id: 3,
      firstName: 'Test',
      lastName: 'User3',
      username: 'testuser3'
    }],
    totalRecords: 3
  }), {
    statusCode: 200,
    delay: 500
  });
```
