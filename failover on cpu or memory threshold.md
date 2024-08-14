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
include it into the keepalived.conf file.
