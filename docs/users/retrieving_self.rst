.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
----------
Retrieving
----------
Retrieving information about a user.

''''
Self
''''

Retrieve details about the authenticated user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         GET https://api.meshydb.com/{accountName}/users/me HTTP/1.1
         Authorization: Bearer {access_token}
         
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var self = await meshyConnection.usersService.getSelf();
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         await connection.Users.GetSelfAsync();

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.

.. rubric:: Responses

200 : OK
   * Retrieves information about the authorized user.

Example Result

.. code-block:: json

  {
    "id": "5c78cc81dd870827a8e7b6c4",
    "username": "username_testermctesterson",
    "firstName": "Tester",
    "lastName": "McTesterton",
    "verified": true,
    "isActive": true,
    "phoneNumber": "5555555555",
    "roles" : [
               {
                  "name":"admin",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               },
               {
                  "name":"test",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               }
            ],
    "securityQuestions": [
                            {
                               "question": "What would you say to this question?",
                               "answer": "..."
                            }
                         ],
    "anonymous": true,
    "lastAccessed":"2019-01-01T00:00:00.0000+00:00"
  }

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

''''''''
Existing
''''''''

Retrieve details about an existing user by id.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         GET https://api.meshydb.com/{accountName}/users/{id} HTTP/1.1
         Authorization: Bearer {access_token}
         
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      id : :type:`string`, :required:`required`
         Identifies id of user.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var user = await meshyConnection.usersService.get(id);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      id : :type:`string`, :required:`required`
         Identifies id of user.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         await connection.Users.GetAsync(id);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      id : :type:`string`, :required:`required`
         Identifies id of user.

.. rubric:: Responses

200 : OK
   * Retrieves information about the existing user.

Example Result

.. code-block:: json

  {
    "id": "5c78cc81dd870827a8e7b6c4",
    "username": "username_testermctesterson",
    "firstName": "Tester",
    "lastName": "McTesterton",
    "verified": true,
    "isActive": true,
    "phoneNumber": "5555555555",
    "roles" : [
               {
                  "name":"admin",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               },
               {
                  "name":"test",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               }
            ],
    "securityQuestions": [
                            {
                               "question": "What would you say to this question?",
                               "answer": "..."
                            }
                         ],
    "anonymous": true,
    "lastAccessed":"2019-01-01T00:00:00.0000+00:00"
  }

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to read users.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.