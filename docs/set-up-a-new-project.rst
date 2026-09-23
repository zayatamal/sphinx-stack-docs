.. meta::
   :description: How to add the Sphinx Stack to a new or existing project, build the documentation locally, and set up automatic documentation checks.

.. _set-up-a-new-project:


Set up a new project
====================

This guide shows you how to add the Sphinx Stack to a new or existing project. You'll
configure it, build the documentation locally, and set up automatic documentation
checks.


Before you start
----------------

Decide where the documentation will live. For most projects, keep it in a ``docs``
directory in the project repository. This lets contributors review documentation
alongside related project changes. It is the default layout used by the Sphinx Stack.

Consider a dedicated documentation repository if the documentation covers multiple
repositories, has a separate release process, or needs different ownership or access
controls.

Both approaches use the same Sphinx Stack directory structure and build commands.


.. _initial-setup:

Add the Sphinx Stack
--------------------

Choose the setup that matches your project.


Create a new repository from the template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. In the `Sphinx Stack repository <https://github.com/canonical/sphinx-stack>`__,
   select **Use this template** > **Create a new repository** and fill in the
   `form <https://github.com/new?template_name=sphinx-stack&template_owner=canonical>`__.
2. Choose an owner. If you're creating documentation for a Canonical project, set the
   repository owner to **canonical**.
3. Enter a repository name, choose its visibility, add a description if needed, and
   create the repository.

The new repository includes the documentation source, build configuration, GitHub
workflows, and Read the Docs configuration.


Add the Sphinx Stack to an existing repository
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Copy these paths from the `Sphinx Stack repository
<https://github.com/canonical/sphinx-stack>`__ into the root of your project's
repository:

- the ``docs`` directory
- ``.readthedocs.yaml``, which configures builds on Read the Docs
- the workflow files in the ``.github/workflows`` directory

If your project already has a ``.github/workflows`` directory, add the relevant
documentation workflow files to it rather than replacing the existing directory.


.. _review-template-files:

Review the template files
-------------------------

Before committing your setup, review the files you copied or received from the template:

- If you created a repository from the template, remove the files that can't be reused,
  such as ``CONTRIBUTING.md`` and ``.github/CODEOWNERS``.
- Remove ``.github/workflows/test-sphinx-stack.yml``. This workflow tests the Sphinx
  Stack itself and can't be reused by your project.
- Review the remaining workflows in ``.github/workflows/``, remove any that duplicate
  checks your project already runs, and keep the ones you need. In particular:

  - ``cla-check.yml`` verifies whether contributors have signed the `Canonical License
    Agreement <https://canonical.com/legal/contributors>`__. All Canonical projects
    require this check, so if you're adding docs to an existing Canonical project that
    already has it, remove this workflow.
  - ``sphinx-python-dependency-build-checks.yml`` verifies Python dependencies for the
    documentation system. If your project has its own dependency checks, remove this
    workflow.
  - ``markdown-style-checks.yml`` runs the built-in Markdown linter. If your project
    already validates its Markdown files, remove this workflow.


Configure your project
----------------------

Open ``docs/conf.py`` and replace the project-specific values marked with ``TODO``, such
as the project name, author, and repository information.

You can leave optional settings at their default values and change them later. See
:ref:`configure-your-project` for details about the available settings.


Build and preview the documentation
-----------------------------------

Building the documentation requires ``make``, ``python3``, ``python3-venv``,
``python3-pip``.

From the ``docs`` directory, run:

.. code-block:: bash

    make run

This creates a virtual environment in ``docs/.venv``, installs the documentation
dependencies, builds the documentation, and starts a local preview server.

Open :literalref:`http://127.0.0.1:8000/` and confirm that the home page loads and
displays the project information you configured.

The server rebuilds the documentation when you change source files or ``docs/conf.py``.

The documentation build is self-contained, so you do not need to integrate it with your
project's build system during initial setup.

If you later want the project build to also build the documentation, see
:ref:`explanation-parent-project-build` for how the two builds can coexist, and
:ref:`bridge-project-and-docs-builds` for the steps to do it.


Add your content
----------------

The home page is ``docs/index.rst``. The template also provides directories for
tutorials, how-to guides, reference material, and explanations under ``docs/``.

The navigation menu structure is set by ``.. toctree::`` directives. These directives
define the hierarchy of included content throughout the documentation. The
``index.rst`` page's ``toctree`` block contains the top level navigation, which by
default is the `Diátaxis <https://diataxis.fr/>`__ documentation structure.

To add a new page to the documentation, for example a **Reference** page about your
project's settings:

1. Create a new file under the ``docs/reference/`` directory called ``settings.rst``
   with the following heading:

   .. code-block:: rest
      :caption: reStructuredText title example

      Settings
      ========

   If you prefer to use Markdown (MyST) syntax instead of |RST|, you can create the
   equivalent ``settings.md`` file and add the following Markdown-formatted heading at
   the beginning:

   .. code-block:: markdown
      :caption: Markdown title example

      # Settings

2. Add the new page to the navigation menu: open the ``docs/reference/index.rst`` file
   or another file where you want to nest the new page; at the bottom of the file, add
   the following ``toctree`` directive:

   .. code-block::

      .. toctree::
         :hidden:
         :maxdepth: 2

         settings

The page will appear in the navigation after the documentation rebuilds. For more on the
supported syntax, see :ref:`rst-syntax` and :ref:`myst-syntax`.

By default, the page's title (the first heading in the file) is shown in the global
navigation. You can override the name of a menu entry by specifying it explicitly in the
``toctree`` block (e.g., ``Reference </reference/index>``).


Review the automatic checks
---------------------------

The Sphinx Stack includes GitHub workflows for documentation checks, including spelling,
links, and inclusive language. Review the workflows' triggers and paths for your
repository.

If you move the documentation from ``docs/``, update the relevant paths and working
directories. See :ref:`github-workflows` for the available workflows, or
:ref:`run the checks locally <run-documentation-checks>`.


Configure pre-commit hooks (optional)
-------------------------------------

Optional `pre-commit <https://pre-commit.com/>`__ hooks run documentation checks when
you create a commit, so you can catch issues before opening a pull request.

The Sphinx Stack includes a ready-to-use ``.pre-commit-config.yaml`` file under
``docs/_dev/``:

.. literalinclude:: /_dev/.pre-commit-config.yaml
   :language: yaml

If your repository does not already use pre-commit, copy this file to the repository
root as ``.pre-commit-config.yaml``. If it does, add the documentation hooks to its
existing configuration.

To apply the configuration, install the Sphinx Stack hooks, for instance:

.. code-block:: bash

    pre-commit install --config docs/_dev/.pre-commit-config.yaml

After that, you should see the checks running with every commit:

.. terminal::

    git commit -m 'add spelling errors'

    Run make spelling.......................................................Failed
    Run make linkcheck......................................................Passed
    Run make woke...........................................................Passed


Next steps
----------

Before publishing the documentation:

- Replace the template pages with content for your project.
- Review the remaining settings in :ref:`configure-your-project`.
- Run the :ref:`documentation checks <run-documentation-checks>` and confirm that the
  GitHub workflows you kept behave as expected.
- Follow :ref:`publish-on-rtd`.

After the initial setup, you can:

- Use :ref:`bridge-project-and-docs-builds` to integrate the documentation and project
  builds.
- Explore :ref:`optional-customisation` for features like diagrams, API references, and
  custom templates.
- Follow :ref:`update-sphinx-stacks` to keep your project's Sphinx Stack up to date.
