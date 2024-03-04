# Nginx-Notes
# Setup Nginx HA

1. On all the instances 
`apt isntall nginx-ha-keepalived`

2. Setup HA IPs
Example: Node1: 10.201.201.176; Node2: 10.201.201.177; Floating: 10.201.201.178
----------------

Run the command `nginx-ha-setup`, it will pop out below

> Thank you for using NGINX Plus!

> This script is intended for use with RHEL/CentOS/SLES/Debian/Ubuntu-based systems.
> It will configure highly available NGINX Plus environment in Active/Passive pair.

> NOTE: you will need the following in order to continue:
>  - 2 running systems (nodes) with static IP addresses
>  - one free IP address to use as Cluster IP endpoint

> It is strongly recommended to run this script simultaneously on both nodes,
> e.g. use two terminal windows and switch between them step by step.

> It is recommended to run this script under screen(1) in order to allow
> installation process to continue in case of unexpected session disconnect.

> Press <Enter> to continue...

> Step 1: configuring internal management IP addresses.

> In order to communicate with each other, both nodes must have at least one IP address.

> The guessed primary IP of this node is: 10.201.201.177/16

> Do you want to use this address for internal cluster communication? (y/n)
> IP address of this host is set to: 10.201.201.177/16
> Primary network interface: ens192

> Now please enter IP address of a second node: 10.201.201.176
> You entered: 10.201.201.176
> Is it correct? (y/n)
> IP address of the second node is set to: 10.201.201.176

> Press <Enter> to continue...

> Step 2: creating keepalived configuration

> Now you have to choose cluster IP address.
> This address will be used as en entry point to all your cluster resources.
> The chosen address must not be one already associated with a physical node.

> Enter cluster IP address: 10.201.201.178
> You entered: 10.201.201.178
> Is it correct? (y/n)

> You must choose which node should have the MASTER role in this cluster.

> Please choose what the current node role is:
> 1) MASTER
> 2) BACKUP

> (on the second node you should choose the opposite variant)

> Press 1 or 2.
> This is the BACKUP node.

> Step 3: starting keepalived

> Starting keepalived...
> keepalived has been successfully started.

> Press <Enter> to continue...

> Step 4: configuring cluster

> Enabling keepalived and nginx at boot time...
> Initial configuration complete!

> keepalived logs are written to syslog and located here:
> /var/log/syslog

> Further configuration may be required according to your needs
> and environment.
> Main configuration file for keepalived can be found at:
>  /etc/keepalived/keepalived.conf

> To control keepalived, use 'service keepalived' command:
>  service keepalived status

> keepalived documentation can be found at:
> http://www.keepalived.org/

> NGINX-HA-keepalived documentation can be found at:
> /usr/share/doc/nginx-ha-keepalived/README

> Thank you for using NGINX Plus!
----------------

3. Check HA configuration
`cat /etc/keepalived/keepalived.conf`

4. Check HA service
`/usr/lib/keepalived/nginx-ha-check`

5. Check instance status (Active or Passive)
`cat /var/run/nginx-ha-keepalived.state`

6. Failover, run the command on Primary instance 
`touch /var/run/keepalived-manual-failover`

7. Sync 
https://docs.nginx.com/nginx/admin-guide/high-availability/configuration-sharing/#install-nginx-sync-on-the-primary-machine
