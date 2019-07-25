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
         
           {
             "username": "username_testermctesterson",
             "attempt": 1
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      attempt : :type:`integer`, :required:`required`
         Identifies how many times a request has been made.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);

         await client.ForgotPasswordAsync(username, attempt);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      attempt : :type:`integer`, :required:`required`
         Identifies how many times a request has been made.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         await client.forgotPassword(username, attempt);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      attempt : :type:`integer`, :required:`required`
         Identifies how many times a request has been made.

         
Example Response:

.. code-block:: json

	{
		"username": "username_testermctesterson",
		"attempt": 1:
		"hash": "...",
		"expires": "1900-01-01T00:00:00.000Z",
		"hint": "xxxx"
	}