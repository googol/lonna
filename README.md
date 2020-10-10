## Lonna

Experimental FRP library for Graphical User Interfaces. To be used in my [Harmaja](https://github.com/raimohanska/harmaja) project.

Compared to Bacon.js

- Stateful Properties and EventStreams have an explicit `scope` which guarantees that they stay up-to-date for the required period. Scope can be global (active forever) or more limited, such as a GUI component lifetime. Access outside the explicit scope will cause thrown Errors.
- `EventStreamSeed` and `PropertySeed` abstractions for streams and properties that don't have a `scope`. These can be mapped, filtered etc, but need to be scoped before accessing the value. They are very lightweight which may prove to be a major performance improvement: when creating temporary streams in flatMap, for instance, there's no need to create a Dispatcher for everything.
- The `scope` decouples subscribers from EventStream/Property activation. This means that "cold observables" like `Bacon.once` cannot be EventStreams in Lonna. They will be `EventStreamSeeds` (what should I call it?) and can be used in constructs like `flatMap` as intermediary Observables.
- Because the `scope` guarantess freshness, the `get()` method in Property and Atom can be used for reliable synchronous access to current value. Also, a Property *always* has a value.
- More flexible Property interface that makes it easy to create properties that, for instance, extract their current value from the DOM or other external synchrnous source. 
- No Error events. I've found them quite counterproductive in my use cases. 
- No End events for now. This is something to include if this will be used as a general purpose FRP library.
- Simpler dispatch system, no Atomic Updates. However, when decomposition using map/combine is stateless which in itself guarantees that composed/decomposed state remains in sync
- Atoms included for read-write state, similarly Bus for read-write streams. Rudimentary lens system included for decomposing state.
- API consists of static methods instead of prototype methods. Is tree-shakable and easier to extend.

Current prioritized goals:

- Implement just enough operators for applying in real-life projects
- Validate / improve API ergonomy based on the above
- Decide on whether to make this an actual library
- After that there will be technical debt to pay, to get to a reasonable level of quality

TODO list of Bacon.js methods for first real-life use case:

- `throttle` WIP: buffer implemented for it!
- `changes`
- `debounce`
- `combineTemplate`
- `fromPoll`

To complete Harmaja examples, we also need

- `repeat`: would require stream end event
- `flatMapLatest`
- `fromEvent`
- `update`
- `once`
- `fromPromise` - needs to map into a tri-value thing: pending-error-value

Other stuff:

- Stream end events may be in order, for a more complete/general-purpose FRP library. Without this, for instance, a never-ending flatMap with will just keep collecting more stream references. 
- More decent testing framework and proper coverage. Bacon has a good foundation for this stuff. At the moment this is more an experiment than a library for production use.
- Make the API pipeable
- Bus should probably buffer pushes while dispatching. Subscriptions while dispatching should not be a major problem because of scoping and stateless composition. (to be proven)
