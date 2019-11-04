.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------
Searching
---------

Search for information related to roles.

''''
Role
''''

Search details about roles.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.SearchAsync(name, page, pageSize);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      name : :type:`string`
         Case-insensitive partial name search.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.search({
                                                    name: name,
                                                    page: page,
                                                    pageSize: pageSize
                                                 });

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      name : :type:`string`
         Case-insensitive partial name search.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: REST
   
      .. code-block:: http
         
        GET https://api.meshydb.com/{accountName}/roles?name={name}&
                                                        page={page}&
                                                        pageSize={pageSize} HTTP/1.1
        Authentication: Bearer {access_token}

        (Line breaks added for readability)
      
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      name : :type:`string`
         Case-insensitive partial name search.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

.. rubric:: Responses

200 : OK
    * Identifies if role were found.

Example Result

.. code-block:: json

   {
      "page": 1,
      "pageSize": 25,
      "results": [{
                    "name":"test",
                    "description":"...",
                    "id":"5db...",
                    "numberOfUsers": 0
                 }],
      "totalRecords": 1
   }

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to read roles.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''
Permission
''''''''''

Search permissions from role by id.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.SearchPermissionAsync(roleId, permissibleName, page, pageSize);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissibleName : :type:`string`
         Case-insensitive partial name search of permissible.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.searchPermission(roleId, {
                                                                        permissibleName: permissibleName,
                                                                        page: page,
                                                                        pageSize: pageSize
                                                                    });

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissibleName : :type:`string`
         Case-insensitive partial name search of permissible.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: REST
   
      .. code-block:: http
         
        GET https://api.meshydb.com/{accountName}/roles/{roleId}/permissions?permissibleName={permissibleName}&
                                                                             page={page}&
                                                                             pageSize={pageSize} HTTP/1.1
        Authentication: Bearer {access_token}

        (Line breaks added for readability)

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissibleName : :type:`string`
         Case-insensitive partial name search of permissible.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

.. rubric:: Responses

200 : OK
    * Identifies if permissions were found.

Example Result

.. code-block:: json

{
   "results":[
      {
         "permissibleName":"meshes",
         "create":true,
         "update":true,
         "read":true,
         "delete":true,
         "id":"5d9..."
      }
   ],
   "page":1,
   "pageSize":25,
   "totalRecords":1
}

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to read roles.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''''
Permissibles
''''''''''''

Search for permissible to assign to a permission.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.SearchPermissibleAsync(name, page, pageSize);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      name : :type:`string`
         Case-insensitive partial name search.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.searchPermissible(name, page, pageSize);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      name : :type:`string`
         Case-insensitive partial name search.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: REST
   
      .. code-block:: http
         
        GET https://api.meshydb.com/{accountName}/permissibles?name={name}&
                                                               page={page}&
                                                               pageSize={pageSize} HTTP/1.1
        Authentication: Bearer {access_token}

        (Line breaks added for readability)

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      name : :type:`string`
         Case-insensitive partial name search.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

.. rubric:: Responses

200 : OK
    * Identifies if permissibles were found.

Example Result

.. code-block:: json

    {
        "results":[
            {
                "name":"meshes",
                "canCreate":true,
                "canUpdate":true,
                "canRead":true,
                "canDelete":true
            }
        ],
        "page":1,
        "pageSize":25,
        "totalRecords":1
    }

401 : Unauthorized
   * User is not authorized to make call.
   
403 : Forbidden
    * User has insufficent permission to read roles.

404 : Not Found
    * Role was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.