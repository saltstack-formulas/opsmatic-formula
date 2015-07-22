==========================
SaltStack Opsmatic Formula
==========================

.. note::

    See the full `Salt Formulas installation and usage instructions
    <http://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html>`_.

Available states
================

.. contents::
       :local:

``opsmatic``
------------

Installs the Opsmatic agent and configures the service. Based upon pillar data, the minion is added to multiple groups. 
For more information regarding the Opsmatic agent go to https://www.opsmatic.com.



Usage
=====
On salt minion, run the following::

  salt-call state.sls opsmatic

Or to install the agent on all minions, run on the master::

  salt '*' state.sls opsmatic

We're locking the version of opsmatic to ``1.0.241-1`` here. You can set ``version: latest`` in ``install-opsmatic-agent`` state if you wish.


Opsmatic registration key
-------------------------

Update ``opsmatic-agent.conf`` file with your key which you received from Opsmatic during registration.

Pillar data
-----------

Set up your pillar data, so your minions can be associated with multiple groups if necessary. The ``pillar.example`` file below specifies that minion ``app01`` belongs to both the ``sfo`` group as well as the ``us`` group::


  opsmatic.groups:
  {% if  grains['id'] == "app01.mydomain.com" %}
    - sfo 
    - us 
  {% elif  grains['id'] == "db01.mydomain.com" %}
    - sjc 
    - us 
  {% elif  grains['id'] == "app02.mydomain.com" %}
    - boston 
    - us 
  {% endif %}


This will result in a ``opsmatic-agent.conf`` on host ``app01`` with the following contents::

  host_alias = "app01"
  groups = "sfo, us"
