.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

-----------------
Password Recovery
-----------------

When a previously authenticated user used the system, they may need to recover their password. The request will be valid for one hour.

They will first need to request a forgot password before they are able to reset it.

Depending on your verification flow, whether it be email, text or security questions the user will need to either provide a code or answer to question to prove their knowledge of the request.

'''''''''''''''''''
Forgetting Password
'''''''''''''''''''

Creates a request for password reset that must have the matching data to reset to ensure request parity.

If using security questions, you can provide an attempt number to select which question is used for verification.

The attempt will load the question into the hint field to be asked of the user.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);

         await client.ForgotPasswordAsync(username, attempt);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies how many times a request has been made.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         await client.forgotPassword(username, attempt);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies how many times a request has been made.

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
         Indicates which account you are connecting to.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies how many times a request has been made.

.. rubric:: Responses

200 : OK
   * Generates forgot password response to be used for password reset.

Example Result

.. code-block:: json

	{
		"username": "username_testermctesterson",
		"attempt": 1,
		"hash": "...",
		"expires": "1900-01-01T00:00:00.000Z",
		"hint": "..."
	}

400 : Bad request
   * Username is required.
   * Anonymous user cannot recover password.

404 : Not Found
   * User was not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

''''''''''
Check Hash
''''''''''

Optionally, before the user's password is reset you can check if the verification code, they provide is valid.

This would allow a user to verify their code before requiring a reset.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);

        var check = new UserVerificationCheck();
		
        var isValid = await client.CheckHashAsync(check);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         await client.checkHash({
                                    username: username,
                                    attempt: attempt,
                                    hash: hash,
                                    expires: expires,
                                    verificationCode: verificationCode
                               });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/checkhash HTTP/1.1
        Content-Type: application/json
         
          {
             "username": "username_testermctesterson",
             "attempt": 1,
             "hash": "...",
             "expires": "1900-01-01T00:00:00.000Z",
             "verificationCode": "...",
          }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
		
.. rubric:: Responses

200 : OK
   * Identifies if hash with verification code is valid.

Example Result

.. code-block:: boolean

	true

400 : Bad request
   * Username is required.
   * Hash is required.
   * Expires is required.
   * Verification code is required.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

''''''''''''''''''
Resetting Password
''''''''''''''''''

Take result from forgot password and application verification code generated from email/text or security question answer, along with a new password to be used for login.


.. tabs::
        
   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);

         var forgotPassword = await client.ForgotPasswordAsync(username, attempt);

         var resetPassword = new ResetPassword() {
                                                   Username: forgotPassword.Username,
                                                   Attempt: forgotPassword.Attempt,
                                                   Hash: forgotPassword.Hash,
                                                   Expires: forgotPassword.Expires,
                                                   VerificationCode: verificationCode,
                                                   NewPassword: newPassword
                                                 };

         await client.ResetPasswordAsync(resetPassword);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var passwordResetHash = await client.forgotPassword(username);

         await client.resetPassword({
                                       username: passwordResetHash.username,
                                       attempt: passwordResetHash.attempt,
                                       hash: passwordResetHash.hash,
                                       expires: passwordResetHash.expires,
                                       verificationCode: verificationCode,
                                       newPassword: newPassword
                                   });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.

   .. group-tab:: REST
   
      .. code-block:: http

         POST https://api.meshydb.com/{accountName}/users/resetpassword HTTP/1.1
         Content-Type: application/json
         
            {
               username: "username_testermctesterson",
               attempt: 1,
               hash: "...",
               expires: "1900-01-01T00:00:00.000Z",
               verificationCode: "...",
               newPassword: "..."
            }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      attempt : :type:`integer`, default: 1
         Identifies which attempt hash was generated against.
      hash : :type:`string`, :required:`required`
         Generated hash from verification request.
      expires : :type:`date`, :required:`required`
         Identifies when the request expires.
      hint : :type:`string`
         Hint for verification code was generated.
      verificationCode : :type:`string`, :required:`required`
         Value to verify against verification request.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.

.. rubric:: Responses

204 : No Content
   * Identifies password reset is successful.

400 : Bad request
   * Username is required.
   * Hash is required.
   * Expires is required.
   * Verification code is required.
   * Hash is expired.
   * New password is required.
   * Anonymous user cannot be reset.
   * User has already been verified.
   * Request hash is invalid.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.