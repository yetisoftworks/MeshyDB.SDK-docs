.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

-------------
Checking hash
-------------
Verifies user verification hash request.


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
