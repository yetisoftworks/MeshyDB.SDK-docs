.. role:: required

.. role:: type

======
NodeJS
======

----------------------
Before we get started!
----------------------
Let's go to create a free account at `https://meshydb.com <https://meshydb.com/>`_.

Once we verify our account we want to gather our Public Key from our default tenant under Clients.

.. images:: http://cdn.meshydb.com/images/getting-started-client.png

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
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.

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
   
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var connection = await client.loginAnonymously(anonymousUser.username);

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
         Idenfities location of what Mesh data to replace.


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
         Filter criteria for search. Uses MongoDB format.
      orderby : :type:`string`
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


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         await MeshyClient.currentConnection
                          .meshes
                          .delete(meshName, person._id);
         
      |parameters|

      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Idenfities location of what Mesh data to replace.

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

      No parameters provided. The client is aware of who needs to be signed out.

Upon signing out we will clear our connection allowing another user to now be logged in.