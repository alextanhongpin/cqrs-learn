
## CQRS and Event Sourcing

Look into how CQRS and Event Sourcing can be implemented. Also look into how Git versioning works.

What is CQRS? How can I build and test each components of event sourcing while scaling it. Check the implementation for event sourcing using Go, Nodejs, Elixir and Scala.



## Separate Read/Write

Read and writes should be separated in the microservices layer, as they can be scaled independently. Besides, it is easier to control the side-effects and make the system more maintainable (write services requires more attention). The database access can be separated too, and since write is only to the master, the creation of user in slave is only for read access.


## CQRS

https://medium.com/@domagojk/patterns-for-designing-flexible-architecture-in-node-js-cqrs-es-onion-7eb10bbefe17
https://www.confluent.io/blog/apache-kafka-for-service-architectures/

DDD + CQRS flow on command

- Service receives a request from web service (the delivery mechanism should be an interface)
- Requests is routed to an appropriate controller
- Controller creates command object from a given request
- Controller sends command object to a command handler
- Command handler validate command on its own merit
- Command handler call factory to create/reconstitute domain model
- Business logic will be validated inside this domain layer
- Domain model is returned from factory back to command handler 
- Command handler call to repository to save domain model to persistent storage
- Command handler returns result to controller
- Controller sends response back to client (202 created or 204 no content)
