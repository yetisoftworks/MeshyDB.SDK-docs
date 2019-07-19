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
        tenant: {tenant}
         
          {
            "id": "5c78cc81dd870827a8e7b6c4",
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
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      username : :type:`string`, :required:`required`
         Username of user.
      newPassword : :type:`string`, :required:`required`
         Password of user to use for login.
      id : :type:`string`
         Identifier of user.
      firstName : :type:`string`
         First name of user.
      lastName : :type:`string`
         Last name of user.
      verified : :type:`boolean`
         Identifies whether the user is verified.
      isActive : :type:`boolean`
         Identifies whether or not the user is active.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of the user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of the user.
      roles : :type:`string[]`
         Collection of roles user has access.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);

        var user = new RegisterUser();

        await client.RegisterUserAsync(user);

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Username of user.
      newPassword : :type:`string`, :required:`required`
         Password of user to use for login.
      id : :type:`string`
         Identifier of user.
      firstName : :type:`string`
         First name of user.
      lastName : :type:`string`
         Last name of user.
      verified : :type:`boolean`
         Identifies whether the user is verified.
      isActive : :type:`boolean`
         Identifies whether or not the user is active.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of the user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of the user.
      roles : :type:`string[]`
         Collection of roles user has access.
      securityQuestions : :type:`object[]`, :required:`required` *if using question verification*
         Collection of questions and answers used for password recovery if question security is configured.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var user = await client.registerUser({
                                                username: username,
                                                newPassword: newPassword,
                                                id: id,
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

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Username of user.
      newPassword : :type:`string`, :required:`required`
         Password of user to use for login.
      id : :type:`string`
         Identifier of user.
      firstName : :type:`string`
         First name of user.
      lastName : :type:`string`
         Last name of user.
      verified : :type:`boolean`
         Identifies whether the user is verified.
      isActive : :type:`boolean`
         Identifies whether or not the user is active.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of the user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of the user.
      roles : :type:`string[]`
         Collection of roles user has access.
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
