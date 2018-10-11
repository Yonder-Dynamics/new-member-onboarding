# Redis

Redis is a very lightweight key-value store. Its intended use case is as an
external data store for services, which is the source of its name
("REmote DIctionary").

We will be using Redis as the core data store for the rover. Different rover
functionalities can exist as entirely separate programs with a high degree of
isolation, and communicate via Redis. For those wondering why this architecture
is important, read [The 12-Factor App](https://12factor.net/), paying close
attention to sections 4, 6 and 8.

Redis is packed full of features, but there a few particularly important
commands that will be used to string our rover's communications together. A
complete list of Redis commands can be found [here](https://redis.io/commands#list).

## GET and SET

`GET` and `SET` are the heart of Redis. `GET` retrieves the value associated
with the requested key. `SET` associates a value with a given key, overwriting
any previous existing value. These commands are useful with less interactive
rover systems which are not intended to be used in real time. For example, a
service which monitors RAM usage and CPU load might periodically `SET` some
keys in the rover core with updated values of these metrics. Services
interested in these metrics can view them by calling `GET`.

## PUBLISH and SUBSCRIBE

`PUBISH` and `SUBSCRIBE` allow modules to listen for all messages sent to a
specific topic. These commands are key to real-time rover systems, such as
joystick control. The concept of topics allows modules to communicate without
direct dependencies on one another. For example, the rover drive system control
module listens on the `joystick` topic. There is no specific other module which
must send messages to the topic, although `control-panel-api` is the standard
publisher for `joystick` messages originating from a joystick on the control
panel. This system can easily be integrated with the autonomous navigation
modules, which can then drive the rover by publishing their own messages to the
`joystick` topic. Communication and convention are key to preventing conflicts
when using the `PUBLISH`-`SUBSCRIBE` model.

## List Operations

`LPUSH` and `BRPOP` are used to operate on Redis lists. These lists are
concurrency-safe, which allows modules to access them freely, cutting down the
complexity of their usage. This model is useful for asynchronous, scaling jobs.
For example, the position solver for the robotic arm makes use of lists to
organize its work. A position generator, which converts control signals to
target arm positions, uses `LPUSH` to add a value to the left side of a Redis
list. The solver module calls `BRPOP`, removing a value from the right side of
the list, waiting if the list is emtpy. The position generator is much faster
than the position solver, and the use of lists allows the generator to complete
its work without having to synchronize (and wait for) the solver to process
each position. This model also allows for easy scaling. Multiple solver modules
could be started, and the semantics of Redis lists allow them to concurrently
wait for values and solve them in parallel. This way, multiple modules can be
used to split up work evenly, without risk of duplicate solutions. This form
of scaling could be leveraged with frameworks like Kubernetes, which can
dynamically increase the number of solvers to match the current workload.
