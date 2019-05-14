## Automated-Threat-Intelligent-System(ATIS v1.0) 
[![DUB](https://img.shields.io/badge/build-passing-brightgreen.svg)](https://www.travis-ci.org/kaiiyer/automated-threat-intelligent-model)![ ]      [![Join the chat at https://gitter.im/kaiiyer/automated-threat-intelligent-model](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/kaiiyer/automated-threat-intelligent-model)![ ]     [![DUB](https://img.shields.io/github/license/kaiiyer/automated-threat-intelligent-model.svg)](https://github.com/kaiiyer/automated-threat-intelligent-model/blob/master/LICENSE)![ ]    [![DUB](https://img.shields.io/github/issues/kaiiyer/automated-threat-intelligent-model.svg)]((https://github.com/kaiiyer/automated-threat-intelligent-model/issues))![ ]      [![DUB](https://img.shields.io/badge/status-ongoing-blue.svg)](https://img.shields.io/badge/status-ongoing-blue.svg) ![ ](    [![Follow @Anoop_krish47](https://img.shields.io/twitter/url/https/github.com/kaiiyer/automated-threat-intelligent-model.svg?style=social)]((https://twitter.com/Anoop_krish47))

An improvised automated threat intelligent system with advanced vulnerability scanners and Opensource Intelligence Information gathering python scripts when integrated with McAfee Advanced Threat Defense and Malware Information Sharing Platform can defend against new and futuristic cyber attacks.

Emerged as a Hackathon Project at Make-A-Ton.

# ATD-MISP with OpenDXL

This integration is focusing on the automated threat intelligence collection with McAfee ATD, OpenDXL and MISP.
McAfee Advanced Threat Defense (ATD) will produce local threat intelligence that will be pushed via DXL. 
An OpenDXL wrapper will subscribe and parse indicators ATD produced and will import indicators into a threat intelligence management platform (MISP). 

## Component Description

**McAfee Advanced Threat Defense (ATD)** is a malware analytics solution combining signatures and behavioral analysis techniques to rapidly identify malicious content and provides local threat intelligence. ATD exports IOC data in STIX format in several ways including the DXL.
https://www.mcafee.com/in/products/advanced-threat-defense.aspx

**MISP** threat sharing platform is free and open source software helping information sharing of threat and cyber security indicators.
https://github.com/MISP/MISP

## Prerequisites

Download the [Latest Release](https://github.com/kaiiyer/automated-threat-intelligent-model)
   * Extract the release .zip file
   
MISP platform installation ([Link](https://github.com/MISP/MISP)) (tested with MISP 2.4.70)

PyMISP library installation ([Link](https://github.com/CIRCL/PyMISP)) or install dependencies
using the requirements.txt file as mentioned below.

OpenDXL Python installation
1. Python SDK Installation ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/installation.html))
    Install the required dependencies with the requirements.txt file:
    ```sh
    $ pip install -r requirements.txt
    ```
    This will install the dxlclient, and pymisp modules. 
2. Certificate Files Creation ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/certcreation.html))
3. ePO Certificate Authority (CA) Import ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/epocaimport.html))
4. ePO Broker Certificates Export ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/epobrokercertsexport.html))

McAfee ATD solution (tested with ATD 3.8)

## Configuration
McAfee ATD receives files from multiple sensors like Endpoints, Web Gateways, Network IPS or via Rest API. ATD will perform malware analytics and produce local threat intelligence. After an analysis every indicator of comprise will be published via the Data Exchange Layer (topic: /mcafee/event/atd/file/report).

### atd_subscriber.py
The atd_subscriber.py receives DXL messages from ATD, prepares the JSON and loads misp.py.

Change the CONFIG_FILE path in the atd_subscriber.py file

`CONFIG_FILE = "/path/to/config/file"`

### misp.py
The misp.py script receives the JSON messages and parses IOCs and uses the Python API from MISP (PyMISP) to create a new threat event, add atributes and asign a tag.

Change the misp_url and misp_key

`misp_url = 'https://misp-url.com/`

`misp_key = 'auth-key'`

The MISP auth key can be found under the automation section in MISP.

Change the tag assignment in line 133

`misp.add_tag(event, str("ATD:Report"))`

Make sure that you added the tag in MISP already.

## Run the OpenDXL wrapper
> python atd_subscriber.py

or

> nohup python atd_subscriber.py &

## Summary
With this use case, ATD produces local intelligence and contributes information to an intelligence management platform like MISP.
MISP is able to combine global, community and locally produced intelligence.


<img src="https://66.media.tumblr.com/b323da789262c09b7c5b135cd46dd6bb/tumblr_pllgi5VnuU1wnca1uo1_1280.png" alt="Screenshot1">
<img src="https://cloud.githubusercontent.com/assets/25227268/25057844/d5ded02a-2173-11e7-914d-422329a1bb51.PNG" alt="Screenshot2">
<img src="https://66.media.tumblr.com/b8745deee40b1068be5895ab1a063544/tumblr_pllgi5VnuU1wnca1uo2_1280.png" alt="Screenshot3">
<img src="https://66.media.tumblr.com/63856d859cba4d64053140d060137461/tumblr_pllgi5VnuU1wnca1uo3_1280.png" alt="Screenshot4">
<img src="https://66.media.tumblr.com/8b3df16ae586be9ca844f8a6b24850b6/tumblr_pllgi5VnuU1wnca1uo4_1280.png" alt="Screenshot5">
<img src="https://66.media.tumblr.com/45e2f7b02feb9e39e97d1599cb196184/tumblr_pllgi5VnuU1wnca1uo5_1280.png" alt="Screenshot6">

# Active Response-Elastic

This integration is focusing on the automated real-time threat hunting with McAfee ATD, OpenDXL, Active Response and Elasticsearch. McAfee Advanced Threat Defense will produce local threat intelligence that will be pushed via DXL. An OpenDXL wrapper will subscribe and parse indicators ATD produced and execute automated Active Response searches across multiple DXL fabrics. The result will be imported in a big data analytic platform. 

## Component Description
**McAfee Advanced Threat Defense (ATD)** is a malware analytics solution combining signatures and behavioral analysis techniques to rapidly identify malicious content and provides the local threat intelligence for our solution. ATD exports IOC data in STIX format in several ways including DXL. https://www.mcafee.com/in/products/advanced-threat-defense.aspx

**McAfee Active Response (MAR)** is an incident response solution that leverage the DXL messaging fabric to support the threat hunting process and provide real time visibility. https://www.mcafee.com/in/products/endpoint-threat-defense-response.aspx

**Elasticsearch** is a search engine that provides a distributed, multitenant-capable full-text search engine. Kibana is an open source data visualization plugin for Elasticsearch that provides visualization capabilities on top of the content indexed on Elasticsearch. https://www.elastic.co/

## Prerequisites
McAfee ATD solution (tested with ATD 3.8)

Clone the repo
```
git clone https://github.com/kaiiyer/automated-threat-intelligent-model.git
```

OpenDXL Python installation
1. Python SDK Installation ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/installation.html))
    Install the required dependencies with the requirements.txt file:
    ```sh
    $ pip install -r requirements.txt
    ```
    This will install the dxlclient, dxlmarclient, and  elasticsearch modules.     
2. Certificate Files Creation ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/certcreation.html))
3. ePO Certificate Authority (CA) Import ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/epocaimport.html))
4. ePO Broker Certificates Export ([Link](https://opendxl.github.io/opendxl-client-python/pydoc/epobrokercertsexport.html))
5. Python SDK for MAR Installation ([Link](https://github.com/opendxl/opendxl-mar-client-python))

Elasticsearch and Kibana (tested with 5.1.2)

Elasticsearch Python client ([Link](https://github.com/elastic/elasticsearch-py)). This dependency will be installed
as part of install using the requirements.txt file.

## Configuration
McAfee ATD receives files from multiple sensors like Endpoints, Web Gateways, Network IPS or via Rest API. ATD will perform malware analytics and produce local threat intelligence. After an analysis every indicator of comprise will be published via the Data Exchange Layer (topic: /mcafee/event/atd/file/report).

### atd_subscriber.py
The atd_subscriber.py receives DXL messages from ATD, parse out the hash information and loads marc1.py and marc2.py. (This can be extended by using e.g. C2 IP's ATD discovered.)

Change the CONFIG_FILE path in the atd_subscriber.py file (line 25)

`CONFIG_FILE = "/path/to/config/file"`

Change the Elasticsearch information (line 33)

`es = Elasticsearch(['http://elasticsearchurl:port'])`

### marc1.py (First DXL fabric)
The marc1.py receives the hash information ATD discovered (including the main file hashes as well as dropped file hashes) and launches multiple Active Response searches. The client response will automatically pushed and indexed by Elasticsearch.

Change the Elasticsearch information (line 11)

`es = Elasticsearch(['http://elasticsearchurl:port'])`

Change the CONFIG_FILE path (line 16)

`CONFIG_FILE = "/path/to/config/file"`

### marc2.py (Second DXL fabric)
Repead the same steps mention under marc1.py if you want to search in other DXL fabrics (multiple DXL fabrics).

## Run the OpenDXL wrapper
> python atd_subscriber.py

or

> nohup python atd_subscriber.py &

## Summary
With this use case, ATD produces local intelligence and pushes IOC information via DXL. With OpenDXL we are able to receives these information and launch multiple Active Response lookups. The client response will automatically pushed to Elasticsearch.

It is possible to visualize the results with Kibana. Make sure to add the Index Patterns first.

The Dashboard below shows the latest ATD analysis (atd index) and the two rows below show the indicators Active Response found in DXL fabric1 (marc1 index) and DXL fabric2 (marc2 index).

![22_atd_mar_elastic](https://cloud.githubusercontent.com/assets/25227268/25066853/9b207370-2232-11e7-981b-5e84ed242d18.PNG)



## Achievements and recognitions :
1.The project was awarded with the honorable mention at Dhishna Make-a-ton.

2.The project was shortlisted for finals in Beach Hack 2019.

3.The project was awarded first runner-up at Innpasco 3.0 Project Presentation.

4.The project was shortlisted for internship-offer by Geektrust.

5.The project was shortlisted for Pre-finals in EY Ideathon 2019.

6.The project was shortlisted for Idea Fest 2019 Finals from EY Challenge.
