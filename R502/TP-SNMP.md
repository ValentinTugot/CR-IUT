# R502 - TP - SNMP #

### 2. Interrogation d'un serveur avec Linux ###

- Mémoire totale de la machine
```bash
root@debian:~# snmpwalk -v 2c -c publicbeziers 127.0.0.1 1.3.6.1.4.1.2021.4.5
UCD-SNMP-MIB::memTotalReal.0 = INTEGER: 2030768 kB
```

- Uptime de la machine
```bash
root@debian:~# snmpwalk -v 2c -c publicbeziers 127.0.0.1 1.3.6.1.2.1.25.1.1
HOST-RESOURCES-MIB::hrSystemUptime.0 = Timeticks: (238454) 0:39:44.54
```

- Nombre de process de la machine
```bash
root@debian:~# snmpwalk -v 2c -c publicbeziers 127.0.0.1 1.3.6.1.2.1.25.1.6
HOST-RESOURCES-MIB::hrSystemProcesses.0 = Gauge32: 2
```

- L'espace de stockage utilisé sur la machine
```bash
root@debian:~# snmpwalk -v 2c -c publicbeziers 127.0.0.1 1.3.6.1.2.1.25.2.3.1.6.1
HOST-RESOURCES-MIB::hrStorageUsed.1 = INTEGER: 1877172
```

- Taille d'une unité d'allocation de stockage
```bash
root@debian:~# snmpwalk -v 2c -c publicbeziers 127.0.0.1 1.3.6.1.2.1.25.2.3.1.4.1
HOST-RESOURCES-MIB::hrStorageAllocationUnits.1 = INTEGER: 1024 Bytes
```

