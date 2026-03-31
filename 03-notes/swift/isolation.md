# Main-Actor Isolated Values

## Context

This issue is a warning that outlines an attempt to access state that is isolated to the main actor from a different, non-isolated part of the code. This is unsafe and throws a compile time error in Swift 6. 

To fix the issue, make the property or initialiser non-isolated if it doesn't need to access, or manage the isolation by accessing within an async call that 'switches to' the main actor and resolves any potential data races. 

There are a few sub-queries from there, such as:

- What is an Actor?
- What is a data race?

### Actors and Isolation and Data Races

#### Actors 

Actors are ways to manage concurrency and synchronisity. You can use actors by creating a 'namecontroller' which controls access to that property. This then allows you to moderate requrests that come in through the controller. I think this is somewhat similar to adding in getters and setters to properties (in this one instance?). 

I think more broadly this would be like creating a seperate class object which contains the methods that you would want to call. 

Actors are reference types, that mean that multiple copies of the same object will refer to the same data at the same memory address. Hence, if you init a new actor and modify some underlying values, this will modify that value for all instances of that actor.

However, actors are not classes and do not support inheritance. I think you do inheritance in Swift, byt simply adding the class name after the declaration but before the methods. Kinda like in other languages I suppose, but it looks a bit weird.

Actors effectively create threatsafe get/set methods that different functions and threads can use to access the data. I think the application effectively enforces some sort of access methodology or gate so that things must queue up to get at the data.

You MUST call your actor's methods asynchronouosly. This allows for the threadsafety ensuring magic under the hood to take effect. This is done through the SWIFT standard lib.

##### Main Actor

The Main Actor is a type of Global Actor that is only acessible from the main thread? But you do have to call it from an async context like usual.

Global Actors are effectively Singletons, you can only have the one instance of them. You effectively just decorate them with `@globalactor` and then the back end wizardry happens.



#### Data Races

Data races occur when the same parts of the memory are accessed from multiple threads without syncronisation and at least one of those accesses is a write action. This can lead to crashes and memory corruption. These are unpredicatable in general and so can lead to crashes that only happen sometimes and can be hard to both test for and reproduce.

This might include writing and updating the same variable without any concurrency or blocking functions. Each method is trying to do the same thing at the same time and so it explodes.

You can work through this issue once you have identified the data race. This can include lock queues and or dispatch queues. This will ensure that you can keep things seperate to a degree.

Wrapping one of the calls (to the same var) in a `lockQueue.async {}` will ensure that access is synchronised and that only one thread at a time is accessing the named variable.

That said, using actors is probably going to be better at least according to this nice man on the internet.

### Default Values


### Conflict

## Sources

1. https://www.avanderlee.com/swift/thread-sanitizer-data-races/#what-are-data-races
2. https://www.avanderlee.com/swift/actors/
3. https://www.avanderlee.com/swift/mainactor-dispatch-main-thread/
