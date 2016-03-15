GIS.lab cluster is managed by decentralized cluster management tool
called `Serf <https://www.serfdom.io/intro/>`__, which is based on
gossip protocol. Serf is responsible for automatic joining and removing
machines to and from GIS.lab cluster and OWS load balancer management.
It is also used as interface for running cluster *events* and *queries*.

Roles, events and queries
-------------------------

Machines belonging to GIS.lab cluster are divided into two roles: \*
**server**: GIS.lab server \* **client**: GIS.lab clients

All machines are capable of running different set of cluster
`events <https://www.serfdom.io/docs/commands/event.html>`__ and
`queries <https://www.serfdom.io/docs/commands/query.html>`__ depending
on their *role* membership. Events and queries can be send from any
machine which is a member of GIS.lab cluster using *gislab-cluster*
client (which is currently just symlink to serf binary), or
programmatically using `RPC
mechanism <https://www.serfdom.io/docs/agent/rpc.html>`__. All machines
in cluster will receive all events and queries and will decide to
respond or not depending on existence of
`handler <https://www.serfdom.io/docs/agent/event-handlers.html>`__
responsible for particular event or query.

The main difference between *event* and *query* is that while query is
designed to send some query and receive response, the purpose of event
is just to announce that something has happend or should happen without
receiving any response. Response from query can be returned in two
formats: text or JSON.

Public events and queries
-------------------------

Here is a list of publicly available events and queries designed for
ordinary usage. This list doesn't contain system events and queries
which are used for internal GIS.lab cluster management.

Get list of cluster members, their roles membership and list of
supported events and queries

::

    $ gislab-cluster members

Get list of cluster members in JSON format

::

    $ gislab-cluster members -format json

    {
      "members": [
        {
          "name": "server.gis.lab",
          "addr": "192.168.15.5:7946",
          "port": 7946,
          "tags": {
            "role": "server"
          },
          "status": "alive",
          "protocol": {
            "max": 4,
            "min": 2,
            "version": 4
          }
        },
        {
          "name": "c51",
          "addr": "192.168.15.51:7946",
          "port": 7946,
          "tags": {
            "role": "client"
          },
          "status": "alive",
          "protocol": {
            "max": 4,
            "min": 2,
            "version": 4
          }
        },
        {
          "name": "c50",
          "addr": "192.168.15.50:7946",
          "port": 7946,
          "tags": {
            "role": "client"
          },
          "status": "alive",
          "protocol": {
            "max": 4,
            "min": 2,
            "version": 4
          }
        }
      ]
    }

Reboot/shutdown all client machines

::

    $ gislab-cluster event reboot
    $ gislab-cluster event shutdown

Reboot/shutdown particular client machines

::

    $ gislab-cluster event reboot "<hostname>,<hostname>,..."
    $ gislab-cluster event shutdown "<hostname>,<hostname>,..."

Get list of client machines which are currently running user session

::

    $ gislab-cluster members -tag sesion-active=*.

Running commands on whole cluster with parallel-ssh
---------------------------------------------------

Deploy public ssh key to your GIS.lab user to allow passwordless login

::

    $ ssh-copy-id -i ~/.ssh/id_rsa.pub  gislab@<GIS.lab server IP>

Get list of currently running client machines

::

    $ MACHINES="$(gislab-cluster members -tag role=client -status=alive | awk -F " " '{printf "%s ", $1}')"

Install gedit on all client machines

::

    $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" sudo DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends gedit

    [1] 23:02:57 [SUCCESS] c51
    Reading package lists...
    Building dependency tree...
    Reading state information...
    The following NEW packages will be installed:
      gedit
    0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
    Need to get 0 B/827 kB of archives.
    After this operation, 2,781 kB of additional disk space will be used.
    Selecting previously unselected package gedit.
    (Reading database ... 134642 files and directories currently installed.)
    Unpacking gedit (from .../gedit_3.4.1-0ubuntu1_amd64.deb) ...
    Processing triggers for desktop-file-utils ...
    Setting up gedit (3.4.1-0ubuntu1) ...
    update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode.
    Processing triggers for libc-bin ...
    ldconfig deferred processing now taking place
    [2] 23:02:57 [SUCCESS] c50
    Reading package lists...
    Building dependency tree...
    Reading state information...
    The following NEW packages will be installed:
      gedit
    0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
    Need to get 0 B/827 kB of archives.
    After this operation, 2,781 kB of additional disk space will be used.
    Selecting previously unselected package gedit.
    (Reading database ... 134642 files and directories currently installed.)
    Unpacking gedit (from .../gedit_3.4.1-0ubuntu1_amd64.deb) ...
    Processing triggers for desktop-file-utils ...
    Setting up gedit (3.4.1-0ubuntu1) ...
    update-alternatives: using /usr/bin/gedit to provide /usr/bin/gnome-text-editor (gnome-text-editor) in auto mode.
    Processing triggers for libc-bin ...
    ldconfig deferred processing now taking place

Perform performance test of parallel write to network share

::

    $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/dev/zero of=/mnt/barrel/file-$(hostname).io bs=1M count=1024'

    [1] 09:42:11 [SUCCESS] c54
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 37.7824 s, 28.4 MB/s
    [2] 09:42:11 [SUCCESS] c52
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 38.1136 s, 28.2 MB/s
    [3] 09:42:11 [SUCCESS] c51
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 38.4403 s, 27.9 MB/s
    [4] 09:42:12 [SUCCESS] c53
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 38.6802 s, 27.8 MB/s

Perform performance test of parallel read from network share

::

    $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/mnt/barrel/file-$(hostname).io of=/dev/zero bs=1M'

    [1] 09:42:45 [SUCCESS] c51
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 0.207453 s, 5.2 GB/s
    [2] 09:42:45 [SUCCESS] c53
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 0.210259 s, 5.1 GB/s
    [3] 09:42:45 [SUCCESS] c52
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 0.227793 s, 4.7 GB/s
    [4] 09:42:45 [SUCCESS] c54
    Stderr: 1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 0.207774 s, 5.2 GB/s

Perform CPU performance test

::

    $ parallel-ssh -O StrictHostKeyChecking=no -i -H "$MACHINES" 'dd if=/dev/zero bs=1M count=1024 | md5sum'

    [1] 09:39:05 [SUCCESS] c52
    cd573cfaace07e7949bc0c46028904ff  -
    Stderr: Warning: Permanently added 'c52,192.168.19.52' (ECDSA) to the list of known hosts.
    1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 2.51008 s, 428 MB/s
    [2] 09:39:05 [SUCCESS] c53
    cd573cfaace07e7949bc0c46028904ff  -
    Stderr: Warning: Permanently added 'c53,192.168.19.53' (ECDSA) to the list of known hosts.
    1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 2.50255 s, 429 MB/s
    [3] 09:39:06 [SUCCESS] c54
    cd573cfaace07e7949bc0c46028904ff  -
    Stderr: Warning: Permanently added 'c54,192.168.19.54' (ECDSA) to the list of known hosts.
    1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 2.52551 s, 425 MB/s
    [4] 09:39:06 [SUCCESS] c51
    cd573cfaace07e7949bc0c46028904ff  -
    Stderr: Warning: Permanently added 'c51,192.168.19.51' (ECDSA) to the list of known hosts.
    1024+0 records in
    1024+0 records out
    1073741824 bytes (1.1 GB) copied, 2.56706 s, 418 MB/s

Remote desktop management
-------------------------

Connecting to running remote desktop session

::

    HOST=<REMOTE-HOST-NAME> ssh gislab@$HOST "x11vnc -bg -safer -once -nopw -scale 0.9x0.9 -display :0 -allow $(hostname -f)" && vncviewer $HOST

