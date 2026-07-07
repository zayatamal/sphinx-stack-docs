.. meta::
    :description: Learn how to update your Sphinx Stack with the latest features and improvements.

.. _update-sphinx-stacks:

Update your Sphinx Stack
========================

The Sphinx Stack is updated frequently. After you copy its code and it becomes the
documentation system in your project, you must manually maintain it. 

There are special pathways to bring in the latest features and improvements from the
Sphinx Stack repository.


Determine your current version
------------------------------

There are two versions of the Sphinx Stack that entail different update processes:

- the **new**, or **extension-based** Sphinx Stack
- the **legacy**, or **pre-extension** Sphinx Stack


New Sphinx Stack
^^^^^^^^^^^^^^^^

If your ``conf.py`` file includes ``canonical_sphinx`` under the ``extensions`` list,
you are using the new Sphinx Stack. 

To update a new Sphinx Stack project to the latest version, see:

- :ref:`update-new-sphinx-stack`

.. note::

    New Sphinx Stack releases use a semantic versioning scheme for minor and patch
    versions, which can be found in your project's ``_dev/version`` file. 


Legacy Sphinx Stack
^^^^^^^^^^^^^^^^^^^^

If your ``conf.py`` file does _not_ include ``canonical_sphinx`` you are using the
legacy Sphinx Stack. 

To update a legacy Sphinx Stack project to the latest version of the new Sphinx Stack:

- :ref:`update-legacy-sphinx-stack`

.. toctree::
   :hidden:

    New Sphinx Stack <new-sphinx-stack>
    Legacy Sphinx Stack <legacy-sphinx-stack>
