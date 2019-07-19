.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
------------------
Resetting Password
------------------
Uses result from Forgot password to allow a user to reset their password.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/resetpassword  HTTP/1.1
         Content-Type: application/json
         tenant: {tenant}
         
           {
             "username": "username_testermctesterson",
             "expires": "1-1-2019",
             "hash": "randomlygeneratedhash",
             "newPassword": "newPassword"
           }

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      username : :type:`string`, :required:`required`
        Unique identifier for user or device.
      expires : :type:`date`, :required:`required`
        Defines when hash will expire before it needs to be regenerated.
      hash : :type:`string`, :required:`required`
        Hash result of forgot password to verify request for password reset.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.
        
   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);

         await client.ResetPasswordAsync(resetHash, newPassword);

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
        User name that is being reset.
      expires : :type:`date`, :required:`required`
        Defines when hash will expire before it needs to be regenerated.
      hash : :type:`string`, :required:`required`
        Hash result of forgot password to verify request for password reset.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var passwordResetHash = await client.forgotPassword(username);

         await client.resetPassword(passwordResetHash, newPassword)
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
        User name that is being reset.
      expires : :type:`date`, :required:`required`
        Defines when hash will expire before it needs to be regenerated.
      hash : :type:`string`, :required:`required`
        Hash result of forgot password to verify request for password reset.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.
