.. _readme:

redis-formula
=============

|img_travis| |img_sr| |img_pc|

.. |img_travis| image:: https://travis-ci.com/saltstack-formulas/redis-formula.svg?branch=master
   :alt: Travis CI Build Status
   :scale: 100%
   :target: https://travis-ci.com/saltstack-formulas/redis-formula
.. |img_sr| image:: https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg
   :alt: Semantic Release
   :scale: 100%
   :target: https://github.com/semantic-release/semantic-release
.. |img_pc| image:: https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit&logoColor=white
   :alt: pre-commit
   :scale: 100%
   :target: https://github.com/pre-commit/pre-commit

Formula to install and configure redis.

.. contents:: **Table of Contents**
   :depth: 1

General notes
-------------

See the full `SaltStack Formulas installation and usage instructions
<https://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html>`_.

If you are interested in writing or contributing to formulas, please pay attention to the `Writing Formula Section
<https://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html#writing-formulas>`_.

If you want to use this formula, please pay attention to the ``FORMULA`` file and/or ``git tag``,
which contains the currently released version. This formula is versioned according to `Semantic Versioning <http://semver.org/>`_.

See `Formula Versioning Section <https://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html#versioning>`_ for more details.

Contributing to this repo
-------------------------

**Commit message formatting is significant!!**

Please see `How to contribute <https://github.com/saltstack-formulas/.github/blob/master/CONTRIBUTING.rst>`_ for more details.

pre-commit
^^^^^^^^^^

`pre-commit <https://pre-commit.com/>`_ is configured for this formula, which you may optionally use to ease the steps involved in submitting your changes.
First install  the ``pre-commit`` package manager using the appropriate `method <https://pre-commit.com/#installation>`_, then run ``bin/install-hooks`` and
now ``pre-commit`` will run automatically on each ``git commit``. ::

  $ bin/install-hooks
  pre-commit installed at .git/hooks/pre-commit
  pre-commit installed at .git/hooks/commit-msg

Special notes
-------------

None.

Todo
----

* merge redis 2.4 and 2.6 templates to generic redis.conf.jinja

Available states
----------------

.. contents::
   :local:

``redis``
^^^^^^^^^

*Meta-state (This is a state that includes other states)*.

This state installs the Redis server solution (see https://redis.io)

``redis.clean``
^^^^^^^^^^^^^^^^

*Meta-state (This is a state that includes other states)*.

Stop Redis daemon and remove redis server solution.

``redis.common``
^^^^^^^^^^^^^^^^

Install redis only

``redis.server``
^^^^^^^^^^^^^^^^

Install redis and start up the service ( Ubuntu + RedHat based systems should work ).

For a list of all available options, look at: `redis/files/redis-{version}.conf.jinja` - also have a look at the pillar.example and map.jinja.

To use the new redis 3.2 template (more structured) add the following line to the pillar:

``source_path: salt://redis/files/redis-3.2-ng.conf.jinja``

For options that aren't in the template (timeout, for example in redis-2.8 template), you can use ``extra_opts``:

.. code-block:: yaml

   extra_opts:
     timeout: 10

``redis.server.clean``
^^^^^^^^^^^^^^^^^^^^^^

*Meta-state (This is a state that includes other states)*.

Stop Redis daemon and remove redis package/archive.

``redis.sentinel``
^^^^^^^^^^^^^^^^^^

Install redis sentinel. Compatible with Ubuntu 14/16 and CentOS 6/7.

Testing
-------

Linux testing is done with ``kitchen-salt``.

Requirements
^^^^^^^^^^^^

* Ruby
* Docker

.. code-block:: bash

   $ gem install bundler
   $ bundle install
   $ bin/kitchen test [platform]

Where ``[platform]`` is the platform name defined in ``kitchen.yml``,
e.g. ``debian-9-2019-2-py3``.

``bin/kitchen converge``
^^^^^^^^^^^^^^^^^^^^^^^^

Creates the docker instance and runs the ``redis`` main states, ready for testing.

``bin/kitchen verify``
^^^^^^^^^^^^^^^^^^^^^^

Runs the ``inspec`` tests on the actual instance.

``bin/kitchen destroy``
^^^^^^^^^^^^^^^^^^^^^^^

Removes the docker instance.

``bin/kitchen test``
^^^^^^^^^^^^^^^^^^^^

Runs all of the stages above in one go: i.e. ``destroy`` + ``converge`` + ``verify`` + ``destroy``.

``bin/kitchen login``
^^^^^^^^^^^^^^^^^^^^^

Gives you SSH access to the instance for manual testing.
