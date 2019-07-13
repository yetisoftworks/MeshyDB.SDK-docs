.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Creating
--------
Create new data into Mesh collection. If it is the first time this Mesh is being used a new collection will be created automatically.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         POST https://api.meshydb.com/{accountName}/meshes/{mesh} HTTP/1.1
         Authentication: Bearer {access_token}
         Content-Type: application/json
         tenant: {tenant}
         
            {
               "firstName": "Bob",
               "lastName": "Bobberson"
            }
            
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
   
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
         
         var person = await connection.Meshes.CreateAsync(new Person(){
           FirstName="Bob",
           LastName="Bobberson"
         });

      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Username of user.
      mesh : :type:`string`, default: class name
         Identifies name of mesh collection. e.g. person.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initializeWithTenant(accountName, tenant, publicKey);

         client.loginAnonymously(username)
               .then(function (meshyConnection){
                        var refreshToken = meshyConnection.meshes.create(meshName, 
                                                                        {
                                                                           firstName:"Bob",
                                                                           lastName:"Bobberson"
                                                                        })
                                                                 .then(function(result) { });
               }); 
      
      |parameters|

      tenant : :type:`string`, :required:`required`
         Indicates which tenant data to use. If not provided, it will use the configured default.
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Username of user.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
         
Example Response:

.. code-block:: json

   {
      "_id":"5c78cc81dd870827a8e7b6c4",
      "firstName": "Bob",
      "lastName": "Bobberson"
   }
