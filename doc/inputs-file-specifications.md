# Topological inputs file specifications

CyberCAPTOR-Data-Extraction can take several types of inputs to generate the XML topology file.

We describe in this documentation file the format of the inputs that are currently taken by the script. CyberCAPTOR-Data-Extraction may be extended to take into account new types of inputs.

Note that all CSV files use a semi-colon `;` as separator, as it is done by default with Microsoft Excel.

## Topological files

### Host-interfaces file

This CSV file describes the hosts of the topology, with their network interface. 

#### Columns explanations

Hostname  | Interface Name | IP address | Connected to WAN | Metric
--------- | -------------- | ---------- | ---------------- | ------
The name of the host (without spaces) | The name of the interface | The IP address of the interface | Whether or not this network interface is connected to WAN (Internet) | A Metric describing the importance of the services running on this IP address.


#### Example

```
Hostname;Interface Name;IP address;Connected to WAN;Metric
linux-user-1;eth0;192.168.1.111;false;7
linux-user-2;eth0;192.168.1.112;false;30
Dmz-1;eth0;10.15.10.11;false;0.8
Dmz-2;eth0;10.15.10.14;false;0.7
router;eth0;192.168.1.1;false;0.1
router;eth1;10.15.10.1;false;0.1
router;eth2;1.1.2.2;true
```

### Vlans file

This CSV file describes the subnetworks/VLANS of the network topology.

#### Columns explanations

VLAN Name | VLAN Address | VLAN Netmask | VLAN default Gateway 
--------- | ------------ | ------------ | --------------------
The name of the VLAN | The IP address of the network | The subnet mask CIDR | The IP address of the VLAN default gateway

#### Example

```
name;address;netmask;gateway
user-lan;192.168.1.0;24;192.168.1.1
dmz;10.15.10.0;24;10.15.10.1

```

### Flow Matrix File 

This CSV file describes the authorized accesses in the network topology. Note that all accesses that are not specified are supposed unauthorized.

#### Columns explanations

Source | Destination | Source port | Destination port | Protocol
------ | ----------- | ----------- | ---------------- | -------
The source network (`IP/mask`) or internet | The destination network (`IP/mask`) or internet | The source port or `any` | the destination portor `any` | the protocol or `any`.

Each line describes an authorized access.

#### Example

```
"source";"destination";"source_port";"destination_port";"protocol"
"10.15.10.0/24";"192.168.1.0/24";"any";80;"TCP"
"192.168.1.0/24";"10.15.10.0/24";"any";"any";"any"
"internet";"10.15.10.0/24";"any";"any";"any"
"internet";"10.15.10.0/24";"any";443;"TCP"
"192.168.1.0/24";"internet";"any";"any";"any"
"10.15.10.0/24";"internet";"any";"any";"any"
10.15.10.11;192.168.1.112;any;5353;TCP
```

### Routing file

This file describes the routes of the hosts that have routes, others than the default gateways of the interfaces' VLAN.

#### Columns explanations

Host | Destination | Mask | Gateway | Interface
---- | ----------- | ---- | ------- | ---------
The name of the host for which this route is specified | The destination network of this route | the network mask of this route | The gateway IP address for this route | The outgoing interface of the route.

#### Example

```
host;destination;mask;gateway;interface
router;10.15.10.1;255.255.255.0;10.15.10.1;eth1
router;192.168.1.1;255.255.255.0;192.168.1.1;eth0
router;0.0.0.0;0.0.0.0;1.1.1.1;eth2
```

## Vulnerability scanner files 

Currently, two vulnerability scanners can be used: Nessus and OpenVAS.

### Nessus scanner files

The outputs of the vulnerability scanner Nessus are stored in a .nessus file, which is an XML file.

The only outputs that are used in this file are:
```
<Report>
<ReportHost name="host ip address or hostname">
<ReportItem port="service port" svc_name="service name" protocol="service protocol">
<cve>CVE-2015-1234</cve>
<cve>CVE-2015-2345</cve>
</ReportItem>
<ReportItem>
...
</ReportItem>
```

### OpenVAS files

The outputs of the vulnerability scanner OpenVAS are stored in an XML file.

