.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------
Searching
---------

Filter Mesh data from collection based on query parameters.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         GET https://api.meshydb.com/{accountName}/meshes/{mesh}?filter={filter}&
                                                               orderBy={orderBy}&
                                                               page={page}&
                                                               pageSize={pageSize} HTTP/1.1
         Authorization: Bearer {access_token}
         
      (Line breaks added for readability)

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      mesh : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var pagedResults = await meshyConnection.meshes.search(meshName, 
                                                               {
                                                                  filter: filter,
                                                                  orderBy: orderBy,
                                                                  pageNumber: page,
                                                                  pageSize: pageSize
                                                               });
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      username : :type:`string`
         Unique user name for authentication.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         var pagedPersonResult = await connection.Meshes.SearchAsync<Person>(filter, page, pageSize);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique user name for authentication.
      mesh : :type:`string`, :required:`required`, default: class name
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.


.. rubric:: Responses

200 : OK
   * Mesh data found with given search criteria.

Example Result

.. code-block:: json

   {
      "page": 1,
      "pageSize": 25,
      "results": [{
                     "_id":"5c78cc81dd870827a8e7b6c4",
                     "firstName": "Bobbo",
                     "lastName": "Bobberson"
                 }],
      "totalRecords": 1
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Filter is in an invalid format. It must be in a valid Mongo DB format.
   * Order by is in an invalid format. It must be in a valid Mongo DB format.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to read meshes or mesh.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.