.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

--------------
Creating Users
--------------

A user can be authenticated with the system for ensuring they are authorized to interact with the system.

You can either generate an anonymous user, or device user with limited functionality. Otherwise you can register a new user with full credentials.

''''''''''''''''''''''''''
Registering Anonymous User
''''''''''''''''''''''''''

An anonymous user can identify a device or unique user without requiring user interaction.

This kind of user has limited functionality such as not having the ability to be verified or be assigned roles.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/register/anonymous HTTP/1.1
        Content-Type: application/json
         
          {
            "username": "username_testermctesterson"
          }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);

        var anonymousUser = await client.RegisterAnonymousUserAsync(userName);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser(username);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
         

Responses
*********

201 : Created
   * New user has been registered and is now available for use.

   Example Result

   .. code-block:: json

   {
      "id": "5c78cc81dd870827a8e7b6c4",
      "username": "username_testermctesterson",
      "firstName": null,
      "lastName": null,
      "verified": false,
      "isActive": true,
      "phoneNumber": null,
      "emailAddress": null,
      "roles": [],
      "securityQuestions": [],
      "anonymous": true
   }

400 : Bad request
   * Username must be unique.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.

''''''''''''''''
Registering User
''''''''''''''''

Registering a user allows user defined credentials to access the system.

If email or text verification is configured, they will be prompted to verify their account.

The user will not be able to be authenticated until verification has been completed. The verification request lasts one hour before it expires.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/register HTTP/1.1
        Content-Type: application/json
         
          {
            "username": "username_testermctesterson",
            "firstName": "Tester",
            "lastName": "McTesterton",
            "phoneNumber": "+15555555555",
            "emailAddress": "test@test.com",
            "securityQuestions": [
                                    {
                                       "question": "What would you say to this question?",
                                       "answer": "mceasy123"
                                    }
                                 ],
            "newPassword": "newPassword"
          }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      newPassword : :type:`string`, :required:`required`
         New user secret credentials for login.
      firstName : :type:`string`
         First name of registering user.
      lastName : :type:`string`
         Last name of registering user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of registering user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of registering user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         New set of questions and answers for registering user in password recovery.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);

        var user = new RegisterUser();

        await client.RegisterUserAsync(user);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      newPassword : :type:`string`, :required:`required`
         New user secret credentials for login.
      firstName : :type:`string`
         First name of registering user.
      lastName : :type:`string`
         Last name of registering user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of registering user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of registering user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         New set of questions and answers for registering user in password recovery.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var user = await client.registerUser({
                                                username: username,
                                                newPassword: newPassword,
                                                firstName: firstName,
                                                lastName: lastName,
                                                phoneNumber: phoneNumber,
                                                emailAddress: emailAddress,
                                                securityQuestions: securityQuestions
                                             });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      newPassword : :type:`string`, :required:`required`
         New user secret credentials for login.
      firstName : :type:`string`
         First name of registering user.
      lastName : :type:`string`
         Last name of registering user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of registering user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of registering user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         New set of questions and answers for registering user in password recovery.
         
Response
~~~~~~~~

.. code-block:: json

   {
      "username": "username_testermctesterson",
      "attempt": 1,
      "hash": "...",
      "expires": "1/1/1900",
      "hint": "..."
   }

''''''''''
Check Hash
''''''''''

Optionally, before verifying the request you can choose to check if the verification code provided is valid.

You may want to provide this flow if you still need to collect more information about the user before finalizing verification.

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

''''''
Verify
''''''

If email or text verification is configured the registered user must be verified. The resulting request lasts one hour.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/verify HTTP/1.1
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
		
        await client.VerifyAsync(check);

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
         
         await client.verify({
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