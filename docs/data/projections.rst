.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------------
Projections Data
---------------

Retrieve data from a defined projection.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         public class ProjectionTest
         {
            [JsonProperty("_id")]
            public string Id { get; set; }
            
            [JsonProperty("test")]
            public string Test { get; set; }
         }

         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
         
         var person = await connection.Projections.Get<ProjectionTest>(projectionName, sort, page, pageSize);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      projectionName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      sort : :type:`object`
         Defines which fields need to be sorted and direction.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var projectionData = await meshyConnection.projections.get<any>(projectionName, 
                                                                        {
                                                                            orderby: orderby,
                                                                            page: page,
                                                                            pageSize: pageSize
                                                                        });

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      projectionName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      sort : :type:`string`
         Defines which fields need to be sorted and direction in a MongoDB format.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: REST
   
      .. code-block:: http

         GET https://api.meshydb.com/{accountName}/projections/{projectionName} HTTP/1.1
         Authentication: Bearer {access_token}
            
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      projectionName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      sort : :type:`string`
         Defines which fields need to be sorted and direction in a MongoDB format.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.


.. rubric:: Responses

200 : OK
   * Projection found with given identifier.

Example Result

.. code-block:: json

   {
      "page": 1,
      "pageSize": 25,
      "results": [{
                     "_id":"5c78cc81dd870827a8e7b6c4",
                     "test": "Projection Test"
                 }],
      "totalRecords": 1
   }

400 : Bad request
   * Projection name is required.
   * Order by is invalid.

401 : Unauthorized
   * User is not authorized to make call.
   
404 : Not Found
   * Projection was not found.

429 : Too many request
   * You have have either hit your API or Database limit. Please review your account.