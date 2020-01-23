=======
Welcome
=======

--------------
What is Meshy?
--------------

MeshyDB gives you a fully functional API backend in minutes. We take care of the bulky time-consuming API, letting you focus on the front-end design. Build apps faster by leveraging the MeshyDB backend.

-----------------------
Before you get started!
-----------------------

This documentation assumes you have an active MeshyDB account. If you do not, please create a free account at `https://meshydb.com <https://meshydb.com/>`_.

.. |publicKey| raw:: html

    <code>Public Key</code>

Once your account is verified, you will need to gather your |publicKey| from the Clients page under your default tenant. See image below:

.. |gettingStarted| image:: https://cdn.meshydb.com/images/getting-started-client.png
           :alt: Public Key under Clients default tenant

|gettingStarted|

----------
Next Steps
----------

Now that you have your public key, you can begin with any of our language specific quick starts:

    * `C# <https://docs.meshydb.com/en/latest/intro/getting_started_csharp.html#install-sdk>`_
    * `NodeJS <https://docs.meshydb.com/en/latest/intro/getting_started_nodejs.html#install-sdk>`_
    * `REST <https://docs.meshydb.com/en/latest/intro/getting_started_rest.html#registering-anonymous-user>`_

.. toctree::
    :maxdepth: 1
    :hidden:
    :caption: Getting Started

    intro/getting_started_csharp
    intro/getting_started_nodejs
    intro/getting_started_rest
   
.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: Authorization

    authorization/generating_token
    authorization/refreshing_token

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: Users

    users/creating_users
    users/retrieving_self
    users/searching
    users/updating_self
    users/password_recovery
    users/logging_out
    users/deleting

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: Data

    data/creating
    data/retrieving
    data/updating
    data/searching
    data/deleting
    data/projecting

.. toctree::
    :maxdepth: 2
    :hidden:
    :caption: Roles

    roles/creating
    roles/retrieving
    roles/updating
    roles/searching
    roles/deleting