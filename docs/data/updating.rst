.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
-------------
Updating Data
-------------

Update Mesh data in collection by id.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
         var person = await connection.Meshes.GetDataAsync<Person>(id);         

         person.FirstName = "Bobbo";

         person = await connection.Meshes.UpdateAsync(person);
         
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      mesh : :type:`string`, :required:`required`, default: class name
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to replace.


   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var meshData = await meshyConnection.meshes.update(meshName, 
                                                            {
                                                               firstName:"Bob",
                                                               lastName:"Bobberson"
                                                            },
                                                            id);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to replace.

   .. group-tab:: REST
   
      .. code-block:: http

         PUT https://api.meshydb.com/{accountName}/meshes/{mesh}/{id}  HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json

         {
            "firstName": "Bobbo",
            "lastName": "Bobberson"
         }

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to replace.

.. rubric:: Responses

200 : OK
   * Result of updated mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Bobbo",
      "lastName": "Bobberson"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to update meshes or mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.