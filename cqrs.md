
## CQRS and Event Sourcing

Look into how CQRS and Event Sourcing can be implemented. Also look into how Git versioning works.

What is CQRS? How can I build and test each components of event sourcing while scaling it. Check the implementation for event sourcing using Go, Nodejs, Elixir and Scala.



## Separate Read/Write

Read and writes should be separated in the microservices layer, as they can be scaled independently. Besides, it is easier to control the side-effects and make the system more maintainable (write services requires more attention). The database access can be separated too, and since write is only to the master, the creation of user in slave is only for read access.


## CQRS

https://medium.com/@domagojk/patterns-for-designing-flexible-architecture-in-node-js-cqrs-es-onion-7eb10bbefe17
https://www.confluent.io/blog/apache-kafka-for-service-architectures/
