.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-----------------
Changing Password
-----------------
Allows the authenticated user to change their password.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/me/password HTTP/1.1
         Authentication: Bearer {access_token}
         Content-Type: application/json
         
           {
             "newPassword": "newPassword",
             "previousPassword": "previousPassword"
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token: :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generate Access Token <auth.html#generate-access-token>`_.
      previousPassword : :type:`string`, :required:`required`
        Previous user secret credentials for login.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginWithPasswordAsync(username, password);

         await connection.UpdatePasswordAsync(previousPassword, newPassword);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
      previousPassword : :type:`string`, :required:`required`
        Previous user secret credentials for login.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.login(username, password);

         await meshyConnection.updatePassword(previousPassword, newPassword);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
      previousPassword : :type:`string`, :required:`required`
        Previous user secret credentials for login.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.
