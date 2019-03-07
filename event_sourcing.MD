# Event sourcing

## What: 
Capture state changes as events
Stores events in the sequence it happens
Event store is immutable and append only
Current state is built from these events

With event sourced systems, user performs an action, the system processes this, appends it to the event store, the system again processes this and projections calculated are added to a main database
Event store as correlation_id and causation_id fields. Note that this isn't a relational database.
Event source is one big stream, but is split up into domains, each with their own aggregate id. So a Domain Driven Design model works well for event sourcing (i.e. receives a command, emits events, has its state populated from replaying an event stream)

## Projectors:
Listens for events from the event store
Writes data to projections
Projections are the "current states" of your application
Many projectors can be implemented to project your events for different purposes
Projectors my be idempotent and not emit events
Al data written by projects are disposable.

## Process Managers:
Execute long running processes
Execute multi-step process
Coordinate multiple aggregates
Cut across bounded contexts
e.g. customer places order, this is captured as an event, the order process manager goes to the order aggregate and creates an order, the process manager also goes to reservation, and the payment aggregates.
Process manager can have its own state, if needed, but itself doesn't create events

## Why:
Good for banks, your total isn't a column in a database, it's derived from events

Why use event sourcing:
Structure changes more often than behaviour
When history matters, high criticality and high fidelity components of application

(to do: research the "apply" keyword in regards to event sourcing aggregates)

CQRS: Command Query Responsibility Segregation
Event store - write model
Projections - read model
FIts well with event sourcing, but not with whole application

