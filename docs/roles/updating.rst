.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Updating
--------

Update information for specific role.

''''
Role
''''

Update details about role by id.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
         
        PUT https://api.meshydb.com/{accountName}/roles/{roleId} HTTP/1.1
        Authorization: Bearer {access_token}
        Content-Type: application/json

        {
            "name":"test",
            "description":"..."
        }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.update(roleId, role);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.UpdateAsync(roleId, role);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      name : :type:`string`, :required:`required`
         Name of the role.
      description : :type:`string`
         Describes the purpose of the role.
      numberOfUsers : :type:`string`
         Read-only count of users assigned to the role.

.. rubric:: Responses

200 : OK
    * Identifies if role was updated.

Example Result

.. code-block:: json

    {
        "name":"test",
        "description":"...",
        "id":"5db..."
    }

400 : Bad request
    * Name is required.
    * Name can only be alpha characters only.
    * Role cannot start with 'meshy.'.
    * Role already exists.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to update roles.

404 : Not Found
    * Role was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''
Permission
''''''''''

Update specific permission from role by id.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
         
        PUT https://api.meshydb.com/{accountName}/roles/{roleId}/permissions/{permissionId} HTTP/1.1
        Authorization: Bearer {access_token}
        Content-Type: application/json

        {
            "permissibleName":"meshes",
            "create":"true",
            "update":"true",
            "read":"true",
            "delete":"true"
        }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissionId : :type:`string`, :required:`required`
         Identifies id of permission.
      permissibleName : :type:`string`, :required:`required`
         Name of permissible reference. An example would be 'meshes' or 'meshes.{meshName}' to identify access to a specific mesh.
      create : type:`boolean`
         Identifies if role can create data.
      update : type:`boolean`
         Identifies if role can update data.
      read : type:`boolean`
         Identifies if role can read data.
      delete : type:`boolean`
         Identifies if role can delete data.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.deletePermission(roleId, permissionId);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissionId : :type:`string`, :required:`required`
         Identifies id of permission.
      permissibleName : :type:`string`, :required:`required`
         Name of permissible reference. An example would be 'meshes' or 'meshes.{meshName}' to identify access to a specific mesh.
      create : type:`boolean`
         Identifies if role can create data.
      update : type:`boolean`
         Identifies if role can update data.
      read : type:`boolean`
         Identifies if role can read data.
      delete : type:`boolean`
         Identifies if role can delete data.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.UpdatePermissionAsync(roleId, permissionId, permission);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissionId : :type:`string`, :required:`required`
         Identifies id of permission.
      permissibleName : :type:`string`, :required:`required`
         Name of permissible reference. An example would be 'meshes' or 'meshes.{meshName}' to identify access to a specific mesh.
      create : type:`boolean`
         Identifies if role can create data.
      update : type:`boolean`
         Identifies if role can update data.
      read : type:`boolean`
         Identifies if role can read data.
      delete : type:`boolean`
         Identifies if role can delete data.

.. rubric:: Responses

200 : OK
    * Identifies if permission was updated.

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

400 : Bad request
    * Permissible name is required.
    * At least one of the following must be set: Create, Update, Read, Delete.
    * Permissible does not exist.
    * Permisisble does not support the permission configuration.
    * Role does not exist.
    * Permissible was already configured for role.
    * A higher permissible cannot be assigned to a role with a specific permission already. IE you cannot have 'meshes' and 'meshes.person' for the role.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to update roles.

404 : Not Found
    * Permission was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

'''''''''
Add Users
'''''''''

Add users from specific role.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
         
        POST https://api.meshydb.com/{accountName}/roles/{roleId}/users HTTP/1.1
        Authorization: Bearer {access_token}
        Content-Type: application/json

        {
            "users": [
                        {
                            "id":"5db..."
                        }
                     ]
        }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.addUsers(roleId, batchRoleAdd);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      batchRoleAdd : :type:`object`, :required:`required`
         Batch object of user ids to be added from role.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        var batchRoleAdd = new BatchRoleAdd();

        await connection.Roles.AddUsersAsync(roleId, batchRoleAdd);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      batchRoleAdd : :type:`object`, :required:`required`
         Batch object of user ids to be added from role.

.. rubric:: Responses

204 : No Content
    * Identifies if role is added.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to update roles.

404 : Not Found
    * Role was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.