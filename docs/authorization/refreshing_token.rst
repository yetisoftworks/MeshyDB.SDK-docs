.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
----------------
Refreshing Token
----------------

Using the token request made to generate an access token, a refresh token will also be generated. Once the token expires the refresh token can be used to generate a new set of credentials for authorized calls.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://auth.meshydb.com/{accountName}/connect/token HTTP/1.1
         Content-Type: application/x-www-form-urlencoded
         tenant: {tenant}

            client_id={publicKey}&
            grant_type=refresh_token&
            refresh_token={refresh_token}

        
      (Form-encoding removed, and line breaks added for readability)

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      refresh_token : :type:`string`, :required:`required`
         Refresh token generated from  previous access token generation.

   .. group-tab:: C#
   
      .. code-block:: c#

        var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);
        var connection = client.LoginWithPassword(username, password);
        var refreshToken = connection.RetrievePersistanceToken();
        
        connection = await client.LoginWithPersistanceAsync(refreshToken);

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         User name.
      password : :type:`string`, :required:`required`
         User password.
      refreshToken : :type:`string`, :required:`required`
         Refresh token generated from  previous access token generation.
         
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);

         client.login(username,password)
               .then(function (meshyConnection){
                        var refreshToken = meshyConnection.retrievePersistanceToken();

                        client.loginWithPersistance(refreshToken)
                              .then(function(refreshedMeshyConnection) { });
               });
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         User name.
      password : :type:`string`, :required:`required`
         User password.
      refreshToken : :type:`string`, :required:`required`
         Refresh token generated from  previous access token generation.
         
Example Response:

.. code-block:: json

   {
      "access_token": "ey...",
      "expires_in": 3600,
      "token_type": "Bearer",
      "refresh_token": "ab23cd3343e9328g"
   }
