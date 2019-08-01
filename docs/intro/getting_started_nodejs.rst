.. role:: required

.. role:: type

======
NodeJS
======

----------------------
Before you get started!
----------------------
This documentation assumes you have an active MeshyDB account. If you do not, please create a free account at `https://meshydb.com <https://meshydb.com/>`_.

.. |publicKey| raw:: html

    <code>Public Key</code>

Once your account is verified, you will need to gather your |publicKey| from the Clients page under your default tenant. See image below:

.. |gettingStarted| image:: https://cdn.meshydb.com/images/getting-started-client.png
           :alt: Public Key under Clients default tenant

|gettingStarted|

In the following we will assume no other configuration has been made to your account or tenants so we can just begin!

Now that we have the required information let's jump in and see how easy it is to start with MeshyDB.

.. |parameters| raw:: html

   <h4>Parameters</h4>
  
-----------
Install SDK
-----------
Now that we have the required information let's start coding!

The supporting SDK is open source and you are able to use a browser or NodeJS application.

Let's install the `MeshyDB.SDK <https://www.npmjs.com/package/@meshydb/sdk/>`_ NPM package with the following command:

.. code-block:: console

   npm install @meshydb/sdk

----------
Initialize
----------
Let's start with initializing our MeshyDB Client. This will allow us to register a new anonymous user next! 

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript
   
         var client = MeshyClient.initialize(accountName, publicKey);
         
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.

-----------------------
Register Anonymous User
-----------------------
Anonymous users are great for associating data to people without having them go through any type of user registration. Simply create the user behind the scenes with a unique identifier and begin recording data for that user.

We will register an anonymous user using our initialized client.

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         var username = null;

         var anonymousUser = await client.registerAnonymousUser(username);

      |parameters|

      username : :type:`string`
         Unique identifier for user or device. If it is not provided a username will be automatically generated.

.. rubric:: Responses

201 : Created
   * New user has been registered and is now available for use.

Example Result

.. code-block:: json

   {
      "id": "5c...",
      "username": "2d4c2a18-2596-4ba9-b657-3413d5974502",
      "firstName": null,
      "lastName": null,
      "verified": false,
      "isActive": true,
      "phoneNumber": null,
      "emailAddress": null,
      "roles": [],
      "securityQuestions": [],
      "anonymous": true
   }

400 : Bad request
   * Username is a required field.
   * Anonymous registration is not enabled.
   * Username must be unique.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

-----
Login
-----
All data interaction must be done on behalf of a user. To start interacting with data establish a connection as that user.

.. tabs::
   
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var connection = await client.loginAnonymously(anonymousUser.username);

      |parameters|

      username : :type:`string`, :required:`required`
         Unique identifier for user or device.


.. rubric:: Responses

200 : OK
   * Generates new credentials for authorized user.

Example Result

.. code-block:: json

   {
      "access_token": "ey...",
      "expires_in": 3600,
      "token_type": "Bearer",
      "refresh_token": "ab23cd3343e9328g"
   }

400 : Bad request
   * Token is invalid.
   * Client id is invalid.
   * Grant type is invalid.
   * User is no longer active.
   * Invalid Scope.
   * Username is invalid.
   * Password is invalid.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

Once we login we can access our connection staticly after we ensure a successful login.

.. tabs::

   .. group-tab:: C#

      .. code-block:: c#

         connection = MeshyClient.CurrentConnection;

-----------
Create data
-----------
We can use our newly authenticated user to make requests with MeshyDB and create some data.

The data object can whatever information you would like to capture. The following example will have some data fields with example data.

.. tabs::
   
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
        
         var person = {
                        firstName:"Bob",
                        lastName:"Bobberson"
                      };
         
         var meshName = "person";
         
         var person = await MeshyClient.currentConnection.meshesService.create(meshName, person);

      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.

.. rubric:: Responses

201 : Created
   * Result of newly created mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Bob",
      "lastName": "Bobberson"
   }

400 : Bad request
   * Mesh name is invalid and must contain alpha numeric.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

-----------
Update data
-----------
If we need to make a modification let's update our Mesh! 

.. tabs::

   .. group-tab:: NodeJS
      
      .. code-block:: javascript

        person.firstName = "Bobbo";
        
        await MeshyClient.currentConnection
                         .meshes
                         .update(meshName, person, person._id);
      
      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to replace.


.. rubric:: Responses

200 : OK
   * Result of updated mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Bobbo",
      "lastName": "Bobberson"
   }

400 : Bad request
   * Mesh name is invalid and must contain alpha numeric.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

-----------
Search data
-----------
Let's see if we can find Bobbo.

.. tabs::

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         

         var people  = await MeshyClient.currentConnection
                                        .meshes
                                        .search(meshName, 
                                                {
                                                   filter: { "firstName": "Bobbo" },
                                                   orderby: null,
                                                   pageNumber: 1,
                                                   pageSize: 25
                                                });
      
      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderby : :type:`string`
         Defines which fields need to be sorted and direction in a MongoDB format.
      page : :type:`integer`
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

.. rubric:: Responses

200 : OK
   * Mesh data found with given search criteria.

Example Result

.. code-block:: json

   {
      "page": 1,
      "pageSize": 25,
      "results":  [{
                     "_id":"5c78cc81dd870827a8e7b6c4",
                     "firstName": "Bobbo",
                     "lastName": "Bobberson"
                  }],
      "totalRecords": 1
   }

400 : Bad request
   * Mesh name is invalid and must contain alpha numeric.
   * Filter is in an invalid format. It must be in a valid Mongo DB format.
   * Order by is in an invalid format. It must be in a valid Mongo DB format.

401 : Unauthorized
   * User is not authorized to make call.
   
429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

-----------
Delete data
-----------
We are now done with our data, so let us clean up after ourselves.

.. tabs::


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         await MeshyClient.currentConnection
                          .meshes
                          .delete(meshName, person._id);
         
      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.

.. rubric:: Responses

204 : No Content
   * Mesh has been deleted successfully.

400 : Bad request
   * Mesh name is invalid and must contain alpha numeric.

401 : Unauthorized
   * User is not authorized to make call.

404 : Not Found
   * Mesh data was not found.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

--------
Sign out
--------
Now the user is complete. Let us sign out so someone else can have a try.

.. tabs::

   .. group-tab:: NodeJS
      
      .. code-block:: javascript

         await MeshyClient.currentConnection
                          .signout();
      
      |parameters|

      No parameters provided. The connection is aware of who needs to be signed out.

.. rubric:: Responses

200 : OK
   * Identifies successful logout.

400 : Bad request
   * Invalid client id.
   * Token is missing.
   * Unsupported Token type.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

Upon signing out we will clear our connection allowing another user to now be authenticated.