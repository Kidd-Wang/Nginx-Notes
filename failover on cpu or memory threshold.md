Create a shell script to check the usage of the CPU, memory etc.
```
#/usr/lib/keepalived/nginx-ha-memory-check.sh
#!/bin/bash
THRESHOLD=90
MEM_USAGE=$(free | awk '/Mem/{printf("%.0f"), $3/$2*100}')
if (( MEM_USAGE > THRESHOLD )); then
  exit 1
else
  exit 0
fi
```
Include it into the keepalived.conf file.
```
global_defs {
    vrrp_version 3
}

vrrp_script chk_manual_failover {
    script   "/usr/libexec/keepalived/nginx-ha-manual-failover"
    interval 10
    weight   50
}

vrrp_script chk_nginx_memory {  <<< include the shell script to check the memory usage
    script   "/usr/lib/keepalived/nginx-ha-memory-check.sh"
    interval 3
    weight   50
}

vrrp_instance VI_1 {
    interface                  eth0
    priority                   101
    virtual_router_id          51
    advert_int                 1
    accept
    garp_master_refresh        5
    garp_master_refresh_repeat 1
    unicast_src_ip             192.168.100.100

    unicast_peer {
        192.168.100.101
    }

    virtual_ipaddress {
        192.168.100.150
    }

    track_script {
        chk_nginx_memory <<< Add it in the track_script 
        chk_manual_failover
    }

    notify "/usr/libexec/keepalived/nginx-ha-notify"
}
```
