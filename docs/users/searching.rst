.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------
Searching
---------

Filter User data based on query parameters.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         await connection.Users.SearchAsync(name, roleId, orderBy, activeOnly, page, pageSize);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      name : :type:`string`
         Case-insensitive partial name search.
      roleId : :type:`string`
         Filters users with defined role identifier.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format.
      activeOnly : :type:`boolean`, default: true
         Filters users that are not active. Setting to false will include all users regardless of active status.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         await meshyConnection.usersService.search({
                                                    name: name,
                                                    roleId: roleId,
                                                    orderBy: orderBy,
                                                    activeOnly: activeOnly,
                                                    page: page,
                                                    pageSize: pageSize
                                                  });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      name : :type:`string`
         Case-insensitive partial name search.
      roleId : :type:`string`
         Filters users with defined role identifier.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format.
      activeOnly : :type:`boolean`, default: true
         Filters users that are not active. Setting to false will include all users regardless of active status.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: REST
   
      .. code-block:: http
      
         GET https://api.meshydb.com/{accountName}/users?name={name}&
                                                         roleId={roleId}&
                                                         orderBy={orderBy}&
                                                         activeOnly={activeOnly}&
                                                         page={page}&
                                                         pageSize={pageSize} HTTP/1.1
         Authorization: Bearer {access_token}

        (Line breaks added for readability)

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      name : :type:`string`
         Case-insensitive partial name search.
      roleId : :type:`string`
         Filters users with defined role identifier.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format.
      activeOnly : :type:`boolean`, default: true
         Filters users that are not active. Setting to false will include all users regardless of active status.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

.. rubric:: Responses

200 : OK
    * Identifies if users were found.

Example Result

.. code-block:: json

    {
        "results":[
            {
                "username":"test",
                "firstName":null,
                "lastName":null,
                "verified":true,
                "isActive":true,
                "phoneNumber":null,
                "emailAddress":null,
                "roles":[
                    {
                    "name":"meshy.user",
                    "addedDate":"2019-10-18T15:11:55.2413015-05:00"
                    }
                ],
                "securityQuestions":[
                    {
                    "question":"Test 1",
                    "answerHash":"..."
                    }
                ],
                "anonymous":false,
                "lastAccessed":null,
                "id":"5d4..."
            }
        ],
        "page":1,
        "pageSize":25,
        "totalRecords":1
    }

400 : Bad request
    * User is not able to delete self.
    
401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to read users.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.