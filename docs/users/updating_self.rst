.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------
Updating Self
-------------

Updating self allows the ability to update the currently logged in user's information.

This might be personal or security questions for `password recovery <./forgetting_password.html>`_ at a later time.

^^^^^^^^^^^^^^^^^^^^
Personal Information
^^^^^^^^^^^^^^^^^^^^

The following can be used to update a logged in user's personal information such as name, phone number, and email address.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         PUT https://api.meshydb.com/{accountName}/users/me HTTP/1.1
         Authentication: Bearer {access_token}
         Content-Type: application/json
         tenant: {tenant}
         
           {
             "firstName": "Tester",
             "lastName": "McTesterton",
             "phoneNumber": "+15555555555",
             "emailAddress": "test@test.com"
           }

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      firstName : :type:`string`
         Provides details about a user's first name.
      lastName : :type:`string`
         Provides details about a user's last name.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Provides details about a user's phone number.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Provides details about a user's email address.

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
      firstName : :type:`string`
         Provides details about a user's first name.
      lastName : :type:`string`
         Provides details about a user's last name.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Provides details about a user's phone number.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Provides details about a user's email address.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var self = await meshyConnection.usersService.updateSelf({
                                                               firstName: firstName,
                                                               lastName: lastName,
                                                               phoneNumber: phoneNumber,
                                                               emailAddress: emailAddress
                                                            });
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      firstName : :type:`string`
         Provides details about a user's first name.
      lastName : :type:`string`
         Provides details about a user's last name.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Provides details about a user's phone number.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Provides details about a user's email address.
         
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
                               "answer": "..."
                            }
                         ],
    "anonymous": false
  }

^^^^^^^^^^^^^^^^^^
Security Questions
^^^^^^^^^^^^^^^^^^

The following can be used to change the currently logged in user's security questions to be used for `password recovery <./forgetting_password.html>`_.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/me/questions HTTP/1.1
         Authentication: Bearer {access_token}
         Content-Type: application/json
         tenant: {tenant}
         
           {
             "securityQuestions": [
                                    {
                                        "question": "What would you say to this question?",
                                        "answer": "..."
                                    }
                                  ]
           }

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var questions = new UserSecurityQuestionUpdate();

         questions.SecurityQuestions.Add(new SecurityQuestion(){
                                                                    Question = "What should this be?",
                                                                    Answer = "This seems like an ok example"
                                                               };

         await connection.Users.UpdateSecurityQuestions(id, user);

      |parameters|
      
      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var meshyConnection = await client.login(username, password);
               
         await meshyConnection.usersService.updateSecurityQuestion({
                                                                     securityQuestions: securityQuestions
                                                                  }); 
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.
         
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
