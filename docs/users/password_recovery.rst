.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

-----------------
Password Recovery
-----------------

When a previously authenticated user used the system they may need to recover their password. The request will be valid for one hour.

They will first need to request a forgot password before they are able to reset it.

Depending on your verification flow, whether it be email, text or security questions the user will need to either provide a code or answer to question to prove their knowledge of the request.

^^^^^^^^^^^^^^^^^^^^
Forgetting Password
^^^^^^^^^^^^^^^^^^^^

Creates a request for password reset that must have the matching data to reset to ensure request parity.

If using security questions, you can provide an attempt number to select which question is used for verification.

The attempt will load the question into the hint field to be asked of the user.

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

^^^^^^^^^^
Check Hash
^^^^^^^^^^

Optionally, before the user's password is reset you are able to check if the verification code they provide is valid.

This would allow a user to verify their code before requiring a reset.

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

^^^^^^^^^^^^^^^^^^
Resetting Password
^^^^^^^^^^^^^^^^^^

Take result from forgot password and application verificaiton code generated from email/text or security question answer, along with a new password to be used for login.


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