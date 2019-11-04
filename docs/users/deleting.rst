.. role:: required

.. role:: type

.. |parameters| raw:: html

   <h4>Parameters</h4>
   
--------
Deleting
--------
Remove user from system permanently.

.. tabs::

   .. group-tab:: C#
   
      .. code-block:: c#
      
         var client = MeshyClient.Initialize(accountName, publicKey);
         var connection = await client.LoginAnonymouslyAsync(username);

         await connection.Users.DeleteAsync(id);

      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      id : :type:`string`, :required:`required`
         Identifies id of user.

   .. group-tab:: NodeJS
      
      .. code-block:: javascript
         
         var client = MeshyClient.initialize(accountName, publicKey);
         
         var anonymousUser = await client.registerAnonymousUser();

         var meshyConnection = await client.loginAnonymously(anonymousUser.username);

         await meshyConnection.usersService.delete(id);
      
      |parameters|

      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      publicKey : :type:`string`, :required:`required`
         Public identifier of connecting service.
      username : :type:`string`, :required:`required`
         Unique identifier for user or device.
      id : :type:`string`, :required:`required`
         Identifies id of user.

   .. group-tab:: REST
   
      .. code-block:: http
      
         DELETE https://api.meshydb.com/{accountName}/users/{id} HTTP/1.1
         Authentication: Bearer {access_token}
         
      |parameters|
      
      accountName : :type:`string`, :required:`required`
         Indicates which account you are connecting to.
      access_token : :type:`string`, :required:`required`
         Token identifying authorization with MeshyDB requested during `Generating Token <../authorization/generating_token.html#generating-token>`_.
      id : :type:`string`, :required:`required`
         Identifies id of user.

.. rubric:: Responses

204 : No Content
   * Identifies if user was deleted.

400 : Bad request
    * User is not able to delete self.
    
401 : Unauthorized
   * User is not authorized to make call.

403 : Forbidden
    * User has insufficent permission to delete users.

404 : Not Found
    * User is not found.

429 : Too many request
   * You have either hit your API or Database limit. Please review your account.