.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
----------------
Refreshing Token
----------------

Using the token request made to generate an access token, a refresh token will also be generated. 

Once the token expires the refresh token can be used to generate a new set of credentials for authorized calls.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = client.LoginWithPassword(username, password);
        var refreshToken = connection.RetrieveRefreshToken();
        
        connection = await client.LoginWithRefreshAsync(refreshToken);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
      refreshToken : :type:`string`, :required:`required`
         Refresh token generated from  previous access token generation.
         
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);

         var meshyConnection = await client.login(username,password);

         var refreshToken = meshyConnection.retrieveRefreshToken();

         var refreshedMeshyConnection = await client.loginWithRefresh(refreshToken);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
      refreshToken : :type:`string`, :required:`required`
         Refresh token generated from  previous access token generation.

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://auth.meshydb.com/{accountName}/connect/token HTTP/1.1
         Content-Type: application/x-www-form-urlencoded

            client_id={publicKey}&
            grant_type=refresh_token&
            refresh_token={refresh_token}

        
      (Form-encoding removed, and line breaks added for readability)

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      refresh_token : :type:`string`, :required:`required`
         Refresh token generated from  previous access token generation.
         
.. rubric:: Responses

200 : OK
   * Generates new refresh credentials for authorized user.

Example Result

.. code-block:: json

   {
      "access_token": "ey...",
      "expires_in": 3600,
      "token_type": "Bearer",
      "refresh_token": "ab23cd3343e9328g"
   }

400 : Bad request
   * Token is invalid.
   * Client id is invalid.
   * Grant type is invalid.
   * User is no longer active.
   * Refresh token is expired.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.