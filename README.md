CyberCAPTOR-Data-Extraction
==============

This project is part of FIWARE. For more information, please consult [FIWARE website](http://www.fiware.org/).

CyberCAPTOR is an implementation of the Cyber Security Generic Enabler, the future developments of the [Security Monitoring GE](http://catalogue.fiware.org/enablers/security-monitoring).

## Table of Contents

- [CyberCAPTOR-Data-Extraction](#cybercaptor-data-extraction)
	- [Prerequisite](#prerequisite)
	- [Build](#build)
	- [Use the script](#user-he-script)

## Prerequisite

- Python > 3.4
- pip for Python 3 > 1.5

## Build

1) Get sources from Github

```
git clone https://github.com/fiware-cybercaptor/cybercaptor-data-extraction.git
cd cybercaptor-data-extraction
```

2) Use pip to download dependencies

```
pip3 install -r requirements.txt
```

## Use the script

Now you can use the script to generate a XML topology file (for CyberCAPTOR-Server), from several topological files (.CSV files and .XML vulnerability scan).

Here is a typical use of the script to generate the .XML topology file :

```
/usr/bin/python3 main.py --hosts-interfaces-file ./inputs/hosts-interfaces.csv --vlans-file ./inputs/vlan.csv --flow-matrix-file ./inputs/flow-matrix.csv --vulnerability-scan ./inputs/scan.nessus --routing-file ./inputs/routing.csv --to-fiware-xml-topology ./output/topology-generated.xml
```

This execution of the script parse the following inputs files:
  - `./inputs/hosts-interfaces.csv`: The CSV file describing the hosts and their network interfaces.
  - `./inputs/vlan.csv`: The CSV file describing the vlans and their default gateway.
  - `./inputs/flow-matrix.csv`: The CSV file describing the flow matrix inside the information system.
  - `./inputs/scan.nessus`: The XML file output of the Nessus scanner.
  - `./inputs/routing.csv`: The CSV file describing the routes of the routers.

The complete description of the inputs files can be found in [./doc/inputs-file-specifications.md](./doc/inputs-file-specifications.md).

It produces one output file:
  - `./output/topology-generated.xml`: The XML file containing the description of the whole network topology.
	The exhaustive description of this XML file is provided in [./doc/topology-file-specifications.md](./doc/topology-file-specifications.md).

Here is the complete script manual:

```
usage: main.py [-h] --hosts-interfaces-file HOSTS_INTERFACES_FILE 
	--vlans-file VLANS_FILE 
	[--vulnerability-scan VULNERABILITY_SCAN [VULNERABILITY_SCAN ...]]
	[--openvas-scan OPENVAS_VULNERABILITY_SCAN [OPENVAS_VULNERABILITY_SCAN ...]] 
	[--flow-matrix-file FLOW_MATRIX_FILE] 
	[--routing-file ROUTING_FILE] 
	[--mulval-output-file MULVAL_OUTPUT_FILE] 
	[--to-fiware-xml-topology TO_FIWARE_XML_TOPOLOGY] 
	[--display-infos] 
	[-v] [-vv]

Generates attack graph input files from topological files

optional arguments:
  -h, --help            show this help message and exit
  --hosts-interfaces-file HOSTS_INTERFACES_FILE
                        The CSV file containing the hosts and the interfaces.
  --vlans-file VLANS_FILE
                        The CSV file containing the VLANS.
  --vulnerability-scan VULNERABILITY_SCAN [VULNERABILITY_SCAN ...]
                        The Nessus scanner report file(s).
  --openvas-scan OPENVAS_VULNERABILITY_SCAN [OPENVAS_VULNERABILITY_SCAN ...]
                        The OpenVAS scanner report file(s).
  --flow-matrix-file FLOW_MATRIX_FILE
                        The CSV file containing the flow matrix
  --routing-file ROUTING_FILE
                        The CSV file containing the routing informations
  --mulval-output-file MULVAL_OUTPUT_FILE
                        The output path where the mulval input file will be
                        stored.
  --to-fiware-xml-topology TO_FIWARE_XML_TOPOLOGY
                        The path where the XML topology file should be stored.
  --display-infos       Display information and statistics about the topology.
  -v                    Set log printing level to INFO
  -vv                   Set log printing level to DEBUG
```
