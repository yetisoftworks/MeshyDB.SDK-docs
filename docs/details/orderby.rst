--------
Order By
--------

Ordering is supported in a MongoDB format. This format is as an object with a -1 or 1 to identify descending or ascending format respectively.

The following example shows how to sort an object by Name in descending order.

.. code-block:: json

    { "Name": -1 }

The C# implementation allows an alternative for constructing the object.

The following example is the equivalent in C#.

.. code-block:: c#

    OrderByDefinition<Person>.OrderByDescending("Name");

    // Or

    OrderByDefinition<Person>.OrderByDescending(x => x.Name);

To add additional filters it can be extended as follows.

This example will order by Name descending then Age ascending.

.. code-block:: c#

    OrderByDefinition<Person>.OrderByDescending("Name").ThenBy("Age");

    // Or

    OrderByDefinition<Person>.OrderByDescending(x => x.Name).ThenBy(x=> x.Age);
