.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------
Updating Self
-------------
Update details about the logged in user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         PUT https://api.meshydb.com/{accountName}/users/me HTTP/1.1
         Authentication: Bearer {access_token}
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
             "emailAddress": "test@test.com"
             "roles": [
                         "admin",
                         "test"
                      ],
             "securityQuestions": [
                                    {
                                        "question": "What would you say to this question?",
                                        "answer": "..."
                                    }
                                  ],
             "anonymous": false
           }

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      id : :type:`string`
         Identifies unique record of user data.
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
      anonymous : :type:`boolean`
         Identifies whether the user is anonymous.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var user = new User();

         await connection.Users.UpdateUserAsync(id, user);

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      id : :type:`string`
         Identifies unique record of user data.
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
      anonymous : :type:`boolean`
         Identifies whether the user is anonymous.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var self = await meshyConnection.usersService.updateSelf({
                                                               username: username,
                                                               id: id,
                                                               firstName: firstName,
                                                               lastName: lastName,
                                                               verified:  verified,
                                                               isActive: isActive,
                                                               phoneNumber: phoneNumber,
                                                               emailAddress: emailAddress,
                                                               roles: roles,
                                                               securityQuestions: securityQuestions,
                                                               anonymous:  anonymous
                                                            });
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      id : :type:`string`
         Identifies unique record of user data.
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
      anonymous : :type:`boolean`
         Identifies whether the user is anonymous.
         
Example Response:

.. code-block:: json

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
    "anonymous": false
  }
