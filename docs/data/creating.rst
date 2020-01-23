.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Creating
--------

Create new custom mesh data into specified mesh name.

```````
Single
```````

Create a specific record.

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
         
         var person = await connection.Meshes.CreateAsync(new Person(){
           FirstName="Bob",
           LastName="Bobberson"
         });

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      mesh : :type:`string`, default: class name
         Identifies name of mesh collection. e.g. person.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);

         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var createdMesh = await meshyConnection.meshes.create(meshName, 
                                                         {
                                                            firstName:"Bob",
                                                            lastName:"Bobberson"
                                                         });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
   
   .. group-tab:: REST
   
      .. code-block:: http

         POST https://api.meshydb.com/{accountName}/meshes/{mesh} HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
            {
               "firstName": "Bob",
               "lastName": "Bobberson"
            }
            
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
         
.. rubric:: Responses

201 : Created
   * Result of newly created mesh data.

Example Result

.. code-block:: json

   {
      "_id": "5c78cc81dd870827a8e7b6c4",
      "firstName": "Bob",
      "lastName": "Bobberson"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Mesh property cannot begin with '$' or contain '.'.
   * Mesh already exists for provided id.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to create meshes or specific mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

````
Many
````

Bulk create many objects.

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
         var data = new List<Person>();
         
         data.Add(new Person(){
           FirstName="Bob",
           LastName="Bobberson"
         });

         var result = await connection.Meshes.CreateMany(data);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      mesh : :type:`string`, default: class name
         Identifies name of mesh collection. e.g. person.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var client = MeshyClient.initialize(accountName, publicKey);
         var anonymousUser = await client.registerAnonymousUser();
         var connection = await client.loginAnonymously(anonymousUser.username);

         var result = await connection.meshesService.createMany(meshName, [{
                                                                              firstName:"Bob",
                                                                              lastName:"Bobberson"
                                                                          }]);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.

   .. group-tab:: REST
   
      .. code-block:: http

         POST https://api.meshydb.com/{accountName}/meshes/{mesh} HTTP/1.1
         Authorization: Bearer {access_token}
         Content-Type: application/json
         
            [{
               "firstName": "Bob",
               "lastName": "Bobberson"
            }]

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.

.. rubric:: Responses

201 : Created
   * Result of newly created mesh data.

Example Result

.. code-block:: json

   {
      "createdCount": 1
   }

400 : Bad request
   * No data was provided.
   * Data is in an invalid format. The status of each object will be brought back to identify the error. The errors are as follows:
      * Mesh name is invalid and must be alpha characters only.
      * Mesh property cannot begin with '$' or contain '.'.
      * Mesh already exists for provided id.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to create meshes or specific mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.