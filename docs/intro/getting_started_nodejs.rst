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

    <code><a href="#identify-public-key">Public Key</a></code>

.. |accountName| raw:: html

    <code><a href="#identify-account-name">Account Name</a></code>

Once you verify your account you will need to gather your |accountName| and |publicKey|.

Identify Account Name
~~~~~~~~~~~~~~~~~~~~~

Your |accountName| can be found under the Account page. See image below:

.. |gettingStartedAccount| image:: https://cdn.meshydb.com/images/getting-started-account.png
           :alt: Account Name under Account

|gettingStartedAccount|

Identify  Public Key
~~~~~~~~~~~~~~~~~~~~

Your |publicKey| can be found under the Clients page under your default tenant. See image below:

.. |gettingStartedClient| image:: https://cdn.meshydb.com/images/getting-started-client.png
           :alt: Public Key under Clients default tenant

|gettingStartedClient|

In the following we will assume no other configuration has been made to your account or tenants so we can just begin!

.. |parameters| raw:: html

   <h4>Parameters</h4>

-----------
Install SDK
-----------
The supporting SDK is open source and you are able to use a browser or NodeJS application.

Let's install the `MeshyDB.SDK <https://www.npmjs.com/package/@meshydb/sdk/>`_ NPM package with the following command:

.. code-block:: console

   npm install @meshydb/sdk

----------
Initialize
----------

The client is used to establish a connection to the API. You will need to provide your |accountName| and |publicKey| from your account and client.

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

Anonymous users are great for associating data to people or devices without having them go through any type of user registration.

The example below shows verifying a username is available and registering an anonymous user if the username does not exist.

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         var username = "mctesterton";

         var userExists = await client.checkUserExist(username);

         if (!userExists.exists) {
            await client.registerAnonymousUser(username);
         }

      |parameters|

      username : :type:`string`
         Unique user name for authentication. If it is not provided a username will be automatically generated.

.. rubric:: Responses

201 : Created
   * New user has been registered and is now available for use.

Example Result

.. code-block:: json

   {
      "id": "5c78cc81dd870827a8e7b6c4",
      "username": "mctesterton",
      "firstName": null,
      "lastName": null,
      "verified": false,
      "isActive": true,
      "phoneNumber": null,
      "emailAddress": null,
      "roles": [],
      "securityQuestions": [],
      "anonymous": true,
      "lastAccessed":"2019-01-01T00:00:00.0000+00:00"
   }

400 : Bad request
   * Username is a required field.
   * Anonymous registration is not enabled.
   * Username must be unique.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----
Login
-----

All data interaction must be done on behalf of a user. This is done to ensure proper authorized access of your data.

The example below shows logging in an anonymous user.

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         var connection = await client.loginAnonymously(username);

      |parameters|

      username : :type:`string`, :required:`required`
         Unique user name for authentication.

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
   * You have either hit your API or Database limit. Please review your account.

Once we login we can access our connection through a static member.

.. tabs::

   .. group-tab:: NodeJS

      .. code-block:: javascript

         connection = MeshyClient.currentConnection;

---------------
Retrieving Self
---------------

When a user is created they have some profile information that helps identify them. We can use this information to link their id back to data that has been created.

The example below shows retrieving information of the user.

.. tabs::

   .. group-tab:: NodeJS
   
      .. code-block:: javascript
      
         var user = await connection.usersService.getSelf();

      |parameters|
      
      No parameters provided.

.. rubric:: Responses

200 : OK
   * Retrieves information about the authorized user.

Example Result

.. code-block:: json

   {
      "id": "5c78cc81dd870827a8e7b6c4",
      "username": "mctesterton",
      "firstName": null,
      "lastName": null,
      "verified": false,
      "isActive": true,
      "phoneNumber": null,
      "emailAddress": null,
      "roles": [],
      "securityQuestions": [],
      "anonymous": true,
      "lastAccessed":"2019-01-01T00:00:00.0000+00:00"
   }

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.
   
-----------
Create data
-----------

Now that a user connection is established you can begin making API requests.

The example below shows committing a new person.

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         var model = {
                        _id: undefined,
                        firstName: "Bob",
                        lastName: "Bobson",
                        userId: user.id
                     };

         var meshName = "person";

         model = await connection.meshesService.create(meshName, model);

      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies which mesh collection to manage.
      model : :type:`object`, :required:`required`
         Represents a person in this example.

.. rubric:: Responses

201 : Created
   * Result of newly created mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5d438ff23b0b7dd957a765ce",
      "firstName": "Bob",
      "lastName": "Bobson",
      "userId": "5c78cc81dd870827a8e7b6c4"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to create meshes or mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Update data
-----------

The API allows you to make updates to specific Mesh Data by targeting the id.

The SDK makes this even simpler since the id can be derived from the object itself along with all it's modifications.

The example below shows modifying the first name and committing those changes to the API.

.. tabs::

   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         model.firstName = "Robert";

         model = await connection.meshesService.update(meshName, model);

      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies which mesh collection to manage.
      model : :type:`object`, :required:`required`
         Represents a person in this example.

.. rubric:: Responses

200 : OK
   * Result of updated mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5d438ff23b0b7dd957a765ce",
      "firstName": "Robert",
      "lastName": "Bobson",
      "userId": "5c78cc81dd870827a8e7b6c4"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to update meshes or mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Search data
-----------

The API allows you to search a mesh collection using a MongoDB expression.

The example below shows searching based where the first name starts with Rob.

.. tabs::

   .. group-tab:: NodeJS
   
      .. code-block:: javascript
	  
         var filter = { 'firstName': { "$regex": "^Rob" } };

         var pagedPersonResult = await connection.meshesService
                                                 .search(meshName, { filter: filter });

      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies which mesh collection to manage.
      filter : :type:`object`
         Criteria provided in a MongoDB expression to limit results.

.. rubric:: Responses

200 : OK
   * Mesh data found with given search criteria.

Example Result

.. code-block:: json

   {
      "page": 1,
      "pageSize": 25,
      "results":  [{
                     "_id":"5d438ff23b0b7dd957a765ce",
                     "firstName": "Robert",
                     "lastName": "Bobson",
                     "userId": "5c78cc81dd870827a8e7b6c4"
                  }],
      "totalRecords": 1
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Filter is in an invalid format. It must be in a valid Mongo DB format.
   * Order by is in an invalid format. It must be in a valid Mongo DB format.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to read meshes or mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Delete data
-----------

The API allows you to delete a specific Mesh Data by targeting the id.

The example below shows deleting the data from the API by providing the object.

.. |softDelete| raw:: html
   
   <code>Soft Delete</code>

*Deleted* data is not able to be recovered. If you anticipate the need to recover this data please implementing a |softDelete|.

.. tabs::

   .. group-tab:: NodeJS
   
      .. code-block:: javascript
      
         var id = model._id;

         await connection.meshesService.delete(meshName, id);

      |parameters|
      
      meshName : :type:`string`, :required:`required`
         Identifies which mesh collection to manage.
      id : :type:`string`, :required:`required`
         Identifier of record that must be deleted.

.. rubric:: Responses

204 : No Content
   * Mesh has been deleted successfully.

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to delete meshes or mesh.

404 : Not Found
   * Mesh data was not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

--------
Sign out
--------

The MeshyDB SDK manages a single connection to the API. 

The Meshy SDK handles token management, this includes refresh tokens used to maintain a user's connection.

As a result it is recommended to implement Sign Out to ensure the current user is logged out and all refresh tokens are revoked.

The example below shows signing out of the currently established connection.

.. tabs::

   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         await connection.signout();
         
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
   * You have either hit your API or Database limit. Please review your account.

Not seeing something you need? Feel free to give us a chat or contact us at support@meshydb.com.