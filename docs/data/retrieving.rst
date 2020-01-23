.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------------
Retrieving Data
---------------

Retrieve single item from Mesh collection.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         // Mesh is derived from class name
         public class Person: MeshData
         {
           public string FirstName { get; set; }
           public string LastName { get; set; }
         }

         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
         
         var person = await connection.Meshes.GetData<Person>(id);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      mesh : :type:`string`, default: class name
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to retrieve.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var meshData = await meshyConnection.meshes.get(meshName, id);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to retrieve.

   .. group-tab:: REST
   
      .. code-block:: http

         GET https://api.meshydb.com/{accountName}/meshes/{mesh}/{id} HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
            
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies location of what Mesh data to retrieve.

.. rubric:: Responses

200 : OK
   * Mesh data found with given identifier.

Example Result

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Bob",
      "lastName": "Bobberson"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.

401 : Unauthorized
   * User is not authorized to make call.
   
403 : Forbidden
   * User has insufficent permission to read meshes or mesh.

404 : Not Found
   * Mesh data was not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.