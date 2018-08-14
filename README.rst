.. _section-role-warden-client:

Role **warden-client**
================================================================================

Ansible role for convenient installation of the `Warden <https://warden.cesnet.cz/>`__
client library and filer.

* `Ansible Galaxy page <https://galaxy.ansible.com/honzamach/warden_client>`__
* `GitHub repository <https://github.com/honzamach/ansible-role-warden-client>`__
* `Travis CI page <https://travis-ci.org/honzamach/ansible-role-warden-client>`__


Description
--------------------------------------------------------------------------------

This role is responsible for provisioning of Warden-client library on target host.

.. note::

    This role supports the :ref:`template customization <section-overview-customize-templates>` feature.


Requirements
--------------------------------------------------------------------------------

This role does not have any special requirements.


Dependencies
--------------------------------------------------------------------------------

This role is dependent on following roles:

* :ref:`certified <section-role-certified>` (soft)
* :ref:`monitored <section-role-certified>` (soft)

No other roles have direct dependency on this role.


Managed files
--------------------------------------------------------------------------------

This role directly manages content of following files on target system:

* ``/etc/default/warden_filer_receiver``
* ``/etc/default/warden_filer_sender``
* ``/etc/nagios/nrpe.d/warden-client.cfg``
* ``/opt/system-status/system-status.d/30-warden-client``
* ``/etc/warden_filer.cfg``


Role variables
--------------------------------------------------------------------------------

There are following internal role variables defined in ``defaults/main.yml`` file,
that can be overriden and adjusted as needed:

.. envvar:: hm_warden_client__node_name:

    Name of the node running Warden client.

    * *Datatype:* ``string``
    * *Default value:* (undefined)

.. envvar:: hm_warden_client__daemon_uid:

    Numerical UID of the user account under which to run Warden filer.

    * *Datatype:* ``integer``
    * *Default value:* (undefined)

.. envvar:: hm_warden_client__daemon_gid:

    Numerical GID of the group account under which to run Warden filer.

    * *Datatype:* ``integer``
    * *Default value:* (undefined)

.. envvar:: hm_warden_client__server_url:

    URL of the Warden server to which send or from which to receive IDEA messages.

    * *Datatype:* ``string``
    * *Default value:* (undefined)

.. envvar:: hm_warden_client__sender_enabled

    Enable receiving warden_filer.

    * *Datatype:* ``bool``
    * *Default value:* ``true``

.. envvar:: hm_warden_client__receiver_enabled

    Enable receiving warden_filer.

    * *Datatype:* ``bool``
    * *Default value:* ``true``

.. envvar:: hm_warden_client__sender_queue

    Queue directory for sending warden_filer.

    * *Datatype:* ``directory``
    * *Default value:* ``/var/warden/sender/queue``

.. envvar:: hm_warden_client__receiver_queue

    Queue directory for receiving warden_filer.

    * *Datatype:* ``directory``
    * *Default value:* ``/var/mentat/spool/_inspector``

Additionally this role makes use of following built-in Ansible variables:

.. envvar:: ansible_lsb['codename']

    Debian distribution codename is used for :ref:`template customization <section-overview-customize-templates>`
    feature.

.. envvar:: group_names

    See section *Group memberships* below for details.


Foreign variables
--------------------------------------------------------------------------------

This role makes use of following foreign variables, that are defined within other
roles:

:envvar:`hm_certified__cert_host_dir`

    Warden filer will be configured to use custom certificates.

:envvar:`rf_monitored__plugins_dir`

    Custom monitoring commands will be enabled, so the path to the plugin directory is needed.


Group memberships
--------------------------------------------------------------------------------

* **servers-monitored**

  In case the target server is member of this group Nagios monitoring is automagically
  configured for the Syslog-ng system.

* **servers-commonenv**

  In case the target server is member of this group system status script is automagically
  configured for the Syslog-ng system.


Usage and customization
--------------------------------------------------------------------------------

This role is (attempted to be) written according to the `Ansible best practices <https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html>`__. The default implementation should fit most users,
however you may customize it by tweaking default variables and providing custom
templates.


Variable customizations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Most of the usefull variables are defined in ``defaults/main.yml`` file, so they
can be easily overridden almost from `anywhere <https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#variable-precedence-where-should-i-put-a-variable>`__.


Template customizations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This roles uses *with_first_found* mechanism for all of its templates. If you do
not like anything about built-in template files you may provide your own custom
templates. For now please see the role tasks for list of all checked paths for
each of the template files.


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


Example Playbook
--------------------------------------------------------------------------------

Example content of inventory file ``inventory``::

    [servers-warden-client]
    localhost

Example content of role playbook file ``playbook.yml``::

    - hosts: servers-warden-client
      remote_user: root
      roles:
        - role: honzamach.warden_client
      tags:
        - role-warden_client

Example usage::

    ansible-playbook -i inventory playbook.yml


License
--------------------------------------------------------------------------------

MIT


Author Information
--------------------------------------------------------------------------------

Jan Mach <jan.mach@cesnet.cz>, CESNET, a.l.e.
