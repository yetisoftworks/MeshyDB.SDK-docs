.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------------
Forgetting Password
-------------------
Creates a request for password reset that must have the matching data to reset to ensure request parity.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/forgotpassword HTTP/1.1
         Content-Type: application/json
         tenant: {tenant}
         
           {
             "username": "username_testermctesterson",
             "attempt": 1
           }

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      username : :type:`string`, :required:`required`
         User name to be reset.
      attempt : :type:`integer`, :required:`required`
         Identifies which number of times of request.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = new MeshyClient(accountName, tenant, publicKey);

         await client.ForgotPasswordAsync(username, attempt);

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         User name to be reset.
      attempt : :type:`integer`, :required:`required`
         Identifies which number of times of request.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = initializeMeshyClientWithTenant(accountName, tenant, publicKey);
         
         client.forgotPassword(username, attempt)
               .then(function(passwordResetHash) { });
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         User name to be reset.
      attempt : :type:`integer`, :required:`required`
         Identifies which number of times of request.

         
Example Response:

.. code-block:: json

	{
		"username": "username_testermctesterson",
		"attempt": 1:
		"hash": "...",
		"expires": "1900-01-01T00:00:00.000Z",
		"hint": "xxxx"
	}