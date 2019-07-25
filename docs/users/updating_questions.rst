.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
------------------
Updating Questions
------------------
Update security questions about the logged in user. This is only available if Question verification has been configured.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         POST https://api.meshydb.com/{accountName}/users/me/questions HTTP/1.1
         Authentication: Bearer {access_token}
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
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var questions = new UserSecurityQuestionUpdate();

         questions.SecurityQuestions.Add(new SecurityQuestion(){
                                                                    Question = "What should this be?",
                                                                    Answer = "This seems like an ok example"
                                                               };

         await connection.Users.UpdateSecurityQuestions(id, user);

      |parameters|
      
      accountName  : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      securityQuestions : :type:`object[]`, :required:`required`
         Collection of questions and answers used for password recovery if question security is configured.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var meshyConnection = await client.login(username, password);
               
         await meshyConnection.usersService.updateSecurityQuestion({
                                                                     securityQuestions: securityQuestions
                                                                  }); 
      
      |parameters|

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
