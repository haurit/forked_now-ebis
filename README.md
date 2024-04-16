# ServiceNow - Event Based Instance Synchronizer (EBIS)
Synchronize records between ServiceNow instances based on events.
The reason this app came to life is the need to be able to capture deletes in one ServiceNow instance and transmit and execute them on another ServiceNow instance (to make sure CI Relationships are properly synchronised for example).

## Installation
Install this app on both source and target instances.
Provide the credential and URL for the "outbound" instance and link them to the Connection & Credential alias in the source instance.

## Configuration
Create a configuration record to define inbound or outbound events to be captured and processed.
