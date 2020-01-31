.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>

--------
Deleting
--------

Delete mesh data from a collection is permanent. If you desire to retain your data one possible pattern to consider would be a soft-delete pattern.

``````
Single
``````

Delete a specific record of data.

.. tabs::

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
         Unique user name for authentication.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.

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
         Unique user name for authentication.
      meshName : :type:`string`, :required:`required`, default: class name
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

````
Many
````

Delete all objects with the provided filter.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         DELETE https://api.meshydb.com/{accountName}/meshes/{mesh}?filter={filter} HTTP/1.1
         Authorization: Bearer {access_token}

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`, :required:`required`
         Criteria provided in a MongoDB format to limit results.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript

         var client = MeshyClient.initialize(accountName, publicKey);
         var anonymousUser = await client.registerAnonymousUser();
         var connection = await client.loginAnonymously(anonymousUser.username);

         var data = await connection.meshesService.deleteMany(meshName, filter)
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      meshName : :type:`string`, :required:`required`, default: class name
         Identifies name of mesh collection. e.g. person.
      filter : :type:`object`, :required:`required`
         Criteria provided in a MongoDB format to limit results.
   
   .. group-tab:: C#
   
      .. code-block:: c#

         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var result = connection.Meshes.DeleteMany<DeleteManyTest>(filter);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      meshName : :type:`string`, :required:`required`, default: class name
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`, :required:`required`
         Criteria provided in a MongoDB format to limit results.

.. rubric:: Responses

200 : OK
   * Mesh data found with given search criteria and successfully deleted.

Example Result

.. code-block:: json

   {
      "deletedCount": 5,
      "isAcknowledged": true
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Filter was not provided.
   * Filter is in an invalid format.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to delete meshes or mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.