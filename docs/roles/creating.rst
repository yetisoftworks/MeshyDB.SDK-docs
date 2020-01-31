.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Creating
--------

Roles allow the ability to setup specific permissions for users.

These permissions can either be set as a blanket allow for data or be granlar to specific data sets.

''''
Role
''''

Creating a role allows it to be assignable to a user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
         
        POST https://api.meshydb.com/{accountName}/roles HTTP/1.1
        Authorization: Bearer {access_token}
        Content-Type: application/json

        {
            "name":"test",
            "description":"..."
        }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      name : :type:`string`, :required:`required`
         Name of the role.
      description : :type:`string`
         Describes the purpose of the role.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);
        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.create(role);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      name : :type:`string`, :required:`required`
         Name of the role.
      description : :type:`string`
         Describes the purpose of the role.
      numberOfUsers : :type:`string`
         Read-only count of users assigned to the role.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        var role = new Role();

        await connection.Roles.CreateAsync(role);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      name : :type:`string`, :required:`required`
         Name of the role.
      description : :type:`string`
         Describes the purpose of the role.
      numberOfUsers : :type:`string`
         Read-only count of users assigned to the role.
		
.. rubric:: Responses

201 : Created
    * Identifies if role was created.

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
    * User has insufficent permission to create roles.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.

''''''''''
Permission
''''''''''

When creating a permission it is assigned to a role. When a user has the role this permission will take effect on their next signin/token refresh.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
         
        POST https://api.meshydb.com/{accountName}/roles/{roleId}/permissions HTTP/1.1
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
      permissibleName : :type:`string`, :required:`required`
         Name of permissible reference. An example would be 'meshes' or 'meshes.{meshName}' to identify access to a specific mesh.
      create : :type:`boolean`
         Identifies if role can create data.
      update : :type:`boolean`
         Identifies if role can update data.
      read : :type:`boolean`
         Identifies if role can read data.
      delete : :type:`boolean`
         Identifies if role can delete data.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
        var client = MeshyClient.initialize(accountName, publicKey);
        var meshyConnection = await client.loginAnonymously(username);
      
        await meshyConnection.rolesService.createPermission(roleId, permission);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissibleName : :type:`string`, :required:`required`
         Name of permissible reference. An example would be 'meshes' or 'meshes.{meshName}' to identify access to a specific mesh.
      create : :type:`boolean`
         Identifies if role can create data.
      update : :type:`boolean`
         Identifies if role can update data.
      read : :type:`boolean`
         Identifies if role can read data.
      delete : :type:`boolean`
         Identifies if role can delete data.

   .. group-tab:: C#
   
      .. code-block:: c#
      
        var client = MeshyClient.Initialize(accountName, publicKey);
        var connection = await client.LoginAnonymouslyAsync(username);

        var permission = new Permission();

        await connection.Roles.CreatePermissionAsync(roleId, permission);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      roleId : :type:`string`, :required:`required`
         Identifies id of role.
      permissibleName : :type:`string`, :required:`required`
         Name of permissible reference. An example would be 'meshes' or 'meshes.{meshName}' to identify access to a specific mesh.
      create : :type:`boolean`
         Identifies if role can create data.
      update : :type:`boolean`
         Identifies if role can update data.
      read : :type:`boolean`
         Identifies if role can read data.
      delete : :type:`boolean`
         Identifies if role can delete data.

.. rubric:: Responses

201 : Created
    * Identifies if role was created.

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
    * User has insufficent permission to create permissions.

429 : Too many request
    * You have either hit your API or Database limit. Please review your account.