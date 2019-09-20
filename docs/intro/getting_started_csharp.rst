.. role:: required

.. role:: type

===
C#
===

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
The supporting SDK is open source and you are able to use .Net Framework 4.7.1+ or .Net Core 2.x.

Let's install the `MeshyDB.SDK <https://www.nuget.org/packages/MeshyDB.SDK/>`_ NuGet package with the following command:

.. code-block:: powershell

   Install-Package MeshyDb.SDK

----------
Initialize
----------
The client is used to establish a connection to the API. You will need to provide your |accountName| and |publicKey| from your account and client.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
   
         var client = MeshyClient.Initialize(accountName, publicKey);
         
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
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var username = "mctesterton";

         var userExists = await client.CheckUserExistAsync(username);

         if (!userExists.Exists)
         {
            await client.RegisterAnonymousUserAsync(username);
         }

      |parameters|

      username : :type:`string`
         Unique identifier for user or device. If it is not provided a username will be automatically generated.

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

All data interaction must be done on behalf of a user. This is done to ensure proper authorized access of your data.

The example below shows logging in an anonymous user.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var connection = await client.LoginAnonymouslyAsync(username);

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

Once we login we can access our connection through a static member.

.. tabs::

   .. group-tab:: C#

      .. code-block:: c#

         connection = MeshyClient.CurrentConnection;

---------------
Retrieving Self
---------------

When a user is created they have some profile information that helps identify them. We can use this information to link their id back to data that has been created.

The example below shows retrieving information of the user.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var user = await connection.Users.GetSelfAsync();

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
      "anonymous": true
   }

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.
   
-----------
Create data
-----------

Now that a user connection is established you can begin making API requests.

.. |meshData| raw:: html

    <code>MeshData</code>
    
The MeshyDB SDK requires all data extend the |meshData| class. 

The example below shows a Person represented by a first name, last name and user id.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
         
         // Mesh Name can be overridden by attribute, otherwise by default it is derived from class name
         [MeshName("Person")]
         public class Person : MeshData
         {
            public string FirstName { get; set; }
            public string LastName { get; set; }
            public string UserId { get; set; }
         }

Now that we have a representation of a person we can start making data to write to the API.

The example below shows committing a new person.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var model = new Person()
         {
            FirstName = "Bob",
            LastName = "Bobson",
            UserId = user.Id
         };

         model = await connection.Meshes.CreateAsync(model);

      |parameters|

      model : :type:`object`, :required:`required`
         Representation of data that *must* extend |meshData|.

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

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

-----------
Update data
-----------

The API allows you to make updates to specific |meshData| by targeting the id.

The SDK makes this even simpler since the id can be derived from the object itself along with all it's modifications.

The example below shows modifying the first name and committing those changes to the API.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         model.FirstName = "Robert";

         model = await connection.Meshes.UpdateAsync(model);
      
      |parameters|

      model : :type:`object`, :required:`required`
         Representation of data that *must* extend |meshData|.

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

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

-----------
Search data
-----------

The API allows you to search |meshData| using a Linq expression.

The example below shows searching based where the first name starts with Rob.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

            Expression<Func<Person, bool>> filter = (Person x) => x.FirstName.StartsWith("Rob");

            var pagedPersonResult = await connection.Meshes
                                                    .SearchAsync<Person>(filter);

      |parameters|

      filter : :type:`object`
         Criteria provided in a Linq expression to limit results.

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
   
429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

In some cases you may need more control on your filtering or ordering. You can optionally provide this criteria in a MongoDB format.

-----------
Delete data
-----------

The API allows you to delete a specific |meshData| by targeting the id.

The example below shows deleting the data from the API by providing the object.

.. |softDelete| raw:: html
   
   <code>Soft Delete</code>

*Deleted* data is not able to be recovered. If you anticipate the need to recover this data please implementing a |softDelete|.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
            var id = model.Id;

            await connection.Meshes.DeleteAsync<Person>(id);

      |parameters|

      id : :type:`string`, :required:`required`
         Identifier of record that must be deleted.

.. rubric:: Responses

204 : No Content
   * Mesh has been deleted successfully.

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.

401 : Unauthorized
   * User is not authorized to make call.

404 : Not Found
   * Mesh data was not found.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

--------
Sign out
--------

The MeshyDB SDK manages a single connection to the API. 

The Meshy SDK handles token management, this includes refresh tokens used to maintain a user's connection.

As a result it is recommended to implement Sign Out to ensure the current user is logged out and all refresh tokens are revoked.

The example below shows signing out of the currently established connection.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         await connection.SignoutAsync();
         
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

Not seeing something you need? Feel free to give us a chat or contact us at support@meshydb.com.