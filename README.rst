
==================
Ceilometer Formula
==================

The ceilometer project aims to deliver a unique point of contact for billing
systems to acquire all of the measurements they need to establish customer
billing, across all current OpenStack components with work underway to
support future OpenStack components.
This formula provides different backends for Ceilometer data: MongoDB, InfluxDB. Also,
Graphite and direct (to Elasticsearch) publishers are available. If InfluxDB is used
as a backend, heka is configured to consume messages from RabbitMQ and write in to
InfluxDB, i.e. ceilometer collector service is not used in this configuration.

Sample Pillars
==============

Ceilometer API/controller node

.. code-block:: yaml

    ceilometer:
      server:
        enabled: true
        version: mitaka
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

Configuration of policy.json file

.. code-block:: yaml

    ceilometer:
      server:
        ....
        policy:
          segregation: 'rule:context_is_admin'
          # Add key without value to remove line from policy.json
          'telemetry:get_resource':

Databases configuration

MongoDB example:

.. code-block:: yaml

    ceilometer:
      server:
        database:
          engine: mongodb
          members:
          - host: 10.0.106.10
            port: 27017
          - host: 10.0.106.20
            port: 27017
          - host: 10.0.106.30
            port: 27017
          name: ceilometer
          user: ceilometer
          password: password

InfluxDB/Elasticsearch example:

.. code-block:: yaml

    ceilometer:
      server:
        database:
          influxdb:
            host: 10.0.106.10
            port: 8086
            user: ceilometer
            password: password
            database: ceilometer
          elasticsearch:
            enabled: true
            host: 10.0.106.10
            port: 9200

Client-side RabbitMQ HA setup

.. code-block:: yaml

    ceilometer:
      server:
        ....
        message_queue:
          engine: rabbitmq
          members:
          - host: 10.0.106.10
          - host: 10.0.106.20
          - host: 10.0.106.30
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
        version: mitaka
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
