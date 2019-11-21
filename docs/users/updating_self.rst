.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Updating
--------

Updating self allows the ability to update the authenticated user's information.

This might be personal or security questions for password recovery later.

''''''''''''''''''''''
My Personal Information
''''''''''''''''''''''

The following can be used to update an authenticated user's personal information such as name, phone number, and email address.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var user = new User();

         await connection.Users.UpdateSelfAsync(user);

      |parameters|
      
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      firstName : :type:`string`
         First name of authenticated user.
      lastName : :type:`string`
         Last name of authenticated user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of authenticated user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of authenticated user.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var self = await meshyConnection.usersService.updateSelf({
                                                               firstName: firstName,
                                                               lastName: lastName,
                                                               phoneNumber: phoneNumber,
                                                               emailAddress: emailAddress
                                                            });
      
      |parameters|

      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      firstName : :type:`string`
         First name of authenticated user.
      lastName : :type:`string`
         Last name of authenticated user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of authenticated user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of authenticated user.

   .. group-tab:: REST
   
      .. code-block:: http
      
         PUT https://api.meshydb.com/{accountName}/users/me HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
           {
             "firstName": "Tester",
             "lastName": "McTesterton",
             "phoneNumber": "+15555555555",
             "emailAddress": "test@test.com"
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      firstName : :type:`string`
         First name of authenticated user.
      lastName : :type:`string`
         Last name of authenticated user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of authenticated user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of authenticated user.

.. rubric:: Responses

200 : OK
   Updated information of updated authorized user.
   
Example Result

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
    "roles" : [
               {
                  "name":"admin",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               },
               {
                  "name":"test",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               }
            ],
    "securityQuestions": [
                            {
                               "question": "What would you say to this question?",
                               "answer": "..."
                            }
                         ],
    "anonymous": false,
    "lastAccessed":"2019-01-01T00:00:00.0000+00:00"
  }

400 : Bad request
   * Email address is required when Email recovery is enabled and the user is not anonymous.
   * Phone number is required when Text recovery is enabled and the user is not anonymous.
   * Username is a required field.
   * Email address must be in a valid format.
   * Phone number must be in an international format.
   * Unable to change user roles via API.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

'''''''''''''''''''''''''''''
Existing Personal Information
'''''''''''''''''''''''''''''

The following can be used to update an existing user's personal information such as name, phone number, and email address.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var user = new User();

         await connection.Users.UpdateAsync(id, user);

      |parameters|
      
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      id : :type:`string`, :required:`required`
         Identifies id of user.
      firstName : :type:`string`
         First name of authenticated user.
      lastName : :type:`string`
         Last name of authenticated user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of authenticated user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of authenticated user.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.loginAnonymously(username);

         var self = await meshyConnection.usersService.update(id,
                                                             {
                                                               firstName: firstName,
                                                               lastName: lastName,
                                                               phoneNumber: phoneNumber,
                                                               emailAddress: emailAddress
                                                             });
      
      |parameters|

      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      id : :type:`string`, :required:`required`
         Identifies id of user.
      firstName : :type:`string`
         First name of authenticated user.
      lastName : :type:`string`
         Last name of authenticated user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of authenticated user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of authenticated user.

   .. group-tab:: REST
   
      .. code-block:: http
      
         PUT https://api.meshydb.com/{accountName}/users/{id} HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
           {
             "firstName": "Tester",
             "lastName": "McTesterton",
             "phoneNumber": "+15555555555",
             "emailAddress": "test@test.com"
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      id : :type:`string`, :required:`required`
         Identifies id of user.
      firstName : :type:`string`
         First name of authenticated user.
      lastName : :type:`string`
         Last name of authenticated user.
      phoneNumber : :type:`string`, :required:`required` *if using phone verification*
         Phone number of authenticated user.
      emailAddress : :type:`string`, :required:`required` *if using email verification*
         Email address of authenticated user.

.. rubric:: Responses

200 : OK
   Updated information of updated existing user.
   
Example Result

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
    "roles" : [
               {
                  "name":"admin",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               },
               {
                  "name":"test",
                  "addedDate":"2019-01-01T00:00:00.0000000+00:00"
               }
            ],
    "securityQuestions": [
                            {
                               "question": "What would you say to this question?",
                               "answer": "..."
                            }
                         ],
    "anonymous": false,
    "lastAccessed":"2019-01-01T00:00:00.0000+00:00"
  }

400 : Bad request
   * Email address is required when Email recovery is enabled and the user is not anonymous.
   * Phone number is required when Text recovery is enabled and the user is not anonymous.
   * Username is a required field.
   * Email address must be in a valid format.
   * Phone number must be in an international format.
   * Unable to change user roles via API.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to update users.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

'''''''''''''''''''''
My Security Questions
'''''''''''''''''''''

The following can be used to change the authenticated user's security questions to be used for password recovery.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var questions = new UserSecurityQuestionUpdate();

         questions.SecurityQuestions.Add(new SecurityQuestion(){
                                                                    Question = "What should this be?",
                                                                    Answer = "This seems like an ok example"
                                                               };

         await connection.Users.UpdateSecurityQuestionsAsync(questions);

      |parameters|
      
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      securityQuestions : :type:`object[]`, :required:`required`
         New set of questions and answers for authenticated user in password recovery.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.login(username, password);
               
         await meshyConnection.usersService.updateSecurityQuestion({
                                                                     securityQuestions: securityQuestions
                                                                  }); 
      
      |parameters|

      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/me/questions HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
           {
             "securityQuestions": [
                                    {
                                        "question": "What would you say to this question?",
                                        "answer": "..."
                                    }
                                  ]
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      securityQuestions : :type:`object[]`, :required:`required`
         New set of questions and answers for authenticated user in password recovery.

.. rubric:: Responses

204 : No Content
   * Updated information of updated authorized user.

400 : Bad request
   * Unable to update security questions if question verification is not configured.
   * Anonymous user cannot have security questions.
   * At least one question is required.
   * Question text is required.
   * Answer is required.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

'''''''''''''''''''''''''''
Existing Security Questions
'''''''''''''''''''''''''''

The following can be used to change the authenticated user's security questions to be used for password recovery.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var questions = new UserSecurityQuestionUpdate();

         questions.SecurityQuestions.Add(new SecurityQuestion(){
                                                                    Question = "What should this be?",
                                                                    Answer = "This seems like an ok example"
                                                               };

         await connection.Users.UpdateSecurityQuestionsAsync(id, questions);

      |parameters|
      
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      id : :type:`string`, :required:`required`
         Identifies id of user.
      securityQuestions : :type:`object[]`, :required:`required`
         New set of questions and answers for authenticated user in password recovery.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.login(username, password);
               
         await meshyConnection.usersService.updateUserSecurityQuestion(id, 
                                                                      {
                                                                        securityQuestions: securityQuestions
                                                                      });

      |parameters|

      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      id : :type:`string`, :required:`required`
         Identifies id of user.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/{id}/questions HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
           {
             "securityQuestions": [
                                    {
                                        "question": "What would you say to this question?",
                                        "answer": "..."
                                    }
                                  ]
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      id : :type:`string`, :required:`required`
         Identifies id of user.
      securityQuestions : :type:`object[]`, :required:`required`
         New set of questions and answers for authenticated user in password recovery.

.. rubric:: Responses

204 : No Content
   * Updated information of updated existing user.

400 : Bad request
   * Unable to update security questions if question verification is not configured.
   * Anonymous user cannot have security questions.
   * At least one question is required.
   * Question text is required.
   * Answer is required.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to update users.
   
429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

'''''''''''''''''
Changing Password
'''''''''''''''''

Allows the authenticated user to change their password.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginWithPasswordAsync(username, password);

         await connection.UpdatePasswordAsync(previousPassword, newPassword);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
      previousPassword : :type:`string`, :required:`required`
        Previous user secret credentials for login.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.login(username, password);

         await meshyConnection.updatePassword(previousPassword, newPassword);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
      previousPassword : :type:`string`, :required:`required`
        Previous user secret credentials for login.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/me/password HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
           {
             "newPassword": "newPassword",
             "previousPassword": "previousPassword"
           }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token: :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generate Access Token <auth.html#generate-access-token>`_.
      previousPassword : :type:`string`, :required:`required`
        Previous user secret credentials for login.
      newPassword : :type:`string`, :required:`required`
        New user secret credentials for login.

.. rubric:: Responses

204 : No Content
   * Identifies password was updated successfully.

400 : Bad request
   * Anonymous user cannot change password.
   * New password is required.
   * Previous password is required.
   * Previous password does not match existing password.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.