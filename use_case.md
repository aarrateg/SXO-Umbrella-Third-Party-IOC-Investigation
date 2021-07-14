# False Negative Check Workflow: the Use Case

## Problem Statement

Cisco customers are often receiving Threat Intelligence information from 3rd party feeds. 
The IOCs contained in the 3rd party feed might be still not known by Cisco Talos. In this case, the Cisco Security solutions (Umbrella, AMP, Firepower...) might not block the threat. This would be a False Negative occurrence. 

Customer need: 
* Easily Identify the 3rd party IOCs that are still not identified as Malicious by my Cisco solutions. 
* Quickly check if I have any target that is affected by these IOCs. 
* Easily parse and process the IOCs, which are often shared in MISP format (https://www.misp-project.org/) 

## Solution

* The workflow takes a JSON file from the web, which is the description of a threat feed, including observables, and automatically parses it and processes it through Threat Response. 
* The source JSON file is formatted according MISP (https://www.misp-project.org/), which is a well known format for sharing threat information. 
* Particular attention is given to the observables which are NOT known as malicious by the Security products which are integrated in Threat Response. In fact, the 3rd party feed might contain a threat indication which is not yet known to Talos, and therefore not blocked. That's why the workflow is named "False Negative Check". 
* In case there are observables which are not yet known as malicious, they are stored in a new Casebook. A further analysis is done, to find if there are any affected targets: in this case, a new Incident is created. 

## Key Customer Outcomes

* Observables in IOC feed are parsed and converted to actionable items, thanks to SecureX Orchestration. 
* A Casebook is created, so that a quick investigation is possible. 
* In case of targets found, an Incident is created, so that the Incident Management team on the customer side can take quick remediation actions. 

## Products used

Cisco Threat Response (with AMP and Umbrella integrated, at least); Webex.

## Learning Secure X Orchestration (SXO)
It's highly advised to go through the Secure X Orchestration on DevNet (https://developer.cisco.com/learning/modules/SecureX-orchestration) to get familiarized with the tool. 


