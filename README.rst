pfsense-scripts
===============

Misc pfSense helper scripts.


Installation
------------

The Right Way to install these scripts to pfSense machine is to upload them
through "Filer" package to ``/usr/local/etc/rc.d`` directory, with executable
bit and retaining ``.sh`` extension.

"Filer" package can be installed from WebUI's "System - Packages" menu.

After that, individual scripts should be uploaded through "Diagnostics - Filer"
WebUI form, with the following fields set:

* ``File: /usr/local/etc/<script_name>.sh``
* ``Permissions: 0755``
* ``File Contents: <script_contents>``
* ``Script/Command: /usr/local/etc/<script_name>.sh <script_args>``
* ``Description`` field can be filled with anything you like.

Substitute <scriptname> and <script_contents> above with name/contents for the
specific script and <script_args> with any parameters that it expects on the
command line (see script descriptions below for these).

Scripts that are configurable can also have values that can be changed when
copying script contents, usually at the very top.


Scripts
-------

pfsense_iface_state.sh
``````````````````````

Script to detect when interface goes down and restart it::

  Usage: pfsense_iface_state.sh interface_label [check_interval]

Command-line arguments:

* interface_label: interface name, same as it is set/shown in pfSense WebUI.

  For example, default wan/lan interface names in pfSense are WAN and LAN.
  Upper/lower case matters.

* check_interval (optional): interval between iface state checks, in seconds.

  Default: 60

Example Filer's "Script/Command" setting (for WAN interface)::

  /usr/local/etc/pfsense_iface_state.sh WAN

Runs an endless loop, checking specified interface state every N seconds and
restarting it if check fails.

Restart is done via ``interface_reconfigure($iface)`` from php, which cycles
iface state down/up.