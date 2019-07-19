.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Retrieving
--------
Retrieve single item from Mesh collection.

.. tabs::

    .. group-tab:: REST
   
        .. code-block:: http

            GET https://api.meshydb.com/{accountName}/meshes/{mesh}/{id} HTTP/1.1
            Authentication: Bearer {access_token}
            Content-Type: application/json
            tenant: {tenant}

                {
                    "firstName": "Bob",
                    "lastName": "Bobberson"
                }
            
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to retrieve.

   .. group-tab:: C#
   
      .. code-block:: c#

         // Mesh is derived from class name
         public class Person: MeshData
         {
           public string FirstName { get; set; }
           public string LastName { get; set; }
         }

         var client = MeshyClient.InitializeWithTenant(accountName, tenant, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
         
         var person = await connection.Meshes.GetData<Person>(id);

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      mesh : :type:`string`, default: class name
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to retrieve.

    .. group-tab:: NodeJS
      
        .. code-block:: javascript
         
            var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);
            
            var anonymousUser = await client.registerAnonymousUser();

            var meshyConnection = await client.loginAnonymously(anonymousUser.username);

            var meshData = await meshyConnection.meshes.get(meshName, id);

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, the system will assume to use the default client.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to retrieve.

Example Response:

.. code-block:: json

    {
        "_id":"5c78cc81dd870827a8e7b6c4",
        "firstName": "Bob",
        "lastName": "Bobberson"
    }
