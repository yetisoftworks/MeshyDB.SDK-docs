.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------
Logging Out
-------------
Log user out.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/connect/revocation HTTP/1.1
        Content-Type: application/x-www-form-urlencoded
         
          token={refresh_token}&
          token_type_hint=refresh_token&
          client_id={publicKey}

        (Form-encoding removed, and line breaks added for readability)

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      refresh_token  : :type:`string`, :required:`required`
         Refresh token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
         
   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.SignoutAsync();

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

         await meshyConnection.signout();
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.