![image](https://github.com/DaGolle/now-EventBasedInstanceSynchronizer/assets/14361888/d621d649-6210-4136-a6a9-d31f194013ef)# ServiceNow - Event Based Instance Synchronizer (EBIS)
Synchronize records between ServiceNow instances based on events.
The reason this app came to life is the need to be able to capture deletes in one ServiceNow instance and transmit and execute them on another ServiceNow instance (to make sure CI Relationships are properly synchronised for example).

## Design
Allow exchange of basic database table operations insert, update and delete via the ServiceNow Event Queue [sysevent].
A table operation on the source instance table will trigger a Business Rule (configured by the app) which will generate an Outbound Event. That event will be processed by a Script Action that will call a REST API on the target instance.
An Inbound event with the same payload will be created on the target instance that is in turn processed by a Script Action that will execute the same table operation on the target table.
![Configuration Flow diagram](images/synchronization_flow.png?raw=true "Synchronization Flow")

## Installation
Install this app on both source and target instances.
Provide the credential and URL for the "outbound" instance and link them to the Connection & Credential alias in the source instance.

## Configuration
Create a configuration record to define inbound or outbound events to be captured and processed.
![Configuration Flow diagram](images/configuration_flow.png?raw=true "Configuration Flow")
