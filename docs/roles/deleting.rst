.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Deleting
--------

Delete information for specific role.

''''
Role
''''

Delete details about role by id.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.DeleteAsync(roleId);

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
      
        await meshyConnection.rolesService.delete(roleId);

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
         
        DELETE https://api.meshydb.com/{accountName}/roles/{roleId} HTTP/1.1
        Authentication: Bearer {access_token}

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.

.. rubric:: Responses

204 : No Content
    * Identifies if role was deleted.

400 : Bad request
    * Unable to delete role that starts with 'meshy.'.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to delete roles.

404 : Not Found
    * Role was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''
Permission
''''''''''

Delete specific permission from role by id.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        await connection.Roles.DeletePermissionAsync(roleId, permissionId);

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
      
        await meshyConnection.rolesService.deletePermission(roleId, permissionId);

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
         
        DELETE https://api.meshydb.com/{accountName}/roles/{roleId}/permissions/{permissionId} HTTP/1.1
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

204 : No Content
    * Identifies if permission was deleted.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to delete roles.

404 : Not Found
    * Permission was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''''
Remove Users
''''''''''''

Remove users from specific role.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        var batchRoleRemove = new BatchRoleRemove();

        await connection.Roles.RemoveUsersAsync(roleId, batchRoleRemove);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      batchRoleRemove : :type:`object`, :required:`required`
         Batch object of user ids to be removed from role.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);

        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.removeUsers(roleId, batchRoleRemove);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      batchRoleRemove : :type:`object`, :required:`required`
         Batch object of user ids to be removed from role.

   .. group-tab:: REST
   
      .. code-block:: http
         
        DELETE https://api.meshydb.com/{accountName}/roles/{roleId}/users HTTP/1.1
        Authentication: Bearer {access_token}
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

.. rubric:: Responses

204 : No Content
    * Identifies if role is removed.

401 : Unauthorized
   * User is not authorized to make call.
   
403 : Forbidden
    * User has insufficent permission to update roles.

404 : Not Found
    * Role was not found.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.