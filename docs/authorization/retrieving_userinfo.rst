.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------------------
Retrieving User Info
--------------------
Retrieve user information.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        GET https://auth.meshydb.com/{accountName}/connect/userinfo HTTP/1.1
        Authentication: Bearer {access_token}
         
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        var userInfo = await connection.GetMyUserInfoAsync();

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);

         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);
         
         var info = await meshyConnection.getMyUserInfo();
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
		 
Example Response:

.. code-block:: json

   {
       "sub": "5c990a772a8fc94ec4b3dc20",
       "name": "",
       "given_name": "",
       "family_name": "",
       "id": "login@email.com",
       "rate_limit": "10",
       "role": "admin"
   }
