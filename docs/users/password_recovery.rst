.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

-----------------
Password Recovery
-----------------

^^^^^^^^^^^^^^^^^^^^
Forgetting Password
^^^^^^^^^^^^^^^^^^^^
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

^^^^^^^^^^^^^^^^^^
Resetting Password
^^^^^^^^^^^^^^^^^^
Uses result from Forgot password to allow a user to reset their password.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/resetpassword  HTTP/1.1
         Content-Type: application/json
         
           {
             "username": "username_testermctesterson",
             "expires": "1-1-2019",
             "hash": "randomlygeneratedhash",
             "newPassword": "newPassword"
           }

      |parameters|
      
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
      
         var client = MeshyClient.Initialize(accountName, publicKey);

         await client.ResetPasswordAsync(resetHash, newPassword);

      |parameters|
      
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
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var passwordResetHash = await client.forgotPassword(username);

         await client.resetPassword(passwordResetHash, newPassword)
      
      |parameters|

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

^^^^^^^^^^
Check Hash
^^^^^^^^^^

Performs a check against the generated hash object with the verification code to ensure correctness before resetting a password or verifying a user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/checkhash HTTP/1.1
        Content-Type: application/json
         
          {
             "username": "username_testermctesterson",
             "attempt": 1,
             "hash": "...",
             "expires": "1/1/1900",
             "hint": "...",
             "verificationCode": "...",
          }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      attempt : :type:`integer`, :required:`required`
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`, :required:`required`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);

        var check = new UserVerificationCheck();
		
        var isValid = await client.CheckHashAsync(check);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      attempt : :type:`integer`, :required:`required`
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`, :required:`required`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         await client.checkHash({
                                    username: username,
                                    attempt: attempt:
                                    hash: hash,
                                    expires: expires,
                                    hint: hint,
                                    verificationCode: verificationCode
                               });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      attempt : :type:`integer`, :required:`required`
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`, :required:`required`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
		
Example Response:

.. code-block:: boolean

	true
