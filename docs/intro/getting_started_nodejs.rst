.. role:: required

.. role:: type

======
NodeJS
======
The first thing we need is some MeshyDB credentials. If you have not you can get started with a free account at `MeshyDB.com <https://meshydb.com/>`_.

Once we have done that we can go to Account and get our Account Name. Next, we can go to Clients and get the Public Key.

Now that we have the required information let's jump in and see how easy it is to start with MeshyDB.

.. |parameters| raw:: html

   <h4>Parameters</h4>
  
-----------
Install SDK
-----------
Now that we have the required information let's start coding!

The supporting SDK is open source and your able to use a browser or NodeJS application.

Let's install the `MeshyDB.SDK <https://www.npmjs.com/package/@meshydb/sdk/>`_ NPM package with the following command:

.. code-block:: console

   npm install @meshydb/sdk

Now we are configured and we can get started!

----------
Initialize
----------
Let's start with initializing our MeshyDB Client. This will allow us to register a new user next! 

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript
   
         var client = MeshyClient.initialize(accountName, publicKey);
         
         // Or if we want to use a different tenant

         client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.

-------------
Register User
-------------
Using our client, we can register a user. Optionally, we can register an anonymous user and skip to logging in.

If you have yet to do any configuration through the admin portal, you will by default be required to supply security questions.

If you wish to use email or text message, we can go to Configuration under your tenant you initialized.

.. tabs::
   
   .. group-tab:: NodeJS
   
      .. code-block:: javascript

         var registerUser = { 
                              username: useranme, 
                              password: password,
                              emailAddress: "test@test.com",
                              phoneNumber: "+15551234567",
                              securityQuestions: [
                                 {
                                    question:  "What is the most magical place in the universe?",
                                    answer:  "MeshyDB!"
                                 }
                              ]
         };
         
         client.registerUser(registerUser)
               .then(function(hash){ });
         
         // Or we just register an anonymous user and take the quick way around

         client.registerAnonymousUser()
               .then(function(user){ });

      |parameters|

      username : :type:`string`, :required:`required`
         User name.
      password : :type:`string`, :required:`required`
         User password.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         Collection of questions and answers used for password recovery if question security is configured.

Once we register a user a hash may be returned. This is used to verify the newly registered user.

If we are using question verification by default it will be null since they are automatically verified.

However, if we are using text or email a verification code will be sent.

Once the verification code has been received, we will need to verify the user.


-----
Login
-----
We have a client; we have a user let's make a connection! 

.. tabs::
   
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var meshyConnection;

         client.login(username, password)
               .then(function(connection) { meshyConnection = connection; })
               .catch(function(error) { }); 

         // Or log in anonomously if we made an anonymous user
		   client.loginAnonmously(username)
               .then(function (connection) { meshyConnection = connection; })
               .catch(function(error) { });
      
      |parameters|

      username : :type:`string`, :required:`required`
         User name.
      password : :type:`string`, :required:`required`
         User password.


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

         meshyConnection = MeshyClient.CurrentConnection;

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
                      
         MeshyClient.currentConnection
                    .meshes
                    .create(meshName, person)
                    .then(function(result) { person = result; });
      
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
        
        MeshyClient.currentConnection
                   .meshes
                   .update(meshName, person, person._id)
                   .then(function(result){ person = result; });
      
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
         

         MeshyClient.currentConnection
                    .meshes
                    .search(meshName, 
                           {
                              filter: { "firstName": "Bobbo" },
                              orderby: null,
                              pageNumber: 1,
                              pageSize: 25
                           })
                    .then(function(results){ });
      
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
         
         MeshyClient.currentConnection
                    .meshes
                    .delete(meshName, person._id)
                    .then(function(_){ });
         
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

         MeshyClient.currentConnection
                    .signout()
                    .then(function(result) { });
      
      |parameters|

      No parameters provided. The client is aware of who needs to be signed out.

Upon signing out we will clear our connection allowing another user to now be logged in.