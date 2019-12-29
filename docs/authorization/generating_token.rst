.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
----------------
Generating Token
----------------

Create a short lived access token to be used for authorized API calls. Typically a token will last 3600 seconds(one hour).

.. tabs::
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = client.LoginWithPassword(username, password);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.login(username,password);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://auth.meshydb.com/{accountName}/connect/token HTTP/1.1
         Content-Type: application/x-www-form-urlencoded

            client_id={publicKey}&
            grant_type=password&
            username={username}&
            password={password}&
            scope=meshy.api offline_access

        
      (Form-encoding removed, and line breaks added for readability)

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.

.. rubric:: Responses

200 : OK
   * Generates new credentials for authorized user.

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
   * Invalid Scope.
   * Username is invalid.
   * Password is invalid.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.