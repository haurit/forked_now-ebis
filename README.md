# ServiceNow - Event Based Instance Synchronizer (EBIS)
Synchronize records between ServiceNow instances based on events.
The reason this app came to life is the need to be able to capture deletes in one ServiceNow instance and transmit and execute them on another ServiceNow instance (to make sure CI Relationships are properly synchronised for example).

## Design
Allow exchange of basic database table operations insert, update and delete via the ServiceNow Event Queue [sysevent].
A table operation on the source instance table will trigger a Business Rule (configured by the app) which will generate an Outbound Event. That event will be processed by a Script Action that will execute a Flow Action, which will inturn call a REST API on the target instance.
An Inbound event with the same payload will be created on the target instance that is in turn processed by a Script Action that will execute the same table operation on the target table.
![Configuration Flow diagram](images/synchronization_flow.png?raw=true "Synchronization Flow")

## Installation
Install this app on both source and target instances.
Provide the credential and URL for the "outbound" instance and link them to the Connection & Credential alias in the source instance.

## Configuration
Create a configuration record to define inbound or outbound events to be captured and processed.
![Configuration Flow diagram](images/configuration_flow.png?raw=true "Configuration Flow")
### Source/Outbound Instance
The source instance should have an active **Outbound** Configuration record setup for the table to monitor. Once this Configuration record is created, a(t least one) Field Mapping needs to be linked to the configuration. After that, the configuration can be activated and the EBIS app will generate the necessary Business Rules to capture record transactions that need syncrhonizing to the target instance.
![Outbound Configuration screenshot](images/configuration_outbound.png?raw=true "Example Outbound Configuration")
#### Outbound options
- Table override: optional value to replace the table name in the outbound payload
- Delay: time to wait before processing an outbound event; the delay will be added to the current date/time and written to the "Process on [process_on]" field in the Event [sysevent] table
### Target/Inbound Instance
The target instance should have an active **Inbound** Configuration record for the same table that was in the Outbound Configuration record in the source instance. Once this Configuration record is created, a(t least one) Field Mapping should be linked to the configuration. After that, the configuration can be activated and the EBIS app will start processing incoming events from the source instance.
![Inbound Configuration screenshot](images/configuration_inbound.png?raw=true "Example Inbound Configuration")
#### Inbound options
- Ignore unknown fields: if the event payload contains an attribute that is not mapped in the configuration, it will result in an error unless this field is checked (true)
- Table override: optional value to accept a different table name from the payload than the one that is defined in the configuration. This allows mapping to a different table (e.g. custom table to baseline table or global table to scoped app table)
- Run business rules: see GlideRecord API setWorkflow
