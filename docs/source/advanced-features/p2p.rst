
P2P
===

Implementation of p2p synchronization of different Freedomotic instances. If you turn on a light in instanceA the change is reflected in instanceB. Also moving an object changes its location in both instances.

In details

#. there is no need to duplicate a plugin installing it on many instances, expecially for hardware related plugins where there should be only one. The system is really distributed (despite the current data synch mechanism which is not perfect). If a plugin is duplicated you get load balance between the instances. As the core is always duplicated the core tasks are always load balanced between instances
#. messaging pattern: the first command will be executed by instanceA, the second by instanceB and so on in a round robin way splitting the load.

Each instance has a complete copy of the data so you can turn off the other instances when you want. For sure you'll loose the "local not replicated plugins" but the system will be online and you will not notice any "recovery delay" because each instance always uses its local data, which are silently synchronized with the others. Now we have a "state synchronization" when we'll refine the mechanism we will have a "completely replicated cache" which is far better than the current system.

.. image:: images/p2p.jpg
    :width: 650px
    :align: center
    :height: 450px
    :alt: Freedomotic P2P


Roadmap
#######
To reach this goal we made some changes

#. identified a single Freedomotic instance using an unique id and appended this id to each generated message
#. changed the messaging broker url to peer://freedomotic/INSTANCE_UUID
#. changed broker websocket and stomp endpoint to run on the first available port, otherwise it couldn't start two different Freedomotic instances on the same machine as the port would be already in use
This means the port for ws and stomp endpoint will change at each run.

Todo
####

#. implement a "fix port number" in the config.xml file to make it static when needed
#. add a SynchManager component (already developed in its first raw version)
#. get startup data from another already running Freedmotic instance

How to use
##########

Workaround for now is sharing the data folder on Dropbox and make both instances to read from the same folder to start. No problem if you run both instances from the same PC because they already share a common data folder.

NOTE: The restapi3 port is still static (9111) so the second Freedomotic instance will not be able to start this plugin. If you run 2 instances from the same system the problem is automatically solved (the second one cannot start because the port is already in use). If Freedomotic is installed on 2 Raspberry, then restapi plugin should be removed from the other instance before startup.
