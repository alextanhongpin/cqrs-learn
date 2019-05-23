## Commands and Data Transfer Object

They may share similarity, but they are two different concepts:
- Data transfer object is more data centric
- Commands are behaviour driven

## Where are commands created?

The commands are usually created by the process manager (a.k.a) UI. But they can be internal too.

## Todos
- naming convention and also best practices
- difference between commands and events, or what is the relationship between the command and events. When publishing events, they are usually captured by the consumer, which will then generate the next command.
