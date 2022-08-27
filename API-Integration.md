# **API Integration**

## **Dynamic API Server**
An Express/Node.js based server designed to be a “model agnostic” REST API server, which can perform CRUD operations on any data model

## **Business Requirements**
We’re opening a online store! In order to support our web application, we need to create an API that will serve specific data (namely, categories and products) to our application. We will write an API to interface with our databases to provide category and product information to our front end app.

As it is highly likely that we will need more data types and sources in the future, it’s imperative that we build this API as a standardized means of working with any data model, using any persistence system, though a common interface. The API Server must operate as follows:

## **Support all REST/HTTP methods**
   -  GET: Retrieve record(s) from a data source
      -  All
      -  One (by id)
      -  Some (by filtering)
   -  POST: Create a new record in a data source
   -  PUT: Update a single full record in a data source
   -  PATCH: Update part of a single record in a data source
   -  DELETE: Delete a record in a data source

## **Obey a standard routing structure**
i.e. (http://amazingapi.com/api/v1/products/12345)[http://amazingapi.com/api/v1/products/12345]

   -  /api/v# where # is the version number of our API
      -  /model where ‘model’ is the name of the data model to operate on
         -  /id where ‘id’ is the id number of a specific entity in the data model
   -  Allow for Query String parameters for filtering
      -  i.e. http://amazingapi.com/api/v1/products?category=electronics
      -  This would GET every entry in our products data model where the category is ‘electronics’

## **Obey a standard output format**
   -  Results will be returned in JSON format
   -  Results will be served with the correct HTTP header - application/json
   -  The GET Route, when not retrieving by ID, must return a full header, with count ,pages, and a results array
   -  All other routes must return a single object, representing the state of the entity following the operation

## **Examples**
   -  **GET MANY**
```js
{
    count: 300,
    previous: null,
    next: http://amazingapi.com/api/v1/products?page=2
    results: [
      { name: 'camera', ... },
      { name: 'phone', ... },
      { name: 'tv', ... },
      ...
    ]
}
```
   -  **GET ONE**
```js
{
    id: 12345,
    name: 'camera',
    manufacturer: 'Nikon',
    model: 'xx435',
    price: 99.99,
    inStock: 2200,
    weight: 1.1
}
```

## **Authentication Server / Module**
An Express/Node.js based server using a custom “authentication” module that is designed to handle user registration and sign in using Basic, Bearer, or OAuth along with a custom “authorization” module that will grant/deny users access to the server based on their role or permissions level.

## **Business Requirements**
Our business is expanding! We have a real need to manage a list of users of many types, and control their access to our data accordingly. The system to be built will have the following core features:

Users can create an account, associated with a “role”
User Roles will be pre-defined and will each have a list of allowed capabilities
admin can read, create, update, delete
editor can read, create, update
writer can read, create
user can read
Users can then login with a valid username and password
Alternatively, users can login using an OAuth provider such as Google or GitHub
In this case, users should be automatically assigned the role of user
Once logged in, Users can then access any route on the server, so long as they are permitted by the capabilities that match their role.
For example, a route that deletes records should only work if your user role is “admin”
Technical Requirements
The application will be created with the following overall architecture and methodologies

Node.js
ES6 Classes and best practices
ExpressJS Web Server, built modularly
“Auth” routes for handling the login and authentication system
POST /signup to create an account
POST /signin to login with Basic Auth
GET /oauth
Middleware for handling 404 and 500 conditions
Middleware for handling each type of authentication
Basic (username + password) to be used on the /signin route only
i.e. app.post('/signin', basicAuthentication, (req, res) => { ... })
OAuth (3rd Party) to be used on the /oauth route only
i.e. app.get('/oauth', OAuth, (req, res) => { ... })
Handles the handshake process from a 3rd party OAuth system
Bearer (token) to be used on any other route in the server that requires a logged in user
i.e. app.get('/secretstuff', tokenAuthentication, (req, res) => { ... })
Middleware for handling authorization
To be run after Bearer Middleware on routes to be protected
Accepts a “capability” as a parameter
i.e. app.delete('/secretstuff', tokenAuthRequired, capability('delete'), (req, res) => { ... })
User Persistence using a Mongo Database (NoSQL)
Mongoose Schemas (data models) to define and model data
Test Driven Development, using Jest
Tests will be runnable locally
Tests will auto-execute (CI) in your repo using GitHub actions
Tests will use a 3rd party library called supergoose to:
“mock” the mongo running database
“mock” the running Express server
Deployment to Heroku

Signup
The signup process will require a POST request to the /signup route.

This route should accept a request body in either JSON or form/urlencoded formats.
**POST** http://yourauthserver.com/signup
```js
  {
    "username":"awesomecoder",
    "password":"youcantguessthis33!",
    "fullname":"Awesome Coder",
    "email":"awesome@coders.com"
  }
```
Output from the server:

HTTP Headers:

Set-Cookie: auth=ENCRYPTED.JWT.TOKEN; Path=/
token:ENCRYPTED.JWT.TOKEN
Content-Type: application/json
**BODY:**
```js
{
    "token": "ENCRYPTED.JWT.TOKEN",
    "user": {
        "__v": 0,
        "_id": "5ec44dcde458f14f77b3d8c6",
        "acl": [],
        "password": "$2b$10$Bw5tNNSUACEEHW94CaRItuLsoz/nomUMh4eLTD/T23Ks0mCtUp3Iq",
        "role": "user",
        "username": "awesomecoder"
    }
}
```

---

[Reading Page](./README.md)

---