.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
----------------
Generating Token
----------------

Create a short lived access token to be used for authorized API calls. Typically a token will last 3600 seconds(one hour).

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://auth.meshydb.com/{accountName}/connect/token HTTP/1.1
         Content-Type: application/x-www-form-urlencoded
         tenant: {tenant}

            client_id={publicKey}&
            grant_type=password&
            username={username}&
            password={password}&
            scope=meshy.api offline_access

        
      (Form-encoding removed, and line breaks added for readability)

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);
         var connection = client.LoginWithPassword(username, password);

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var meshyConnection = await client.login(username,password);
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
   
Example Response:

.. code-block:: json

   {
      "access_token": "ey...",
      "expires_in": 3600,
      "token_type": "Bearer",
      "refresh_token": "ab23cd3343e9328g"
   }
