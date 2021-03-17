# LiquidScale Reactive Scopes

Reactive scopes are scalable stateful components that are used to model state in LQS clusters. By abstracting the state away from services and databases, we can create new breeds of interactions and simplify many modern use cases, including real-time state sharing, permissions and long-running workflows.

## What is a reactive scope?

Reactive scopes are managing arbitrary hierarchical data structure analogous to JSON objects. Scopes have many properties that makes them safe, scalable and efficient.

- Mutable only through mutators, applying incoming actions to produce a new state (reducer concept)
- Observable through secure subscriptions targeting explicit publications
- Powerful query capacity to extract portion of the state tree using various operators
- Many configuration like using a store to persist changes, authorization provider, etc.
- Scopes can be combined through mounted subscriptions at specific node in the state tree. This powerful reuse approach supports very complex models and performance control through complex authorization rules (filtering) and data caching.
- Scope state can be rendered to the exact state it was at a specific time. Support for future state projection is also supported (see time-morphing)
- Scope supports atomic transactions

## Rendering Context

Scopes state is rendered only when needed. Each time, the rendering context may be different, so state rendering must be very efficient, even more when external subscriptions are involved. Scopes completely abstract databases and external data sources, providing a unified, schema-based structure that can be consumed and/or modified by mutators, queries, sensors and effects.

So for each state rebuilding, we need to a rendering context containing the following fields:

- **root**: a JSONPath expression that will become the root of the rebuilt state. Might help optimizing rebuild (avoiding subscriptions rendering)
- **locale**: A full ISO locale used to render i18n strings as specified in the scope schema (optional)
- **reftime**: The reference time at which to rebuild the state. It must be a UNIX time (seconds since 1970).
- **version**: Define the schema version that should be used when rebuilding. By default, it will use the active schema at the given time
- **tag**: a simpler way to rebuild a well-known version/time of a scope. implies `reftime` and `version` when present.
- **actor**: Define who is rendering so that we can render only visible state and prevent some actions
