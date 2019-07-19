.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Deleting
--------
Permanently remove Mesh data from collection.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         DELETE https://api.meshydb.com/{accountName}/meshes/{mesh}/{id} HTTP/1.1
         Authentication: Bearer {access_token}
         tenant: {tenant}
         
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to replace.

   .. group-tab:: C#
   
      .. code-block:: c#
         
         var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
      
         await connection.Meshes.DeleteAsync(person);

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         User name.
      meshName : :type:`string`, :required:`required`, default: class name
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to replace.
		 
   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         await meshyConnection.meshes.delete(meshName, id);
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         User name.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to delete. In this case it will be from the Person mesh.
