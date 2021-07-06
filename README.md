![License: CISCO](https://img.shields.io/badge/License-CISCO-blue.svg)
[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/<REPO-HERE>)


## Features
* The workflow takes a JSON file from the web, which is the description of a threat feed, including observables, and automatically parses it and processes it through Threat Response.
* The source JSON file is formatted according MISP (https://www.misp-project.org/), which is a well known format for sharing threat information.
* Particular attention is given to the observables which are NOT known as malicious by the Security products which are integrated in Threat Response. In fact, the 3rd party feed might contain a threat indication which is not yet known to Talos, and therefore not blocked. That's why the workflow is named "False Negative Check".
* In case there are observables which are not yet known as malicious, they are stored in a new Casebook. In this case, a further analysis is done, to find if there are any affected targets: in this case, a new Incident is created.

Here are the workflow steps:

1. Get Indicators - Make a generic http request to a web hosted IOC JSON file in MISP format, parse it and store the indicators.
2. Parse IOCs - Parse the observable types, using SecureX Threat Response Inspect API.
3. Enrich Observables - with SecureX Threat Response Enrich API to find any global sightings (in integrated threat feeds) and more importantly local sightings/targets (in integrated security modules like Umbrella, AMP, etc.). Filter what is considered NOT malicious, and store these observables in a table. (Note: focusing on NON malicious IoCs here, because we are assuming that the malicious ones have already been blocked by the security products, and we want to investigate on unknown threats, instead.)
4. Create Casebook - Format the observables in the right JSON, and use CTR to create a new casebook.
5. Create Incident - If there are targets in the network, also create an Incident and link it to the Casebook.

The Casebook creation and the Incident creation will also be notified to Webex Teams, with all the details of the observable and the targets found.

> **Note:** Please test this properly before implementing in a production environment. This is a sample workflow!

## Required Targets
- An HTTPS target hosting the JSON file to be fetched. In the sample, a target named "Dropbox" is being used.
- CTR_For_Access_Token (default)
- CTR_API (default)
- Webex Teams

## Required Account Keys
- CTR_Credentials
- Webex Teams Bot Token

## Required Global Variables
- Webex Teams Room

## Required Atomic Workflows
- CTRGenerateAccessToken
- CTRInspect
- CTR Enrich Observable
- Threat Response V2 - Generate Access Token
- Threat Response V2 - Create Casebook
- Threat Response V2 - Create Incident
- Threat Response V2 - Create Sighting
- Threat Response V2 - Create Relationship
- Webex Teams - Post Message to Room

## Setup instructions

### Configure Global Variables

1. Browse to your SecureX orchestration instance. This will be a different URL depending on the region your account is in: 

* US: https://securex-ao.us.security.cisco.com/orch-ui/workflows/
* EU: https://securex-ao.eu.security.cisco.com/orch-ui/workflows/
* APJC: https://securex-ao.apjc.security.cisco.com/orch-ui/workflows/

2. In the left hand menu, select **Variables**.

3. Configure the required global variables, as listed in the section **Required Global Variables** above:

* Webex Teams Room. String Containing the Webex Room where alerts should be pushed.
    ![image](https://user-images.githubusercontent.com/86785216/124569743-824b9300-de46-11eb-95df-d0d9dc9cc466.png)


### Import atomic actions

1. In the left pane menu, select **Workflows**. Click on **IMPORT** to import the workflow.

![image](https://user-images.githubusercontent.com/86785216/124569981-be7ef380-de46-11eb-8fb3-e7b7dc2f6b18.png)


2. Choose to import from Git.

3. Under GIT REPOSITORY, select GitHub_Target_Atomics.

![image](https://user-images.githubusercontent.com/86785216/124570069-d35b8700-de46-11eb-9337-f9bbda125373.png)


4. Make sure to import all the Atomic action listed above, in the section named **Required Atomic Workflows**. Please don't use **IMPORT AS A NEW WORKFLOW (CLONE)**, because you need to make sure that the main workflow finds the original atomic actions identifiers.

### Import main workflow

1. In the left pane menu, select **Workflows**. Click on **IMPORT** to import the workflow.

![image](https://user-images.githubusercontent.com/86785216/124570362-17e72280-de47-11eb-87f7-70fdc8396818.png)

2. Click on **Browse** and copy paste the content of the [False Negative Check-MispFeed.json](False%20Negative%20Check%20-%20MISP%20Feed.json) file inside of the text window.  Select **IMPORT AS A NEW WORKFLOW (CLONE)** and click on **IMPORT**.

![image](https://user-images.githubusercontent.com/86785216/124570295-0a319d00-de47-11eb-82e8-3d405253b337.png)


3. Configure the required targets and accounts, as listed in the sections **Required Targets**  above.

![image](https://user-images.githubusercontent.com/86785216/124570430-29302f00-de47-11eb-9d7d-8f1ec606ffac.png)


4. Configure the required targets and accounts, as listed in the sections **Required Account Keys** above.

![image](https://user-images.githubusercontent.com/86785216/124570476-3816e180-de47-11eb-97e9-2d6c9c97f351.png)


5. Make sure that the HTTPS server that you're using in the very first action contains a JSON file describing the threat feed, formatted according to MISP standard. A sample file can be found in this repository, as [sample-observable-feed.json](sample-observable-feed.json).

## Notes

* Please test this properly before implementing in a production environment. This is a sample workflow!

## Author(s)

* Pier Paolo Glave, Aritz Arrate Galan, Mickael Pontoizeau, Juan Miguel Aguayo
 (Cisco)
