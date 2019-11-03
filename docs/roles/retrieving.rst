.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
----------
Retrieving
----------

Retrieve information for specific role.

''''
Role
''''

Retrieve details about role by id.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.GetAsync(roleId);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
		
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.get(roleId);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.

   .. group-tab:: REST
   
      .. code-block:: http
         
        GET https://api.meshydb.com/{accountName}/roles/{roleId} HTTP/1.1
        Authentication: Bearer {access_token}

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.

.. rubric:: Responses

200 : OK
    * Identifies if role was found.

Example Result

.. code-block:: json

    {
        "name":"test",
        "description":"...",
        "id":"5db..."
    }

403 : Forbidden
    * User has insufficent permission to read roles.

404 : Not Found
    * Role was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''
Permission
''''''''''

Get specific permission from role by id.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        var permission = await connection.Roles.GetPermissionAsync(roleId, permissionId);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissionId : :type:`string`, :required:`required`
         Identifies id of permission.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        var permission = await meshyConnection.rolesService.getPermission(roleId, permissionId);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissionId : :type:`string`, :required:`required`
         Identifies id of permission.

   .. group-tab:: REST
   
      .. code-block:: http
         
        GET https://api.meshydb.com/{accountName}/roles/{roleId}/permissions/{permissionId} HTTP/1.1
        Authentication: Bearer {access_token}

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissionId : :type:`string`, :required:`required`
         Identifies id of permission.

.. rubric:: Responses

200 : OK
    * Identifies if permission was found.

Example Result

.. code-block:: json

    {
        "id":"5db...",
        "permissibleName":"meshes",
        "create":"true",
        "update":"true",
        "read":"true",
        "delete":"true"
    }

403 : Forbidden
    * User has insufficent permission to read roles.

404 : Not Found
    * Permission was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.