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

    <code>Public Key</code>

.. |accountName| raw:: html

    <code>Account Name</code>

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

The example below shows registering an anonymous user.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         string username = "TestUser";

         var anonymousUser = await client.RegisterAnonymousUserAsync(username);
         
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

-----
Login
-----

All data interaction must be done on behalf of a user. This is done to ensure proper authorized access of your data.

The example below shows logging in an anonymous user.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var  connection = await client.LoginAnonymouslyAsync(username);
         
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
 
Once we login we can access our connection through a static member.

.. tabs::

   .. group-tab:: C#

      .. code-block:: c#

         connection = MeshyClient.CurrentConnection;

-----------
Create data
-----------
Now that we have an connection we can begin making API requests.

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

         var person = await MeshyClient.CurrentConnection.Meshes.CreateAsync(new Person() {
           FirstName = "Bob",
           LastName = "Bobson",
           UserId = anonymousUser.Id
         });

      |parameters|

      No parameters provided.

.. rubric:: Responses

201 : Created
   * Result of newly created mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Bob",
      "lastName": "Bobson",
      "userName": "5c..."
   }

-----------
Update data
-----------
The API allows you to make updates to specific |meshData| by targeting the id.

The SDK makes this even simpler since the id can be derived from the object itself along with all it's modifications.

The example below shows modifying the first name and committing those changes to the API.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         person.FirstName = "Robert";

         person = await MeshyClient.CurrentConnection.Meshes.UpdateAsync(person);

      |parameters|

      No parameters provided.

.. rubric:: Responses

200 : OK
   * Result of updated mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Robert",
      "lastName": "Bobson"
   }

-----------
Search data
-----------

The API allows you to search |meshData| using a Linq expression.

The example below shows searching based where the first name starts with Rob.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         var filter = "{ \"firstName\": \"^Rob\" }";
         var sort = "";
         var page = 1;
         var pageSize = 25;

         var pagedPersonResult = await MeshyClient.CurrentConnection
                                                  .Meshes
                                                  .SearchAsync<Person>(filter, 
                                                                       sort, 
                                                                       page, 
                                                                       pageSize);

      |parameters|

      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      sort : :type:`string`
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
                     "firstName": "Robert",
                     "lastName": "Bobson"
                  }],
      "totalRecords": 1
   }

..  In some cases you may need more control on your filtering or sorting. You can optionally provide this criteria in a MongoDB format.

-----------
Delete data
-----------

The API allows you to delete a specific |meshData| by targeting the id.

The example below shows deleting the data from the API by providing the object.

.. |softDelete| raw::html
   
   <code>Soft Delete</code>

*Deleted* data is not able to be recovered. If you anticipate the need to recover this data please implementing a |softDelete|.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         await MeshyClient.CurrentConnection.Meshes.DeleteAsync(person.Id);

      |parameters|

      No parameters provided.

.. rubric:: Responses

200 : OK
   * Mesh has been deleted successfully.

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

         await MeshyClient.CurrentConnection.SignoutAsync();
         
      |parameters|

      No parameters provided. The connection is aware of who needs to be signed out.

.. rubric:: Responses

200 : OK
   * Identifies successful logout.

Not seeing something you need? Feel free to give us a chat or contact us at support@meshydb.com.