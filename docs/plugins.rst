.. _plugins:

=========
 Plugins
=========

``sqlite-utils`` supports plugins, which can be used to add extra features to the software.

Plugins can add new commands, for example ``sqlite-utils some-command ...``

Plugins can be installed using the ``sqlite-utils install`` command:

.. code-block:: bash

    sqlite-utils install sqlite-utils-name-of-plugin

You can see a JSON list of plugins that have been installed by running this:

.. code-block:: bash

    sqlite-utils plugins

.. _plugins_building:

Building a plugin
-----------------

Plugins are created in a directory named after the plugin. To create a "hello world" plugin, first create a ``hello-world`` directory:

.. code-block:: bash

    mkdir hello-world
    cd hello-world

In that folder create two files. The first is a ``pyproject.toml`` file describing the plugin:

.. code-block:: toml

    [project]
    name = "sqlite-utils-hello-world"
    version = "0.1"

    [project.entry-points.sqlite_utils]
    hello_world = "sqlite_utils_hello_world"

The ```[project.entry-points.sqlite_utils]`` section tells ``sqlite-tils`` which module to load when executing the plugin.

Then create ``sqlite_utils_hello_world.py`` with the following content:

.. code-block:: python

    import click
    import sqlite_utils

    @sqlite_utils.hookimpl
    def register_commands(cli):
        @cli.command()
        def hello_world():
            "Say hello world"
            click.echo("Hello world!")

Install the plugin in "editable" mode - so you can make changes to the code and have them picked up instantly by ``sqlite-utils`` - like this:

.. code-block:: bash

    sqlite-utils install -e .

Or pass the path to your plugin directory:

.. code-block:: bash

    sqlite-utils install -e `/dev/sqlite-utils-hello-world

Now, running this should execute your new command:

.. code-block:: bash

    sqlite-utils hello-world

Your command will also be listed in the output of ``sqlite-utils --help``.

.. _plugins_hooks:

Plugin hooks
------------

Plugin hooks allow ``sqlite-utils`` to be customized. There is currently one hook.

.. _plugins_hooks_register_commands:

register_commands(cli)
~~~~~~~~~~~~~~~~~~~~~~

This hook can be used to register additional commands with the ``sqlite-utils`` CLI. It is called with the ``cli`` object, which is a ``click.Group`` instance.

Example implementation:

.. code-block:: python

    import click
    import sqlite_utils

    @sqlite_utils.hookimpl
    def register_commands(cli):
        @cli.command()
        def hello_world():
            "Say hello world"
            click.echo("Hello world!")

