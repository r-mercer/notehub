# Events Versus Messages

## Events

Events represent a fact or state changed, such as an OrderPlaced. Events are broadcast async to multiple consumers, promoting loose coupling and scalability.

## Messages

Messages are broader and can encompass commands, such as ProcessPayments, events and documents. 

## Both

Both Messages and events can support various delivery guarentees, such as at least once, at most once and exactly once. 

Delivery patterns for both can include pub/sub methods and point to point, one to one and one to many respectively. 

Messaging usually also support a request / reply format, which can be achieved with events, but it's a bit of a hack. 

Events excel at state propagatoin and decoupled notifications, whilst messages are better for command execution, workflow orchestration and explicit directed coordination.

You should probably use both. 
