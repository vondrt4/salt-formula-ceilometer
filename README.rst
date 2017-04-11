
==================
Ceilometer Formula
==================

The ceilometer project aims to deliver a unique point of contact for billing
systems to acquire all of the measurements they need to establish customer
billing, across all current OpenStack core components with work underway to
support future OpenStack components.

Sample Pillars
==============

Ceilometer API/controller node

.. code-block:: yaml

    ceilometer:
      server:
        enabled: true
        version: havana
        cluster: true
        secret: pwd
        bind:
          host: 127.0.0.1
          port: 8777
        identity:
          engine: keystone
          host: 127.0.0.1
          port: 35357
          tenant: service
          user: ceilometer
          password: pwd
        message_queue:
          engine: rabbitmq
          host: 127.0.0.1
          port: 5672
          user: openstack
          password: pwd
          virtual_host: '/openstack'
        database:
          engine: mongodb
          host: 127.0.0.1
          port: 27017
          name: ceilometer
          user: ceilometer
          password: pwd

Client-side RabbitMQ HA setup

.. code-block:: yaml

    ceilometer:
      server:
        ....
        message_queue:
          engine: rabbitmq
          members:
          - host: 127.0.0.1
          - host: 127.0.0.1
          - host: 127.0.0.1
          user: openstack
          password: pwd
          virtual_host: '/openstack'
       ....


Ceilometer Graphite publisher

.. code-block:: yaml

    ceilometer:
      server:
        enabled: true
        publisher:
          graphite:
            enabled: true
            host: 10.0.0.1
            port: 2003

Ceilometer compute agent

.. code-block:: yaml

    ceilometer:
      agent:
        enabled: true
        version: havana
        secret: pwd
        identity:
          engine: keystone
          host: 127.0.0.1
          port: 35357
          tenant: service
          user: ceilometer
          password: pwd
        message_queue:
          engine: rabbitmq
          host: 127.0.0.1
          port: 5672
          user: openstack
          password: pwd
          virtual_host: '/openstack'
          rabbit_ha_queues: true

More Information
================

* https://wiki.openstack.org/wiki/Ceilometer
* http://docs.openstack.org/developer/ceilometer/install/manual.html
* http://docs.openstack.org/developer/ceilometer/
* https://fedoraproject.org/wiki/QA:Testcase_OpenStack_ceilometer_install
* https://github.com/spilgames/ceilometer_graphite_publisher
* http://engineering.spilgames.com/using-ceilometer-graphite/


Documentation and Bugs
======================

To learn how to install and update salt-formulas, consult the documentation
available online at:

    http://salt-formulas.readthedocs.io/

In the unfortunate event that bugs are discovered, they should be reported to
the appropriate issue tracker. Use Github issue tracker for specific salt
formula:

    https://github.com/salt-formulas/salt-formula-ceilometer/issues

For feature requests, bug reports or blueprints affecting entire ecosystem,
use Launchpad salt-formulas project:

    https://launchpad.net/salt-formulas

You can also join salt-formulas-users team and subscribe to mailing list:

    https://launchpad.net/~salt-formulas-users

Developers wishing to work on the salt-formulas projects should always base
their work on master branch and submit pull request against specific formula.

    https://github.com/salt-formulas/salt-formula-ceilometer

Any questions or feedback is always welcome so feel free to join our IRC
channel:

    #salt-formulas @ irc.freenode.net
