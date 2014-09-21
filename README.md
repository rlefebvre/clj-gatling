# clj-gatling [![Build Status](https://travis-ci.org/mhjort/clj-gatling.png?branch=master)](https://travis-ci.org/mhjort/clj-gatling)

Create and run performance tests using Clojure. For reporting uses Gatling under the hood.

Note! Currently this is more of a proof-of-concept and lacks lot of features.
The integration to Gatling is also far from perfect.

## Installation

Add the following to your `project.clj` `:dependencies`:

```clojure
[clj-gatling "0.0.6"]
```

## Usage

### Custom functions

Custom functions are the most flexible way for implementing tests.
Functions will get a user id and a callback as a parameter
and they must call the callback when they have done their job.

Ideally your testing functions should be asynchronous and non-blocking
to make sure the performance testing client machine can generate as much
as possible load.

Currently clj-gatling has no timeout for custom functions.
Be careful to check that your function will timeout on error.

```clojure

(use 'clj-gatling.core)

(defn example-request [user-id callback]
  (future (println (str "Simulating request for user #" user-id))
          (Thread/sleep (rand 1000))
          (callback true)))

(run-simulation
  [{:name "Test-scenario"
   :requests [{:name "Example-request" :fn example-request}]}] 2)
```

### Non-blocking HTTP

This method uses asynchronous http-kit under the hood. 
Get request succeeds if it returns http status code 200.

```clojure

(use 'clj-gatling.core)

(run-simulation
  [{:name "Test-scenario"
   :requests [{:name "Localhost request" :http "http://localhost"}]}] 100)
```

## License

Copyright (C) 2014 Markus Hjort

Distributed under the Eclipse Public License, the same as Clojure.
