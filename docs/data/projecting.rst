.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
---------------
Projecting Data
---------------

A Projection is a stored query of MongoDB aggregates and filters that can be called for reporting or analysis.

`````````````
API Reference
`````````````

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
         
         var person = await connection.Projections.Get<ProjectionTest>(projectionName, 
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
      orderBy : :type:`object`
         Defines which fields need to be ordered and direction. Review more ways to use `ordering <Ordering Data>`_.
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
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format. Review more ways to use `ordering <Ordering Data>`_.
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
      orderBy : :type:`string`
         Defines which fields need to be ordered and direction in a MongoDB format. Review more ways to use `ordering <Ordering Data>`_.
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

``````````````
Ordering Data
``````````````

Ordering is supported in a MongoDB format. This format is as an object with a -1 or 1 to identify descending or ascending format respectively.

The following example shows how to sort an object by Name in descending order.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         OrderByDefinition<Person>.OrderByDescending("Name");

         // Or

         OrderByDefinition<Person>.OrderByDescending(x => x.Name);

      Alternatively you can use MongoDB syntax

      .. code-block:: json

         { "Name": -1 }
      
   .. group-tab:: NodeJS
      
      .. code-block:: json

         { "Name": -1 }

   .. group-tab:: REST
   
      .. code-block:: json

         { "Name": -1 }


To add additional filters it can be extended as follows.

This example will order by Name descending then Age ascending.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#

         OrderByDefinition<Person>.OrderByDescending("Name").ThenBy("Age");

         // Or

         OrderByDefinition<Person>.OrderByDescending(x => x.Name).ThenBy(x=> x.Age);

      Alternatively you can use MongoDB syntax

      .. code-block:: json

         { "Name": -1, "Age": 1 }
      
   .. group-tab:: NodeJS
      
      .. code-block:: json

         { "Name": -1, "Age": 1 }

   .. group-tab:: REST
   
      .. code-block:: json

         { "Name": -1, "Age": 1 }