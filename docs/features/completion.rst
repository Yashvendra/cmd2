Completion
==========

``cmd2`` adds tab-completion of file system paths for all built-in commands
where it makes sense, including:

- ``edit``
- ``run_pyscript``
- ``run_script``
- ``shell``

``cmd2`` also adds tab-completion of shell commands to the ``shell`` command.

Additionally, it is trivial to add identical file system path completion to
your own custom commands.  Suppose you have defined a custom command ``foo`` by
implementing the ``do_foo`` method.  To enable path completion for the ``foo``
command, then add a line of code similar to the following to your class which
inherits from ``cmd2.Cmd``::

    complete_foo = self.path_complete

This will effectively define the ``complete_foo`` readline completer method in
your class and make it utilize the same path completion logic as the built-in
commands.

The built-in logic allows for a few more advanced path completion capabilities,
such as cases where you only want to match directories.  Suppose you have a
custom command ``bar`` implemented by the ``do_bar`` method.  You can enable
path completion of directories only for this command by adding a line of code
similar to the following to your class which inherits from ``cmd2.Cmd``::

    # Make sure you have an "import functools" somewhere at the top
    complete_bar = functools.partialmethod(cmd2.Cmd.path_complete, path_filter=os.path.isdir)


Tab Completion Using Argparse Decorators
----------------------------------------

When using one the Argparse-based :ref:`decorators`, ``cmd2`` provides
automatic tab-completion of flag names.

Tab-completion of argument values can be configured by using one of five
parameters to ``argparse.ArgumentParser.add_argument()``

- ``choices``
- ``choices_function`` / ``choices_method``
- ``completer_function`` / ``completer_method``

See the arg_decorators_ or colors_ example for a demonstration of how to
use the ``choices`` parameter. See the tab_autocompletion_ example for a
demonstration of how to use the ``choices_function`` and ``choices_method``
parameters. See the arg_decorators_ or tab_autocompletion_ example for a
demonstration of how to use the ``completer_method`` parameter.

When tab-completing flags and/or argument values for a ``cmd2`` command using
one of these decorators, ``cmd2`` keeps track of state so that once a flag has
already previously been provided, it won't attempt to tab-complete it again.
When no completion results exists, a hint for the current argument will be
displayed to help the user.

.. _arg_decorators: https://github.com/python-cmd2/cmd2/blob/master/examples/arg_decorators.py
.. _colors: https://github.com/python-cmd2/cmd2/blob/master/examples/colors.py
.. _tab_autocompletion: https://github.com/python-cmd2/cmd2/blob/master/examples/tab_autocompletion..py


CompletionItem For Providing Extra Context
------------------------------------------

When tab-completing things like a unique ID from a database, it can often be
beneficial to provide the user with some extra context about the item being
completed, such as a description.  To facilitate this, ``cmd2`` defines the
``CompletionItem`` class which can be returned from any of the 4 completion
functions: ``choices_function``, ``choices_method``, ``completion_function``,
or ``completion_method``.

.. autoclass:: cmd2.argparse_custom.CompletionItem
    :members:

See the tab_autocompletion_ example or the implementation of the built-in
**set** command for demonstration of how this is used.
