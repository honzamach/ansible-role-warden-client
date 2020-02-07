.. _section-role-warden-client:

Role **warden_client**
================================================================================

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/warden_client>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-warden-client>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-warden-client>`__

This role is responsible for installation and configuration of `Warden <https://warden.cesnet.cz/>`__
client library on target host. Client library is installed from official Git repository,
which is currently probably the best option, mainly because it provides easy
way to uprgade the software.

Role is designed to enable administrators configure multiple instances of the
**warden_filer** utility. Each configuration file may contain separate configurations
for sending and receiving filer with common server connection and logging options.
You may however configure additional instance with different connection parameters
to enable communication with another Warden server.

**Table of Contents:**

* :ref:`section-role-warden-client-installation`
* :ref:`section-role-warden-client-dependencies`
* :ref:`section-role-warden-client-usage`
* :ref:`section-role-warden-client-variables`
* :ref:`section-role-warden-client-filercfgs`
* :ref:`section-role-warden-client-files`
* :ref:`section-role-warden-client-author`

This role is part of the `MSMS <https://github.com/honzamach/msms>`__ package.
Some common features are documented in its :ref:`manual <section-manual>`.


.. _section-role-warden-client-installation:

Installation
--------------------------------------------------------------------------------

To install the role `honzamach.warden_client <https://galaxy.ansible.com/honzamach/warden_client>`__
from `Ansible Galaxy <https://galaxy.ansible.com/>`__ please use variation of
following command::

    ansible-galaxy install honzamach.warden_client

To install the role directly from `GitHub <https://github.com>`__ by cloning the
`ansible-role-warden-client <https://github.com/honzamach/ansible-role-warden-client>`__
repository please use variation of following command::

    git clone https://github.com/honzamach/ansible-role-warden-client.git honzamach.warden_client

Currently the advantage of using direct Git cloning is the ability to easily update
the role when new version comes out.


.. _section-role-warden-client-dependencies:

Dependencies
--------------------------------------------------------------------------------

This role is dependent on following roles:

* :ref:`monitored <section-role-monitored>` (soft)

No other roles have direct dependency on this role.


.. _section-role-warden-client-usage:

Usage
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers_warden_client]
    your-server

Example content of role playbook file ``role_playbook.yml``::

    - hosts: servers_warden_client
      remote_user: root
      roles:
        - role: honzamach.warden_client
      tags:
        - role-warden_client

Example usage::

    # Run everything:
    ansible-playbook --ask-vault-pass --inventory inventory role_playbook.yml


.. _section-role-warden-client-variables:

Configuration variables
--------------------------------------------------------------------------------


Internal role variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: hm_warden_client__repo_url

    Default URL of the Git repository from which to install Warden client.

    * *Datatype:* ``string``
    * *Default:* ``"https://homeproj.cesnet.cz/git/warden.git/"``

.. envvar:: hm_warden_client__install_path

    Installation path on target hosts, without trailing slash.

    * *Datatype:* ``string``
    * *Default:* ``"/opt/warden3"``

.. envvar:: hm_warden_client__config_path

    Path to configuration directory on target hosts, without trailing slash.

    * *Datatype:* ``string``
    * *Default:* ``"/etc/warden_client"``

.. envvar:: hm_warden_client__run_path

    Path to run directory on target hosts, without trailing slash.

    * *Datatype:* ``string``
    * *Default:* ``"/var/run/warden_client"``

.. envvar:: hm_warden_client__lib_path

    Path to lib directory on target hosts, without trailing slash.

    * *Datatype:* ``string``
    * *Default:* ``"/var/lib/warden_client"``

.. envvar:: hm_warden_client__manage_services

    Enable service management.

    * *Datatype:* ``bool``
    * *Default:* ``false``

.. envvar:: hm_warden_client__server_url

    Default URL of the Warden server to which send or from which to receive IDEA messages.
    May be overridden with ``hm_warden_client__filers.#.server_url`` for particular instance of warden_filer.

    * *Datatype:* ``string``
    * *Default:* ``"https://warden-hub.cesnet.cz/warden3"``

.. envvar:: hm_warden_client__daemon_uid

    Default user account under which to run warden_filer.
    May be overridden with ``hm_warden_client__filers.#.daemon_uid`` for particular instance of warden_filer.

    * *Datatype:* ``integer``
    * *Default:* (undefined)

.. envvar:: hm_warden_client__daemon_gid

    Default group account under which to run warden_filer.
    May be overridden with ``hm_warden_client__filers.#.daemon_gid`` for particular instance of warden_filer.

    * *Datatype:* ``integer``
    * *Default:* (undefined)

.. envvar:: hm_warden_client__sender_queue

    Default queue directory for sending warden_filer.
    May be overridden with ``hm_warden_client__filers.#.sender_queue`` for particular instance of warden_filer.

    * *Datatype:* ``directory``
    * *Default:* ``/var/warden/sender/queue``

.. envvar:: hm_warden_client__receiver_queue

    Default queue directory for receiving warden_filer.
    May be overridden with ``hm_warden_client__filers.#.receiver_queue`` for particular instance of warden_filer.

    * *Datatype:* ``directory``
    * *Default:* ``/var/mentat/spool/mentat-inspector.py``

.. envvar:: hm_warden_client__receiver_queue_limit

    Default queue limit for receiving warden_filer.
    May be overridden with ``hm_warden_client__filers.#.receiver_queue_limit`` for particular instance of warden_filer.

    * *Datatype:* ``integer``
    * *Default:* ``5000``

.. envvar:: hm_warden_client__ca_cert

    Default CA certificate for Warden server verification.
    May be overridden with ``hm_warden_client__filers.#.ca_cert`` for particular instance of warden_filer.

    * *Datatype:* ``file``
    * *Default:* ``/etc/ssl/certs/ca-certificates.crt``

.. envvar:: hm_warden_client__check_queue_size

    Monitoring configuration setting for checking queue size in the *incoming* directory.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 2000, 'c': 5000}``

.. envvar:: hm_warden_client__check_queue_dirs

    Monitoring configuration setting for checking queue size in other than *incoming*
    directories.

    * *Datatype:* ``dict``
    * *Default:* ``{'w': 100, 'c': 1000}``

.. envvar:: hm_warden_client__filers

    List of configurations for warden_filer instances. Please see section :ref:`section-role-warden-client-filercfgs` for more details.

    * *Datatype:* ``dict``
    * *Default:* (undefined)


Foreign variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This role makes use of following foreign variables, that are defined within other
roles:

:envvar:`hm_monitored__plugins_dir`

    Custom monitoring commands will be enabled, so the path to the plugin directory is needed.


Built-in Ansible variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. envvar:: ansible_lsb['codename']

    Debian distribution codename is used for :ref:`template customization <section-overview-role-customize-templates>`
    feature.


Group memberships
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* **servers_monitored**

  In case the target server is member of this group Nagios monitoring is automagically
  configured for the Warden client installation.

* **servers_commonenv**

  In case the target server is member of this group system status script is automagically
  configured for the Warden client installation.


.. _section-role-warden-client-filercfgs:

Configuration of warden_filer instances
--------------------------------------------------------------------------------

Following is and example of possible Warden filer configuration::

    hm_warden_client__filers:
        # Domain for this warden_filer instance. May be empty in case there is only one filer installation.
      - domain: cesnet
        # Name of this warden_filer instance for communication with Warden server.
        node_name: cz.cesnet.host.warden_filer
        # URL of the Warden server to which send or from which to receive IDEA messages.
        server_url: https://warden-hub.cesnet.cz/warden3
        # User account under which to run warden_filer.
        user: 1000
        # Group account under which to run warden_filer.
        group: 1000
        # Enable sending warden_filer.
        sender_enabled: false
        # Enable receiving warden_filer.
        receiver_enabled: false
        # Queue directory for sending warden_filer.
        sender_queue: /var/warden/sender/queue
        # Queue directory for sending warden_filer.
        receiver_queue: /var/mentat/spool/mentat-inspector.py


.. _section-role-warden-client-files:

Managed files
--------------------------------------------------------------------------------

This role directly manages content of following files on target system:

* ``/etc/warden_filer/warden_filer_{{ filer.domain }}.cfg``
* ``/etc/systemd/system/warden_filer_{{ filer.domain }}_receiver.service``
* ``/etc/systemd/system/warden_filer_{{ filer.domain }}_sender.service``
* ``/etc/default/warden_filer_{{ filer.domain }}_receiver``
* ``/etc/default/warden_filer_{{ filer.domain }}_sender``
* ``/etc/nagios/nrpe.d/warden-client.cfg``
* ``/opt/system-status/system-status.d/30-warden-client``


.. _section-role-warden-client-author:

Author and license
--------------------------------------------------------------------------------

| *Copyright:* (C) since 2019 Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
| *Author:* Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
| Use of this role is governed by the MIT license, see LICENSE file.
|
