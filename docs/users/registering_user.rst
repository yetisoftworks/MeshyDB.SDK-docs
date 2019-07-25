.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

----------------
Registering User
----------------
Creates a new user that can log into the system.


.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/register HTTP/1.1
        Content-Type: application/json
         
          {
            "username": "username_testermctesterson",
            "firstName": "Tester",
            "lastName": "McTesterton",
            "verified": true,
            "isActive": true,
            "phoneNumber": "+15555555555",
            "emailAddress": "test@test.com",
            "roles": [
                        "admin",
                        "test"
                     ],
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
         Provides details about a user's first name.
      lastName : :type:`string`
         Provides details about a user's last name.
      verified : :type:`boolean`
         Identifies whether the user is verified.
      isActive : :type:`boolean`
         Identifies whether the user is active.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Provides details about a user's phone number.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Provides details about a user's email address.
      roles : :type:`string[]`
         Provides collection of roles to define permissions set of a user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         Collection of questions and answers used for password recovery if question security is configured.

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
         Provides details about a user's first name.
      lastName : :type:`string`
         Provides details about a user's last name.
      verified : :type:`boolean`
         Identifies whether the user is verified.
      isActive : :type:`boolean`
         Identifies whether the user is active.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Provides details about a user's phone number.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Provides details about a user's email address.
      roles : :type:`string[]`
         Provides collection of roles to define permissions set of a user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         Collection of questions and answers used for password recovery if question security is configured.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var user = await client.registerUser({
                                                username: username,
                                                newPassword: newPassword,
                                                firstName: firstName,
                                                lastName: lastName,
                                                verified: verified,
                                                isActive: isActive,
                                                phoneNumber: phoneNumber,
                                                emailAddress: emailAddress,
                                                roles: roles,
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
         Provides details about a user's first name.
      lastName : :type:`string`
         Provides details about a user's last name.
      verified : :type:`boolean`
         Identifies whether the user is verified.
      isActive : :type:`boolean`
         Identifies whether the user is active.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Provides details about a user's phone number.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Provides details about a user's email address.
      roles : :type:`string[]`
         Provides collection of roles to define permissions set of a user.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         Collection of questions and answers used for password recovery if question security is configured.
         
Example Response:

.. code-block:: json

   {
      "username": "username_testermctesterson",
      "attempt": 1,
      "hash": "...",
      "expires": "1/1/1900",
      "hint": "..."
   }
