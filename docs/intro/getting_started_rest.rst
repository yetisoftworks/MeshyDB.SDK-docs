.. role:: required

.. role:: type

====
REST
====

----------------------
Before we get started!
----------------------
This documentation assumes you have an active MeshyDB account. If you do not, please create a free account at `https://meshydb.com <https://meshydb.com/>`_.

.. |publicKey| raw:: html

    <code>Public Key</code>

Once your account is verified, you will need to gather your |publicKey| from the Clients page under your default tenant. See image below:

.. |gettingStarted| image:: https://cdn.meshydb.com/images/getting-started-client.png
           :alt: Public Key under Clients default tenant

|gettingStarted|

In the following we will assume no other configuration has been made to your account or tenants so we can just begin!

Now that we have the required information let's jump in and see how easy it is to start with MeshyDB.

.. |parameters| raw:: html

   <h4>Parameters</h4>

--------------------------
Registering Anonymous User
--------------------------
Anonymous users are great for associating data to people without having them go through any type of user registration. Simply create the user behind the scenes with a unique identifier and begin recording data for that user.

To get started we need a user to log in with. We will make an anonymous user to authenticate with going forward.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
        POST https://api.meshydb.com/{accountName}/users/register/anonymous HTTP/1.1
        Content-Type: application/json
         
          {
            "username": "2d4c2a18-2596-4ba9-b657-3413d5974502"
          }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

Example Response:

.. code-block:: json

  {
    "id": "5c...",
    "username": "2d4c2a18-2596-4ba9-b657-3413d5974502",
    "firstName": null,
    "lastName": null,
    "verified": false,
    "isActive": true,
    "phoneNumber": null,
    "emailAddress": null,
    "roles": [],
    "securityQuestions": [],
	 "anonymous": true
  }
   
-----
Login
-----
All data interaction must be done on behalf of a user. To start interacting with data establish a connection as that user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         POST https://auth.meshydb.com/{accountName}/connect/token HTTP/1.1
         Content-Type: application/x-www-form-urlencoded
         
            client_id={publicKey}&
            grant_type=password&
            username={username}&
            password=nopassword&
            scope=meshy.api offline_access

      (Form-encoding removed, and line breaks added for readability)

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      publicKey : :type:`string`, :required:`required`
         Public accessor for application.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
   
Example Response:

.. code-block:: json

  {
    "access_token": "ey...",
    "expires_in": 3600,
    "token_type": "Bearer",
    "refresh_token": "ab23cd3343e9328g"
  }
 
-----------
Create data
-----------
Now that we are logged in we can use our Bearer token to authenticate requests with MeshyDB and create some data.

The data object can whatever information you would like to capture. The following example will have some data fields with example data.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         POST https://api.meshydb.com/{accountName}/meshes/{meshName} HTTP/1.1
         Authentication: Bearer {access_token}
         Content-Type: application/json
         
            {
               "firstName": "Bob",
               "lastName": "Bobberson"
            }

      |parameters|

      accountName: :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token: :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.

Example Response:

.. code-block:: json

  {
    "_id":"5c78cc81dd870827a8e7b6c4",
    "firstName": "Bob",
    "lastName": "Bobberson"
  }
  
-----------
Update data
-----------
If we need to make a modification let's update our Mesh!

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

       PUT https://api.meshydb.com/{accountName}/meshes/{meshName}/{id}  HTTP/1.1
       Authentication: Bearer {access_token}
       Content-Type: application/json
         
          {
             "firstName": "Bobbo",
             "lastName": "Bobberson"
          }

      |parameters|

      accountName: :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token: :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to replace.

Example Response:

.. code-block:: json

  {
    "_id":"5c78cc81dd870827a8e7b6c4",
    "firstName": "Bobbo",
    "lastName": "Bobberson"
  }

-----------
Search data
-----------
Let's see if we can find Bobbo.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         GET https://api.meshydb.com/{accountName}/meshes/{meshName}?filter={filter}&
                                                               orderby={orderby}&
                                                               page={page}&
                                                               pageSize={pageSize} HTTP/1.1
         Authentication: Bearer {access_token}
         
      (Line breaks added for readability)

      |parameters|

      accountName: :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token: :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderby : :type:`string`
         Defines which fields need to be sorted and direction in a MongoDB format.
      page : :type:`integer`
         Page number of results to bring back.
      pageSize : :type:`integer`, max: 200, default: 25
         Number of results to bring back per page.

Example Response:

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

-----------
Delete data
-----------
We are now done with our data, so let us clean up after ourselves.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         DELETE https://api.meshydb.com/{accountName}/meshes/{meshName}/{id} HTTP/1.1
         Authentication: Bearer {access_token}
         
      |parameters|

      accountName: :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      access_token: :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.

--------
Sign out
--------
Now the user is complete. Let us sign out so someone else can have a try.

.. tabs::

   .. group-tab:: REST
   
      .. sourcecode:: http

         POST https://auth.meshydb.com/{accountName}/connect/revocation HTTP/1.1
         Content-Type: application/x-www-form-urlencoded
         
           client_id={accountName}&
           grant_type=refresh_token&
           token={refresh_token}

         
      (Line breaks added for readability)
         
      |parameters|

      accountName: :type:`string`, :required:`required`
         Indicates which account you are connecting for authentication.
      refresh_token: :type:`string`, :required:`required`
        Token to allow reauthorization with MeshyDB after the access token expires requested during `Login`_.