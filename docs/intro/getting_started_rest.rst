.. role:: required

.. role:: type

====
REST
====

----------------------
Before you get started!
----------------------
This documentation assumes you have an active MeshyDB account. If you do not, please create a free account at `https://meshydb.com <https://meshydb.com/>`_.

.. |publicKey| raw:: html

    <code><a href="#identify-public-key">Public Key</a></code>

.. |accountName| raw:: html

    <code><a href="#identify-account-name">Account Name</a></code>

Once you verify your account you will need to gather your |accountName| and |publicKey|.

Identify Account Name
~~~~~~~~~~~~~~~~~~~~~

Your |accountName| can be found under the Account page. See image below:

.. |gettingStartedAccount| image:: https://cdn.meshydb.com/images/getting-started-account.png
           :alt: Account Name under Account

|gettingStartedAccount|

Identify  Public Key
~~~~~~~~~~~~~~~~~~~~

Your |publicKey| can be found under the Clients page under your default tenant. See image below:

.. |gettingStartedClient| image:: https://cdn.meshydb.com/images/getting-started-client.png
           :alt: Public Key under Clients default tenant

|gettingStartedClient|

In the following we will assume no other configuration has been made to your account or tenants so we can just begin!

.. |parameters| raw:: html

   <h4>Parameters</h4>


------------------------
Checking Username Exists
------------------------

Checking username helps identify if a device or user has already registered.

The example below shows verifying a username is available.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
         
        GET https://api.meshydb.com/{accountName}/users/{username}/exists HTTP/1.1

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

.. rubric:: Responses

201 : Created
   * Identifies if username already exists.

Example Result

.. code-block:: json

   {
      "exists": false
   }

400 : Bad request
   * Username is required.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------------------
Register Anonymous User
-----------------------

Anonymous users are great for associating data to people or devices without having them go through any type of user registration.

The example below shows registering an anonymous user.
   
.. tabs::
   
   .. group-tab:: REST
   
      .. code-block:: http

        POST https://api.meshydb.com/{accountName}/users/register/anonymous HTTP/1.1
        Content-Type: application/json
         
          {
            "username": "mctesterton"
          }

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.

.. rubric:: Responses

201 : Created
   * New user has been registered and is now available for use.

Example Result

.. code-block:: json

   {
      "id": "5c78cc81dd870827a8e7b6c4",
      "username": "mctesterton",
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

400 : Bad request
   * Username is a required field.
   * Anonymous registration is not enabled.
   * Username must be unique.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----
Login
-----

All data interaction must be done on behalf of a user. This is done to ensure proper authorized access of your data.

The example below shows logging in an anonymous user.

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
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      password : :type:`string`, :required:`required`
         User secret credentials for login. When anonymous it is static as nopassword.
   
.. rubric:: Responses

200 : OK
   * Generates new credentials for authorized user. The token will expire and will need to be refreshed.

Example Result

.. code-block:: json

  {
    "access_token": "ey...",
    "expires_in": 3600,
    "token_type": "Bearer",
    "refresh_token": "ab23cd3343e9328g"
  }
 
400 : Bad request
   * Token is invalid.
   * Client id is invalid.
   * Grant type is invalid.
   * User is no longer active.
   * Invalid Scope.
   * Username is invalid.
   * Password is invalid.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

---------------
Retrieving Self
---------------

When a user is created they have some profile information that helps identify them. We can use this information to link their id back to data that has been created.

The example below shows retrieving information of the user.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         GET https://api.meshydb.com/{accountName}/users/me HTTP/1.1
         Authentication: Bearer {access_token}
         
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.

.. rubric:: Responses

200 : OK
   * Retrieves information about the authorized user.

Example Result

.. code-block:: json

   {
      "id": "5c78cc81dd870827a8e7b6c4",
      "username": "mctesterton",
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

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Create data
-----------

.. |meshData| raw:: html

   <code>Mesh Data</code>
   
Now that a user is authorized you can begin making API requests.

The example below shows committing a new |meshData| such as a person.

.. tabs::
   
   .. group-tab:: REST
   
      .. code-block:: http

         POST https://api.meshydb.com/{accountName}/meshes/{meshName} HTTP/1.1
         Authentication: Bearer {access_token}
         Content-Type: application/json
         
            {
               "firstName": "Bob",
               "lastName": "Bobson",
               "userId": "5c78cc81dd870827a8e7b6c4"
            }

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.


.. rubric:: Responses

201 : Created
   * Result of newly created mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5d438ff23b0b7dd957a765ce",
      "firstName": "Bob",
      "lastName": "Bobson",
      "userId": "5c78cc81dd870827a8e7b6c4"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Update data
-----------

The API allows you to make updates to specific |meshData| by targeting the id.

The example below shows modifying the first name and committing those changes to the API.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

       PUT https://api.meshydb.com/{accountName}/meshes/{meshName}/{id}  HTTP/1.1
       Authentication: Bearer {access_token}
       Content-Type: application/json
         
          {
             "firstName": "Robert",
             "lastName": "Bobson"
          }

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to replace.


.. rubric:: Responses

200 : OK
   * Result of updated mesh data.

Example Result

.. code-block:: json

   {
      "_id":"5d438ff23b0b7dd957a765ce",
      "firstName": "Robert",
      "lastName": "Bobson",
      "userId": "5c78cc81dd870827a8e7b6c4"
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Mesh property cannot begin with '$' or contain '.'.

401 : Unauthorized
   * User is not authorized to make call.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Search data
-----------

The API allows you to search |meshData| using a MongoDB expression.

The example below shows searching based where the first name starts with Rob.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
	  
         GET https://api.meshydb.com/{accountName}/meshes/{meshName}?filter={ 'firstName': { "$regex": "^Rob" } } HTTP/1.1
         Authentication: Bearer {access_token}
         
      (Encoding removed for readability)

      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      filter : :type:`string`
         Criteria provided in a MongoDB format to limit results.
      orderBy : :type:`string`
         Defines which fields need to be ordering and direction in a MongoDB format.
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
      "results":  [{
                     "_id":"5d438ff23b0b7dd957a765ce",
                     "firstName": "Robert",
                     "lastName": "Bobson",
                     "userId": "5c78cc81dd870827a8e7b6c4"
                  }],
      "totalRecords": 1
   }

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.
   * Filter is in an invalid format. It must be in a valid Mongo DB format.
   * Order by is in an invalid format. It must be in a valid Mongo DB format.

401 : Unauthorized
   * User is not authorized to make call.
   
429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

-----------
Delete data
-----------

The API allows you to delete a specific |meshData| by targeting the id.

The example below shows deleting the data from the API by providing the object.

.. |softDelete| raw:: html
   
   <code>Soft Delete</code>

*Deleted* data is not able to be recovered. If you anticipate the need to recover this data please implementing a |softDelete|.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http
      
         DELETE https://api.meshydb.com/{accountName}/meshes/{meshName}/{id} HTTP/1.1
         Authentication: Bearer {access_token}
         
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Login`_.
      meshName : :type:`string`, :required:`required`
         Identifies name of mesh collection. e.g. person.
      id : :type:`string`, :required:`required`
         Identifies unique record of Mesh data to remove.

.. rubric:: Responses

204 : No Content
   * Mesh has been deleted successfully.

400 : Bad request
   * Mesh name is invalid and must be alpha characters only.

401 : Unauthorized
   * User is not authorized to make call.

404 : Not Found
   * Mesh data was not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

--------
Sign out
--------

When a user is authenticated a refresh token  is generated. The refresh token allows a user to be silently authenticated.

As a result it is recommended to implement Sign Out to ensure the current user is logged out and all refresh tokens are revoked.

The example below shows revoking the refresh token. The access token is short lived and will expire within an hour.

.. tabs::

   .. group-tab:: REST
   
      .. code-block:: http

         POST https://auth.meshydb.com/{accountName}/connect/revocation HTTP/1.1
         Content-Type: application/x-www-form-urlencoded
         
           client_id={accountName}&
           grant_type=refresh_token&
           token={refresh_token}

         
      (Line breaks added for readability)
         
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      refresh_token : :type:`string`, :required:`required`
        Token to allow reauthorization with MeshyDB after the access token expires requested during `Login`_.

.. rubric:: Responses

200 : OK
   * Identifies successful logout.

400 : Bad request
   * Invalid client id.
   * Token is missing.
   * Unsupported Token type.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.

Not seeing something you need? Feel free to give us a chat or contact us at support@meshydb.com.