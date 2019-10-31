.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------------
Projecting Data
---------------

A projection is a stored MongoDB query that can contain aggregations, filtering and lookups. It’s great for reporting or simply re-using queries throughout your application. Projections also give you the ability to control access to data by allowing you to explicitly define the data you want to make accessible to end-users.

You can create your query your Mesh Data using the admin portal under the Projections tab.

Please review the following for supported `aggregations <#supported-aggregates>`_.

`````````````
API Reference
`````````````

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         public class PopularState
         {
            [JsonProperty("state")]
            public string State { get; set; }
            
            [JsonProperty("attractions")]
            public int Attractions { get; set; }
         }

         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);
         
         var stateAttractions = await connection.Projections.Get<PopularState>(projectionName,
                                                                               filter,
                                                                               orderBy, 
                                                                               page, 
                                                                               pageSize);

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      projectionName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`object`
         Defines which fields need to be ordered and direction. Review more ways to use `ordering <#ordering-data>`_.
      page : :type:`integer`, default: 1
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
      
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         var popularStates = await meshyConnection.projections.get<any>(projectionName, 
                                                                        {
                                                                            filter: filter,
                                                                            orderBy: orderBy,
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
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format. Review more ways to use `ordering <#ordering-data>`_.
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
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format. Review more ways to use `ordering <#ordering-data>`_.
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
                     "state":"WI",
                     "attractions":"24"
                 }],
      "totalRecords": 1
   }

400 : Bad request
   * Projection name is required.
   * Order by is invalid.

401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
   * User has insufficent permission to read projections.

404 : Not Found
   * Projection was not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

``````````````
Ordering Data
``````````````

Ordering is supported in a MongoDB format. This format is as an object with a -1 or 1 to identify descending or ascending format respectively.

The following example shows how to sort an object by Name in descending order.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         var orderBy = OrderByDefinition<PopularState>.OrderByDescending("Name");

         // Or

         orderBy = OrderByDefinition<PopularState>.OrderByDescending(x => x.Name);

         var popularStates = await connection.Projections.Get<PopularState>(projectionName, 
                                                                            orderBy, 
                                                                            page, 
                                                                            pageSize);


      Alternatively you can use MongoDB syntax.

      .. code-block:: c#

         var orderBy = "{ \"Name\": -1 }";

         var popularStates = await connection.Projections.Get<PopularState>(projectionName, 
                                                                            orderBy, 
                                                                            page, 
                                                                            pageSize);

   .. group-tab:: NodeJS
      
      .. code-block:: javascript

         var orderBy = { "Name": -1 };

         var popularStates = await meshyConnection.projections.get<any>(projectionName, 
                                                                        {
                                                                            orderBy: orderBy,
                                                                            page: page,
                                                                            pageSize: pageSize
                                                                        });

   .. group-tab:: REST
   
      .. code-block:: http

         GET https://api.meshydb.com/{accountName}/projections/{projectionName}?orderBy={ "Name": -1 } HTTP/1.1
         Authentication: Bearer {access_token}

Additional filters can be extended as follows. 

This example will order by Name descending then Age ascending.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         var orderBy = OrderByDefinition<Person>.OrderByDescending("Name").ThenBy("Age");

         // Or

         orderBy = OrderByDefinition<Person>.OrderByDescending(x => x.Name).ThenBy(x=> x.Age);

         var popularStates = await connection.Projections.Get<PopularState>(projectionName, 
                                                                            orderBy, 
                                                                            page, 
                                                                            pageSize);

      Alternatively you can use MongoDB syntax

      .. code-block:: c#

         var orderBy = "{ \"Name\": -1, \"Age\": 1 }";

         var popularStates = await connection.Projections.Get<PopularState>(projectionName, 
                                                                            orderBy, 
                                                                            page, 
                                                                            pageSize);

   .. group-tab:: NodeJS
      
      .. code-block:: javascript

         var orderBy = { "Name": -1, "Age": 1 };

         var popularStates = await meshyConnection.projections.get<any>(projectionName, 
                                                               {
                                                                     orderBy: orderBy,
                                                                     page: page,
                                                                     pageSize: pageSize
                                                               });

   .. group-tab:: REST
   
      .. code-block:: http

         GET https://api.meshydb.com/{accountName}/projections/{projectionName}?orderBy={ "Name": -1, "Age": 1 } HTTP/1.1
         Authentication: Bearer {access_token}

````````````````````
Supported Aggregates
````````````````````

The following aggregates are from MongoDB and more detailed explanation can be found `here <https://docs.mongodb.com/manual/reference/operator/aggregation-pipeline/>`_.

	 - $addFields
	 - $bucket
	 - $bucketAuto
	 - $count
	 - $graphLookup
	 - $facet
	 - $group
	 - $limit
	 - $lookup
	 - $match
	 - $project
	 - $redact
	 - $replaceRoot
	 - $sample
	 - $skip
	 - $sort
	 - $sortByCount
	 - $unwind