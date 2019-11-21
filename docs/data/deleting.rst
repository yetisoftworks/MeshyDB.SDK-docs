.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------
Deleting Data
-------------

Permanently remove Mesh data from collection.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
         
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
      
         await connection.Meshes.DeleteAsync(person);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      meshName : :type:`string`, :required:`required`, default: class name
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.
		 
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         await meshyConnection.meshes.delete(meshName, id);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.

   .. group-tab:: REST
   
      .. code-block:: http
      
         DELETE https://api.meshydb.com/{accountName}/meshes/{mesh}/{id} HTTP/1.1
         Authorization: Bearer {access_token}
         
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.

.. rubric:: Responses

204 : No Content
   * Mesh has been deleted successfully.

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to delete meshes or mesh.

404 : Not Found
   * Mesh data was not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.