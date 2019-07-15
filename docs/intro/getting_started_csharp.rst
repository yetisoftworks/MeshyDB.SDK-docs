.. role:: required

.. role:: type

===
C#
===

----------------------
Before we get started!
----------------------
Let's go to create a free account at `https://meshydb.com <https://meshydb.com/>`_.

Once we verify our account we want to gather our Public Key from our default tenant under Clients.

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
The supporting SDK is open source and you are able to use .Net Framework 4.7.1+ or .Net Core 2.x.

Let's install the `MeshyDB.SDK <https://www.nuget.org/packages/MeshyDB.SDK/>`_ NuGet package with the following command:

.. code-block:: powershell

   Install-Package MeshyDb.SDK

----------
Initialize
----------
Let's start with initializing our MeshyDB Client. This will allow us to register a new user next! 

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
   
         var client = MeshyClient.Initialize(accountName, publicKey);
         
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.

-----------------------
Register Anonymous User
-----------------------
Anonymous users are great for associating data to people without having them go through any type of user registration. Simply create the user behind the scenes with a unique identifier and begin recording data for that user.

We will register an anonymous user using our initialized client.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         string username = null;

         var anonymousUser = await client.RegisterAnonymousUserAsync(username);
         
      |parameters|

      username : :type:`string`
         Identifies user to allow login. If it is not provided a username will be automatically generated.

Example Response:

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
All data interaction must be done on behalf of a user. To start interacting with data establish a connection as that user.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var  connection = await client.LoginAnonymouslyAsync(anonymousUser.Username);
         
      |parameters|

      username : :type:`string`, :required:`required`
         Identifies user to allow login.

Example Response:

.. code-block:: json

  {
    "access_token": "ey...",
    "expires_in": 3600,
    "token_type": "Bearer",
    "refresh_token": "ab23cd3343e9328g"
  }
 
Once we login we can access our connection staticly.

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
   
   .. group-tab:: C#
   
      .. code-block:: c#
         
         // Mesh Name can be overridden by attribute, otherwise by default it is derived from class name
         [MeshName("Person")]
         public class Person : MeshData
         {
           public string FirstName { get; set; }
           public string LastName { get; set; }
         }

         var person = await MeshyClient.CurrentConnection.Meshes.CreateAsync(new Person() {
           FirstName="Bob",
           LastName="Bobberson"
         });

      |parameters|

      No parameters provided.

Example Response:

.. code-block:: json

  {
    "_id":"5c78cc81dd870827a8e7b6c4",
    "firstName": "Bob",
    "lastName": "Bobberson"
  }

-----------
Update data
-----------
If we need to make a modification let's update our Mesh! 

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         person.FirstName = "Bobbo";

         person = await MeshyClient.CurrentConnection.Meshes.UpdateAsync(person);

      |parameters|

      No parameters provided.

Example Response:

.. code-block:: json

  {
    "_id":"5c78cc81dd870827a8e7b6c4",
    "firstName": "Bobbo",
    "lastName": "Bobberson"
  }

-----------
Search data
-----------
Let's see if we can find Bobbo.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         var filter = "{ \"firstName\": \"Bobbo\" }";
         var sort = "";
         var page = 1;
         var pageSize = 25;

         var pagedPersonResult = await MeshyClient.CurrentConnection.Meshes.SearchAsync<Person>(filter, sort, page, pageSize);

      |parameters|

      filter : :type:`string`
         Filter criteria for search. Uses MongoDB format.
      sort : :type:`string`
         How to order results. Uses MongoDB format.
      page : :type:`integer`
         Page number of users to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

Example Response:

.. code-block:: json

  {
    "page": 1,
    "pageSize": 25,
    "results": [{
                 "_id":"5c78cc81dd870827a8e7b6c4",
                 "firstName": "Bobbo",
                 "lastName": "Bobberson"
               }],
    "totalRecords": 1
  }

-----------
Delete data
-----------
We are now done with our data, so let us clean up after ourselves.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         await MeshyClient.CurrentConnection.Meshes.DeleteAsync(person);

      |parameters|

      No parameters provided.

--------
Sign out
--------
Now the user is complete. Let us sign out so someone else can have a try.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         await MeshyClient.CurrentConnection.SignoutAsync();
         
      |parameters|

      No parameters provided. The client is aware of who needs to be signed out.

Upon signing out we will clear our connection allowing another user to now be logged in.