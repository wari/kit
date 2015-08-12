# Go kit [![Circle CI](https://circleci.com/gh/go-kit/kit.svg?style=svg)](https://circleci.com/gh/go-kit/kit) [![Drone.io](https://drone.io/github.com/go-kit/kit/status.png)](https://drone.io/github.com/go-kit/kit/latest) [![Travis CI](https://travis-ci.org/go-kit/kit.svg?branch=master)](https://travis-ci.org/go-kit/kit) [![GoDoc](https://godoc.org/github.com/go-kit/kit?status.svg)](https://godoc.org/github.com/go-kit/kit)

**Go kit** is a **distributed programming toolkit** for building microservices
in large organizations. We solve common problems in distributed systems, so
you can focus on your business logic.

- Mailing list: [go-kit](https://groups.google.com/forum/#!forum/go-kit)
- Slack: [gophers.slack.com](https://gophers.slack.com) **#go-kit** ([invite](http://bit.ly/go-slack-signup))

## Documentation

### Examples

Perhaps the best way to understand Go kit is to follow along as we build an
[example service][examples] from first principles. This can serve as a
blueprint for your own new service, or demonstrate how to adapt your existing
service to use Go kit components.

[examples]: https://github.com/go-kit/kit/tree/master/examples

### Endpoint

Go kit primarily deals in the RPC messaging pattern. We use an abstraction
called an **[endpoint][]** to model invidivual RPCs. An endpoint can be
implemented by a server, and called by a client. It's the fundamental building
block of many Go kit components.

[endpoint]: https://github.com/go-kit/kit/tree/master/endpoint/endpoint.go

#### Circuit breaker

The [circuitbreaker package][circuitbreaker] provides endpoint adapters to
several popular circuit breaker libraries. Circuit breakers prevent thundering
herds, and improve resiliency against intermittent errors. Every client-side
endpoint should be wrapped in a circuit breaker.

[circuitbreaker]: https://github.com/go-kit/kit/tree/master/circuitbreaker

#### Rate limiter

The [ratelimit package][ratelimit] provides endpoint adapters to rate limiting
packages. Rate limiters are equally applicable to both server- and client-side
endpoints. Use rate limiters to enforce upper thresholds on incoming or
outgoing request throughput.

[ratelimit]: https://github.com/go-kit/kit/tree/master/ratelimit

### Transport

The [transport package][transport] provides helpers to bind endpoints to
specific serialization mechanisms. At the moment, Go kit just provides helpers
for simple JSON over HTTP. If your organization uses a fully-featured
transport, bindings are typically provided by the Go library for the
transport, and there's not much for Go kit to do. In those cases, see the
examples to understand how to write adapters for your endpoints. We have
examples for [Thrift][thrift], [gRPC][grpc], [net/rpc][netrpc], and
[JSON over HTTP][jsonhttp]. Avro and JSON/RPC examples are planned.

[transport]: https://github.com/go-kit/kit/tree/master/transport
[thrift]: https://github.com/go-kit/kit/tree/master/examples/thriftsvc
[grpc]: https://github.com/go-kit/kit/tree/master/grpcsvc
[netrpc]: https://github.com/go-kit/kit/tree/master/netrpcsvc
[jsonhttp]: https://github.com/go-kit/kit/tree/master/jsonhttpsvc

### Logging

Services produce logs to be consumed later, either by humans or machines.
Humans might be interested in debugging errors, or tracing specific requests.
Machines might be interested in counting interesting events, or aggregating
information for offline processing. In both cases, it's important that the log
messages be structured and actionable. Go kit's [log package][log] is designed
to encourage both of these best practices.

[log]: https://github.com/go-kit/kit/tree/master/log

### Metrics (Instrumentation)

Services can't be considered production-ready until they're thoroughly
instrumented  with metrics that track counts, latency, health, and other
periodic or per-request information. Go kit's [metrics package][metrics]
provides a robust common set of interfaces for instrumenting your service.
Bindings exist for common systems, from [expvar][] to [statsd][] to
[Prometheus][].

[metrics]: https://github.com/go-kit/kit/tree/master/metrics
[expvar]: http://golang.org/pkg/expvar
[statsd]: https://github.com/etsy/statsd
[Prometheus]: http://prometheus.io

### Request tracing

As your infrastructure grows, it becomes important to be able to trace a
request, as it travels through multiple services and back to the user. Go
kit's [tracing package][tracing] provides enhancements for your endpoints and
transport bindings  to capture information about requests and emit them to
request tracing systems. (Currently, [Zipkin][] is supported; [Appdash][]
support is planned.)

[tracing]: https://github.com/go-kit/kit/tree/master/tracing
[Zipkin]: https://github.com/twitter/zipkin
[Appdash]: https://github.com/sourcegraph/appdash

### Service discovery and load balancing

If your service calls another service, it needs to know how to find it,  and
should intelligently spread its load among those discovered instances. Go
kit's [loadbalancer package][loadbalancer] provides client-side endpoint
middleware to solve that problem,  whether your organization uses static hosts
or IPs, [DNS SRV records][dnssrv], Consul, etcd, or Zookeeper. And if you use
a custom system, it's very easy to write your own [publisher][] and use Go
kit's load balancing strategies. (Currently, static hosts and DNS SRV are
supported; Consul, etcd, and Zookeeper support is planned.)

[loadbalancer]: https://github.com/go-kit/kit/tree/master/loadbalancer
[dnssrv]: https://github.com/go-kit/kit/tree/master/loadbalancer/dnssrv
[Publisher]: https://github.com/go-kit/kit/tree/master/loadbalancer/publisher.go

## Motivation

Go has emerged as the language of the server, but it remains underrepresented
in large, consumer-focused tech companies like Facebook, Twitter, Netflix, and
SoundCloud. These organizations have largely adopted JVM-based stacks for
their business logic, owing in large part to libraries and ecosystems that
directly support their microservice architectures.

To reach its next level of success, Go needs more than simple primitives and
idioms. It needs a comprehensive toolkit, for coherent distributed programming
in the large. Go kit is a set of packages and best practices, leveraging years
of production experience, and providing a comprehensive, robust, and trustable
platform for organizations of any size.

In short, Go kit brings Go to the modern enterprise.

For more details, see
 [the motivating blog post](http://peter.bourgon.org/go-kit) and
 [the video of the talk](https://www.youtube.com/watch?v=iFR_7AKkJFU).
See also the
 [Go kit talk at GopherCon 2015](https://www.youtube.com/watch?v=1AjaZi4QuGo).

## Goals

- Operate in a heterogeneous SOA — expect to interact with mostly non-Go-kit services
- RPC as the primary messaging pattern
- Pluggable serialization and transport — not just JSON over HTTP
- Operate within existing infrastructures — no mandates for specific tools or technologies

## Non-goals

- Supporting messaging patterns other than RPC (in the initial release) — MPI, pub/sub, CQRS, etc.
- Re-implementing functionality that can be provided by wrapping existing packages
- Having opinions on deployment, orchestration, process supervision, etc.
- Having opinions on configuration passing — flags, env vars, files, etc.

## Contributing

Please see [CONTRIBUTING.md]. Thank you, [contributors]!

[CONTRIBUTING.md]: /CONTRIBUTING.md
[contributors]: https://github.com/go-kit/kit/graphs/contributors

## Dependency management

Users who import Go kit into their `package main` are responsible to organize
and maintain all of their dependencies to ensure code compatibility and build
reproducibility. Go kit makes no direct use of dependency management tools like
[Godep](https://github.com/tools/godep).

We will use a variety of continuous integration providers to find and fix
compatibility problems as soon as they occur.

## API stability policy

The Go kit project depends on code maintained by others.
This includes the Go standard library and sub-repositories and other external libraries.
The Go language and standard library provide stability guarantees, but the other external libraries typically do not.
[The API Stability RFC](https://github.com/go-kit/kit/tree/master/rfc/rfc007-api-stability.md)
 proposes a standard policy for package authors to advertise API stability.
The Go kit project prefers to depend on code that abides the API stability policy.

## Related projects

Projects with a ★ have had particular influence on Go kit's design.

### Service frameworks

- [go-micro](https://github.com/asim/go-micro), a microservices client/server library ★
- [gocircuit](https://github.com/gocircuit/circuit), dynamic cloud orchestration
- [gotalk](https://github.com/rsms/gotalk), async peer communication protocol &amp; library
- [Kite](https://github.com/koding/kite), a micro-service framework

### Individual components

- [afex/hystrix-go](https://github.com/afex/hystrix-go), client-side latency and fault tolerance library
- [armon/go-metrics](https://github.com/armon/go-metrics), library for exporting performance and runtime metrics to external metrics systems
- [codahale/lunk](https://github.com/codahale/lunk), structured logging in the style of Google's Dapper or Twitter's Zipkin
- [eapache/go-resiliency](https://github.com/eapache/go-resiliency), resiliency patterns
- [FogCreek/logging](https://github.com/FogCreek/logging), a tagged style of logging
- [grpc/grpc-go](https://github.com/grpc/grpc-go), HTTP/2 based RPC
- [inconshreveable/log15](https://github.com/inconshreveable/log15), simple, powerful logging for Go ★
- [mailgun/vulcand](https://github.com/mailgun/vulcand), programmatic load balancer backed by etcd
- [mattheath/phosphor](https://github.com/mattheath/phosphor), distributed system tracing
- [pivotal-golang/lager](https://github.com/pivotal-golang/lager), an opinionated logging library
- [rubyist/circuitbreaker](https://github.com/rubyist/circuitbreaker), circuit breaker library
- [Sirupsen/logrus](https://github.com/Sirupsen/logrus), structured, pluggable logging for Go ★
- [sourcegraph/appdash](https://github.com/sourcegraph/appdash), application tracing system based on Google's Dapper
- [spacemonkeygo/monitor](https://github.com/spacemonkeygo/monitor), data collection, monitoring, instrumentation, and Zipkin client library
- [streadway/handy](https://github.com/streadway/handy), net/http handler filters
- [vitess/rpcplus](https://godoc.org/code.google.com/p/vitess/go/rpcplus), package rpc + context.Context
- [gdamore/mangos](https://github.com/gdamore/mangos), nanomsg implementation in pure Go

### Web frameworks

- [Beego](http://beego.me/)
- [Gin](https://gin-gonic.github.io/gin/)
- [Goji](https://github.com/zenazn/goji)
- [Gorilla](http://www.gorillatoolkit.org)
- [Martini](https://github.com/go-martini/martini)
- [Negroni](https://github.com/codegangsta/negroni)
- [Revel](https://revel.github.io/) (considered harmful)

## Additional reading

- [Architecting for the Cloud](http://fr.slideshare.net/stonse/architecting-for-the-cloud-using-netflixoss-codemash-workshop-29852233) — Netflix
- [Dapper, a Large-Scale Distributed Systems Tracing Infrastructure](http://research.google.com/pubs/pub36356.html) — Google
- [Your Server as a Function](http://monkey.org/~marius/funsrv.pdf) (PDF) — Twitter
