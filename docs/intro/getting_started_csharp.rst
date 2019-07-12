.. role:: required

.. role:: type

==
C#
==
The first thing we need is some MeshyDB credentials. If you have not you can get started with a free account at `MeshyDB.com <https://meshydb.com/>`_.

Once we have done that we can go to Account and get our Account Name. Next, we can go to Clients and get the Public Key.

Now that we have the required information let's jump in and see how easy it is to start with MeshyDB.

.. |parameters| raw:: html

   <h4>Parameters</h4>
  
-----------
Install SDK
-----------
Now that we have the required information let's start coding!

The  supporting SDK is open source and your able to use .Net Framework 4.7.1+ or .Net Core 2.x.

Let's install the `MeshyDB.SDK <https://www.nuget.org/packages/MeshyDB.SDK/>`_ NuGet package with the following command:

.. code-block:: powershell

   Install-Package MeshyDb.SDK

Now we are installed and we can get started!

----------
Initialize
----------
Let's start with initializing our MeshyDB Client. This will allow us register a new user next!

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
   
         var client = MeshyClient.Initialize(accountName, publicKey);
         
         // Or if we want to use a different tenant

         client = MeshyClient.Initialize(accountName, tenant, publicKey);

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
Using our client we can register a user. Optionally, we can register an anonymous user and skip to logging in.

If you have yet to do any configuration through the admin portal, you will by default be required to supply security questions.

If you wish to use email or text message, we can go to Configuration under your tenant you initialized.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
         var registerUser = new RegisterUser(username,password);
         registerUser.EmailAddress = "test@test.com";
         registerUser.PhoneNumber = "+15551234567";

         var securityQuestions = new List<SecurityQuestion>();

         securityQuestions.Add(new SecurityQuestions() {
            Question = "What is the most magical place in the universe?",
            Answer = "MeshyDB!"
         });
         
         registerUser.SecurityQuestions = securityQuestions;
         
         var hash = client.RegisterUser(registerUser);
         
         // Or we just register an anonymous user and take the quick way around

         var anonymousUser = client.RegisterAnonymousUser();

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

Once the verification code has been recieved we will need to verify the user.

-----
Login
-----
We have a client, we have a user lets make a connection!

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
   
         var connection = await client.LoginWithPassword(username, password);
         
         // Or log in anonomously if we made an anonymous user
         connection = await client.LoginAnonymouslyAsync(anonymousUser.Username);
         
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

         connection = MeshyClient.CurrentConnection;

-----------
Create data
-----------
We can use our newly authenticated user to make requests with MeshyDB and create some data.

The data object can whatever information you would like to capture. The following example will have some data fields with example data.

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#
         
         // Mesh Name can be overriden by attribute, otherwise by default it is derived from class name
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
If we need to make a modificaiton let's update our Mesh!

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