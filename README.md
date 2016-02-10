Interfaces Role for EOS
=======================

The arista.eos-interfaces role creates an abstraction for EOS interface configuration.
This means that you do not need to write any ansible tasks. Simply create
an object that matches the requirements below and this role will ingest that
object and perform the necessary configuration.

This role is used to configure the basics of an interface, for example it will
simply create the interface, enable or disable it and set the description. It will
do this for any valid interface in EOS. This role would be used prior to setting
an IP Address on an interface or before setting more advanced Port-Channel
configuration.

Installation
------------

```
ansible-galaxy install arista.eos-interfaces
```


Requirements
------------

Requires an SSH connection for connectivity to your Arista device. You can use
any of the built-in eos connection variables, or the convenience ``provider``
dictionary.

Role Variables
--------------

The tasks in this role are driven by the ``interfaces`` object described below:

**interfaces** (list) each entry contains the following keys:

|         Key | Type                      | Notes                                                                                                                                                                                                |
|------------:|---------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        name | string (required)         | The unique interface identifier name. The interface name must use the full interface name (no abbreviated names). For example, interfaces should be specified as Ethernet1 not Et1                   |
| description | string                    | Configures a one lne ASCII description for the interface.                                                                                                                                            |
|      enable | boolean: true*, false     | Configures the administrative state for the interface. Setting the value to true will adminstrative enable the interface and setting the value to false will administratively disable the interface. |
|       state | choices: present*, absent | Set the state for the interface configuration.                                                                                                                                                       |


```
Note: Asterisk (*) denotes the default value if none specified
```


Dependencies
------------

The eos-interfaces role is built on modules included in the core Ansible code.
These modules were added in ansible version 2.1

- Ansible 2.1.0

Example Playbook
----------------

The following example will use the arista.eos-interfaces role to setup
a few interfaces without writing any tasks. We'll create a
``hosts`` file with our switch, then a corresponding ``host_vars`` file and
then a simple playbook which only references the eos-interfaces role.
By including the role, we automatically get access to all of the tasks
to configure interfaces. What's nice about this is that if you have a
host without interface configuration, the tasks will be skipped without any issue.


Sample hosts file:

    [leafs]
    leaf1.example.com

Sample host_vars/leaf1.example.com

    interfaces:
      - name: Ethernet1
        description: Link to Peer1
      - name: Ethernet2
        description: Link to Peer1
      - name: Loopback1000
        description: Loopback for BGP
      - name: Port-Channel50
        description: Peer Channel-group


A simple playbook to configure BGP, leaf.yml

    - hosts: leafs
      roles:
         - arista.eos-interfaces

Then run with:

    ansible-playbook -i hosts leaf.yml

License
-------

Copyright (c) 2015, Arista Networks EOS+
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice, this
  list of conditions and the following disclaimer.

* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.

* Neither the name of Arista nor the names of its
  contributors may be used to endorse or promote products derived from
  this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY,
OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

Author Information
------------------

Please raise any issues using our GitHub repo or email us at ansible-dev@arista.com

[quickstart]: http://ansible-eos.readthedocs.org/en/latest/quickstart.html