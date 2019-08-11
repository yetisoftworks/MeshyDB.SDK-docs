.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------
Logging Out
-------------
Log authenticated user out.

.. tabs::
         
   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.SignoutAsync();

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

         await meshyConnection.signout();
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

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
         Indicates which account you are connecting to.
      refresh_token  : :type:`string`, :required:`required`
         Refresh token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.

.. rubric:: Responses

200 : OK
   * Identifies successful logout.

400 : Bad request
   * Invalid client id.
   * Token is missing.
   * Unsupported Token type.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.