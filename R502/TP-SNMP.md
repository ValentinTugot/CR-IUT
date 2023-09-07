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

__snmptable:__  

- Afficher la table des interfaces réseaux
```bash
root@debian:~# snmptable -v2c -c publicbeziers localhost IF-MIB::ifTable
SNMP table: IF-MIB::ifTable

 ifIndex ifDescr           ifType ifMtu    ifSpeed  ifPhysAddress ifAdminStatus ifOperStatus ifLastChange ifInOctets ifInUcastPkts ifInNUcastPkts ifInDiscards ifInErrors ifInUnknownProtos ifOutOctets ifOutUcastPkts ifOutNUcastPkts ifOutDiscards ifOutErrors ifOutQLen              ifSpecific
       1      lo softwareLoopback 65536   10000000                           up           up 0:0:00:00.00       1720             8              0            0          0                 0        1720              8               0             0           0         0 SNMPv2-SMI::zeroDotZero
       6    eth0   ethernetCsmacd  1500 4294967295 2:42:ac:11:0:2            up           up 0:0:00:00.00     712400          7245              0            0          0                 0      749757           7228               0             0           0         0 SNMPv2-SMI::zeroDotZero
```

- Affiche la table des partitions
```bash
root@debian:~# snmptable -v2c -c publicbeziers localhost HOST-RESOURCES-MIB::hrPartitionTable
HOST-RESOURCES-MIB::hrPartitionTable: No entries
```

- Table des load average
```bash
root@debian:~# snmptable -v2c -c publicbeziers localhost UCD-SNMP-MIB::laTable
SNMP table: UCD-SNMP-MIB::laTable

 laIndex laNames laLoad laConfig laLoadInt laLoadFloat laErrorFlag laErrMessage
       1  Load-1   0.04    12.00         4    0.040000     noError
       2  Load-5   0.03    10.00         3    0.030000     noError
       3 Load-15   0.00     5.00         0    0.000000     noError
```

-


