Writing your own config loader
==============================

Do you want to store your WSGI app config in Redis? In a
`TOML <https://github.com/toml-lang/toml>`_ file? In
`ZooKeeper <http://zookeeper.apache.org/>`_? You can do that.

A config loader requires a **filename extension** for your configuration
format and whether **individual methods** must be supported.

Filename extensions
-------------------

Montague uses filename extensions to dispatch config loaders.

If you load your configurartion from a file, this decision is straightforward.
For example, a file named ``foo.json`` would be dispatched to a JSON loader.

However, if you load your configuration from a service, then you need declare
an arbitrary filename extension. For example, information for a Redis
connection could be stored in a file named ``myfile.redis``. Or perhaps the
file doesn't even exist and you extract the connection info from the path
name.

Supporting individual methods
-----------------------------

Montague needs to be configured to support individual methods, such as
``app_config`` or ``server_config``. The default INI loader supports these
because the PasteDeploy INI format allows individual app configs to override
global variables for that specific app.

Config loaders must provide the methods listed in the
:class:`montague.interfaces.IConfigLoader` interface. It's recommended, but
not required, that they actually implement the interface using
:mod:`zope.interface`.

Montague standard format
------------------------

The actual layout of your configuration information will obviously vary from
format to format. Because of this, Montague has a standard layout you should
use when implementing the :meth:`montague.interfaces.IConfigLoader.config()`
method. You should return a dict that looks like the following.

.. code-block:: python

    {
        "globals": {},
        "application": {},
        "composite": {},
        "filter": {},
        "server": {},
    }

Of course, the dict can contain other keys as well, but the preceding keys are
those about which Montague cares.
