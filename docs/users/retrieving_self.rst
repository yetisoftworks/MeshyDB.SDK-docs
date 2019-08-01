.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------------
Retrieving Self
---------------
Retrieve details about the authenticated user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         GET https://api.meshydb.com/{accountName}/users/me HTTP/1.1
         Authentication: Bearer {access_token}
         
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         await connection.Users.GetLoggedInUserAsync();

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

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
         Unique identifier for user or device.

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
    "roles": [
                "admin",
                "test"
             ],
    "securityQuestions": [
                            {
                               "question": "What would you say to this question?",
                               "answer": "..."
                            }
                         ],
    "anonymous": true
  }

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.