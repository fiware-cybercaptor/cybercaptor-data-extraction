# CyberCAPTOR Topology XML Input File description

The XML topological file defined here is the main input which is used globally for CyberCAPTOR-Server (it can be uploaded from CyberCAPTOR-Client on the Initialization page).

It unifies all topological data used by CyberCAPTOR-Server to compute the attack graphs and do the risk analysis.

The main goal of the XML topology file is to describe the network topology, all hosts and their network configuration. Each host can have several network interfaces which can be in different VLAN. The routing and filtering information attached to each host allow computing the network topology (packet route in the network, filtered packets, position of firewalls…). This file can be generated automatically thanks to the CyberCAPTOR-Data-Extraction script [https://github.com/fiware-cybercaptor/cybercaptor-data-extraction](https://github.com/fiware-cybercaptor/cybercaptor-data-extraction).


# Description of all fields (xml tags)

## Machine
All next tags defined in the father Machine tag contain all the attributes related to a machine of the topology. Each <machine> tag is related to a specific host. This father machine tag is used by the Remediation. By way, the algorithm is developed, this information is necessary to compute the solutions proposed by Remediation tool.

### Name
- Type : String
- Usage : Contains the name of a host

### Security Requirement (Optional)
- Type : String : NEGLIGEABLE/MINOR/MEDIUM/SEVERE/CATASTROPHIC
- Usage : A value describing a security requirement related to this host.

### Interfaces - Interface
These XML tags contain all the attributes related to an interface of a machine. Each <interface> tag is related to a specific network interface.

#### Name
- Type : String
- Usage : Contains the name of this interface

#### VLAN - Name (Optional)
- Type : String
- Usage : Contains the name of the VLAN attached to this interface

#### VLAN – Label (Optional)
- Type : String
- Usage : Contains the label of the VLAN attached to this interface

#### IPaddress
- Type : IP address (string)
- Usage : Contains the IP address of this interface

### Services - Service
The description of the network services or applications running on this machine.

#### Name
- Type : String
- Usage : The name of the service

#### IPaddress (Optional)
- Type : IP address (string)
- Usage : The IP address on which the service is listenning (if applicable).

#### Protocol (Optional)
- Type : TCP/UDP/ICMP/ANY (string)
- Usage : The protocol on which the service is listenning (if applicable).

#### Port (Optional)
- Type : Integer
- Usage : The port on which the service is listenning (if applicable).

#### Vulnerabilities - Vulnerability (Optional)
The vulnerabilities of this service, if applicable.

##### Type
- Type : remoteExploit/localExploit
- Usage : The type of vulnerability (cf CVSS).

##### CVE
- Type : String (CVE-YEAR-1234)
- Usage : The CVE identifier of the vulnerability.

##### Goal
- Type : String
- Usage : The goal of the vulnerability

##### CVSS
- Type : Double
- Usage : The CVSS score of the vulnerability.

### Routes - Route
These XML tags contain the routing table attached to each host. Each <route> tag contains a route of the routing table. Each host needs at least a route containing its default gateway (0.0.0.0/0.0.0.0).

#### Destination
- Type : IP address (string)
- Usage : Contains the destination network address of the route

#### Mask
- Type : IP address (string)
- Usage : Contains the network mask of the destination network

#### Gateway
- Type : String
- Usage : Contains the IP address of the gateway to take for this route (next hop)

#### Interface
- Type : IP address (string)
- Usage : Contains the interface of the host to use for this route

### Input-Firewall
This XML tag contains the input firewall table attached to each host.

#### Default-policy
- Type : ALLOW/DENY
- Usage : Contains the default policy of the input firewall table, selected if no firewall line match.

#### Firewall rule (Optional)
This XML tag contains one line of the input firewall table.

##### Protocol
- Type : String : TCP/UDP/ANY
- Usage : Contains the network flow protocol to match for this firewall line.

##### Source IP
- Type : IP address  (string)
- Usage : Contains the source network address to match for this firewall line

##### Source Mask
- Type : IP address  (string)
- Usage : Contains the source network mask to match for this firewall line

##### Source Port
- Type : integer or ANY
- Usage : Contains the source port to match for this firewall line

##### Destination IP
- Type : IP address  (string)
- Usage : Contains the destination network address to match for this firewall line

##### Destination Mask
- Type : IP address  (string)
- Usage : Contains the destination network mask to match for this firewall line

##### Destination Port
- Type : integer or ANY
- Usage : Contains the destination port to match for this firewall line

##### Action
- Type : ACCEPT / DROP
- Usage : Contains the action to do if a packet match this firewall line

### Output-Firewall
This XML tag contains the output firewall table attached to each host.

#### Default-policy
- Type : ALLOW/DENY
- Usage : Contains the default policy of the output firewall table, selected if no firewall line match.

#### Firewall rule (Optional)
This XML tag contains one line of the output firewall table.

##### Protocol
- Type : String : TCP/UDP/ANY
- Usage : Contains the network flow protocol to match for this firewall line.

##### Source IP
- Type : IP address  (string)
- Usage : Contains the source network address to match for this firewall line

##### Source Mask
- Type : IP address  (string)
- Usage : Contains the source network mask to match for this firewall line

##### Source Port
- Type : integer or ANY
- Usage : Contains the source port to match for this firewall line

##### Destination IP
- Type : IP address  (string)
- Usage : Contains the destination network address to match for this firewall line

##### Destination Mask
- Type : IP address  (string)
- Usage : Contains the destination network mask to match for this firewall line

##### Destination Port
- Type : integer or ANY
- Usage : Contains the destination port to match for this firewall line

##### Action
- Type : ACCEPT / DROP
- Usage : Contains the action to do if a packet match this firewall line

### Flow-matrix - Flow-matrix-line
This contain all the lines of the flow matrix in this network (all authorized accesses)

#### Source - Resource
- Type : String
- Usage : The name of the authorized source resource

#### Source - Type
- Type : VLAN/IP
- Usage : The type of the authorized source resource

#### Destination - Resource
- Type : String
- Usage : The name of the authorized destination resource

#### Destination - Type
- Type : VLAN/IP
- Usage : The type of the authorized destination resource

#### Source Port
- Type : Integer
- Usage : The authorized source port

#### Destination Port
- Type : Integer
- Usage : The authorized destination port

#### Protocol
- Type : TCP/UDP/ICMP/ANY
- Usage : The authorized protocol

# APPENDIX: Example topology file

```
<topology>
  <machine>
    <name>linux-user-1</name>
    <security_requirement>50</security_requirement>
    <interfaces>
      <interface>
        <name>eth0</name>
        <ipaddress>192.168.1.111</ipaddress>
        <vlan>
          <name>user-lan</name>
          <label>user-lan</label>
        </vlan>
      </interface>
    </interfaces>
    <services>
      <service>
        <name>mdns</name>
        <ipaddress>192.168.1.111</ipaddress>
        <protocol>udp</protocol>
        <port>5353</port>
      </service>
    </services>
    <routes>
      <route>
        <destination>0.0.0.0</destination>
        <mask>0.0.0.0</mask>
        <gateway>192.168.1.1</gateway>
        <interface>eth0</interface>
      </route>
    </routes>
  </machine>
  <machine>
    <name>linux-user-2</name>
    <security_requirement>30</security_requirement>
    <interfaces>
      <interface>
        <name>eth0</name>
        <ipaddress>192.168.1.112</ipaddress>
        <vlan>
          <name>user-lan</name>
          <label>user-lan</label>
        </vlan>
      </interface>
    </interfaces>
    <services>
      <service>
        <name>mdns</name>
        <ipaddress>192.168.1.112</ipaddress>
        <protocol>udp</protocol>
        <port>5353</port>
        <vulnerabilities>
          <vulnerability>
            <type>remoteExploit</type>
            <cve>CVE-2007-2446</cve>
            <goal>privEscalation</goal>
            <cvss>10.0</cvss>
          </vulnerability>
        </vulnerabilities>
      </service>
    </services>
    <routes>
      <route>
        <destination>0.0.0.0</destination>
        <mask>0.0.0.0</mask>
        <gateway>192.168.1.1</gateway>
        <interface>eth0</interface>
      </route>
    </routes>
  </machine>
  <machine>
    <name>Dmz-1</name>
    <security_requirement>10</security_requirement>
    <interfaces>
      <interface>
        <name>eth0</name>
        <ipaddress>10.15.10.11</ipaddress>
        <vlan>
          <name>dmz</name>
          <label>dmz</label>
        </vlan>
      </interface>
    </interfaces>
    <services>
      <service>
        <name>dns</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>udp</protocol>
        <port>53</port>
      </service>
      <service>
        <name>general</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>0</port>
      </service>
      <service>
        <name>general</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>udp</protocol>
        <port>0</port>
      </service>
      <service>
        <name>cifs</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>443</port>
        <vulnerabilities>
          <vulnerability>
            <type>remoteExploit</type>
            <cve>CVE-2007-2446</cve>
            <goal>privEscalation</goal>
            <cvss>10.0</cvss>
          </vulnerability>
        </vulnerabilities>
      </service>
      <service>
        <name>cifs</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>443</port>
        <vulnerabilities>
          <vulnerability>
            <type>remoteExploit</type>
            <cve>CVE-1999-0519</cve>
            <goal>privEscalation</goal>
            <cvss>7.5</cvss>
          </vulnerability>
          <vulnerability>
            <type>remoteExploit</type>
            <cve>CVE-1999-0520</cve>
            <goal>privEscalation</goal>
            <cvss>6.4</cvss>
          </vulnerability>
        </vulnerabilities>
      </service>
      <service>
        <name>cifs</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>445</port>
      </service>
      <service>
        <name>rpc-nfs</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>2049</port>
      </service>
      <service>
        <name>cifs</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>445</port>
      </service>
      <service>
        <name>dns</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>53</port>
      </service>
      <service>
        <name>x11</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>6000</port>
      </service>
      <service>
        <name>smb</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>139</port>
      </service>
      <service>
        <name>rpc-portmapper</name>
        <ipaddress>10.15.10.11</ipaddress>
        <protocol>tcp</protocol>
        <port>111</port>
      </service>
    </services>
    <routes>
      <route>
        <destination>0.0.0.0</destination>
        <mask>0.0.0.0</mask>
        <gateway>10.15.10.1</gateway>
        <interface>eth0</interface>
      </route>
    </routes>
  </machine>
  <machine>
    <name>Dmz-2</name>
    <security_requirement>10</security_requirement>
    <interfaces>
      <interface>
        <name>eth0</name>
        <ipaddress>10.15.10.14</ipaddress>
        <vlan>
          <name>dmz</name>
          <label>dmz</label>
        </vlan>
      </interface>
    </interfaces>
    <services>
      <service>
        <name>www</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>tcp</protocol>
        <port>8180</port>
      </service>
      <service>
        <name>irc</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>tcp</protocol>
        <port>6667</port>
      </service>
      <service>
        <name>vnc</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>tcp</protocol>
        <port>5900</port>
      </service>
      <service>
        <name>smtp</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>tcp</protocol>
        <port>25</port>
      </service>
      <service>
        <name>general</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>icmp</protocol>
        <port>443</port>
        <vulnerabilities>
          <vulnerability>
            <type>localExploit</type>
            <cve>CVE-1999-0524</cve>
            <goal>privEscalation</goal>
            <cvss>0.0</cvss>
          </vulnerability>
        </vulnerabilities>
      </service>
      <service>
        <name>dns</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>udp</protocol>
        <port>53</port>
      </service>
      <service>
        <name>dns</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>udp</protocol>
        <port>53</port>
      </service>
      <service>
        <name>www</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>tcp</protocol>
        <port>8180</port>
      </service>
      <service>
        <name>rpc-portmapper</name>
        <ipaddress>10.15.10.14</ipaddress>
        <protocol>tcp</protocol>
        <port>111</port>
      </service>
    </services>
    <routes>
      <route>
        <destination>0.0.0.0</destination>
        <mask>0.0.0.0</mask>
        <gateway>10.15.10.1</gateway>
        <interface>eth0</interface>
      </route>
    </routes>
  </machine>
  <machine>
    <name>router</name>
    <security_requirement>0</security_requirement>
    <interfaces>
      <interface>
        <name>eth0</name>
        <ipaddress>192.168.1.1</ipaddress>
        <vlan>
          <name>user-lan</name>
          <label>user-lan</label>
        </vlan>
      </interface>
      <interface>
        <name>eth1</name>
        <ipaddress>10.15.10.1</ipaddress>
        <vlan>
          <name>dmz</name>
          <label>dmz</label>
        </vlan>
      </interface>
      <interface>
        <name>eth2</name>
        <ipaddress>1.1.2.2</ipaddress>
        <directly-connected>
          <internet />
        </directly-connected>
      </interface>
    </interfaces>
    <services />
    <routes>
      <route>
        <destination>10.15.10.1</destination>
        <mask>255.255.255.0</mask>
        <gateway>10.15.10.1</gateway>
        <interface>eth1</interface>
      </route>
      <route>
        <destination>192.168.1.1</destination>
        <mask>255.255.255.0</mask>
        <gateway>192.168.1.1</gateway>
        <interface>eth0</interface>
      </route>
      <route>
        <destination>0.0.0.0</destination>
        <mask>0.0.0.0</mask>
        <gateway>1.1.1.1</gateway>
        <interface>eth2</interface>
      </route>
    </routes>
  </machine>
  <flow-matrix>
    <flow-matrix-line>
      <source resource="dmz" type="VLAN" />
      <destination resource="user-lan" type="VLAN" />
      <source_port>any</source_port>
      <destination_port>80</destination_port>
      <protocol>TCP</protocol>
    </flow-matrix-line>
    <flow-matrix-line>
      <source resource="user-lan" type="VLAN" />
      <destination resource="dmz" type="VLAN" />
      <source_port>any</source_port>
      <destination_port>any</destination_port>
      <protocol>any</protocol>
    </flow-matrix-line>
    <flow-matrix-line>
      <source type="INTERNET" />
      <destination resource="dmz" type="VLAN" />
      <source_port>any</source_port>
      <destination_port>any</destination_port>
      <protocol>any</protocol>
    </flow-matrix-line>
    <flow-matrix-line>
      <source type="INTERNET" />
      <destination resource="dmz" type="VLAN" />
      <source_port>any</source_port>
      <destination_port>443</destination_port>
      <protocol>TCP</protocol>
    </flow-matrix-line>
    <flow-matrix-line>
      <source resource="user-lan" type="VLAN" />
      <destination type="INTERNET" />
      <source_port>any</source_port>
      <destination_port>any</destination_port>
      <protocol>any</protocol>
    </flow-matrix-line>
    <flow-matrix-line>
      <source resource="dmz" type="VLAN" />
      <destination type="INTERNET" />
      <source_port>any</source_port>
      <destination_port>any</destination_port>
      <protocol>any</protocol>
    </flow-matrix-line>
    <flow-matrix-line>
      <source resource="10.15.10.11" type="IP" />
      <destination resource="192.168.1.112" type="IP" />
      <source_port>any</source_port>
      <destination_port>5353</destination_port>
      <protocol>TCP</protocol>
    </flow-matrix-line>
  </flow-matrix>
</topology>
```
