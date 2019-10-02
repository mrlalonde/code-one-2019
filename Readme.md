# Code One write up

## Key take away

- Kotlin is already a great language to safely explore on the JVM.  Years ahead of Java.
- Explore reflection-less frameworks and other tech for a cloud native experience
    - Micronaut
    - Quarkus
    - Microprofile
    - GraalVM
- Explore Reactive Stream API
    - Flow in Java9
    - Reactor impl
    - RxJava impl?
- Microservices?
- Java modules are not that scary but maybe not that compelling either?!
- Explore CompletableFuture via hands on tutorial(s)

## Monday

### 12 Cool things you can do with JVM Languages

by Vankat Subramanian

1. _Files in Groovy  & Scala_, I thought this was less convincing given improvements in Java 7

2. _Processes in Groovy_, Handy for build automation. Example ``` 'git -help'.execute()

3. _Execute around method patter in Java8_, 
this was great!  Example ``` ResourceType.using(Consumer<ResourceType> consumer)```, 
which forces the usage to be done correctly so that the resource is released.  This is
bullet proof, unlike try-with-resources.

4. _Traits in Scala_, This was interesting for *stackability* and mixins.
I wouldn't pay the high complexity cost of Scala for this though.

5. _Joining Strings in Java8_, Already use that one regularly :-)

6. _Lazyness in Kotlin and Scala_, This was neat.  Example in scala: ``` lazy val temp ="value"```, then use **value** and it will only be computed then and once

7. _Pattern matching in Kotlin_, Already use that. :-)

8. _Auto-casting in Kotlin_, Already use that. :-)

9. _Creating xml in Groovy_, already knew but this was a good reminder.  Kotlin works too with a slightly different approach...

10. _Tail call optimization in Kotlin!_ This means that Kotlin
can be used for Functional idioms without paying a big performance penalty or blowing the call stack!

11. _Extension methods in Kotlin_, knew that! :-)
12. _Fluency in kotlin with infix functions_, knew that but  it was a good reminder

#### Keynote: The future of Java
- cool Quantum Computing brief
    - Quantum Computing could change everything!
    - Some Java API existing for simulations.
    - Current impl are ~ 50 Qbits, need millions to revolutionize
    - Programming requires low level Quantum gates


- checkout  Java updates
    - new garbage collector ZGC
    - Project Panama, vector operations and native integration
    - Project Loom: Continuations/Fibers, a la Coroutine
    - Project Amber: Syntax add-ons
    - Project Valhalla: Mechanical Sympathy, inline Classes and records (a la C struct)
    
#### Migrating to Java Modules: Why and how?
by Venkat Subramanian

Concepts:
- Classpath vs Modulepath
- Module types
    - Unnamed
    - Automatic
    - Explicit
- Design for backwards compatibility
- Can migrate gradually
    - use manifest file at first to better name automatic modules
    - start at root and work your way to the leaf nodes!
    
 (*) Did not come out with good notes for the why.  Here is my best guess
 - Better way modularize functionality
 - Smaller deployments
 - Potential for cleaner architecture

### Kubernetes: Faster and easier
by Ray Tsang

- Things to look up:
    - JDeffered
    - WireMock
    - Spring Cloud Contract
    - Test Containers
    - saturnism.me (for Docker best practices)
    - Jib (for creating images for Java apps)
    - scaffold (iterative approach to deploy in dev mode a la hot reload)
    - kustomize (patches for each env)
    - Cloud Foundry's memory calculator
    
 - User Java 8u192 or above, or else memory and threads calculations will be wrong.  Dying containers could ensue!
 - learn best practices for 
    - liveness probe
    - readiness probe
    - preStop endpoint, consider running mgmt on diff port

### Java for the Microservice and serverles era

- Java can be fast and nimble if we work around typical framework problems
- Done it for Android
- Newer frameworks do it:
    - Quarkus
    - Micronaut
    
- The key is eliminating reflection and runtime dynamic proxies and using AOT instead
    - reduces code deployment size
    - reduces memory consumption
    - faster start up time
    
 - Quarkus prohibits reflection, violates some spec
 - Micronaut uses an @Introspected annotation to trigger AOT
    - supports bean intro-spection
    - supports even AOP!
    
#### Performance and Scalability of RDBMS with Oracle JDBC driver
- RSI (Reactive Stream Ingest), supports direct path!
- JSON type support
- Proprietary async extensions
- Easy ConnectPlus
    - tcps
    - HA properties
 - Client side result set cache
 - use of UCP (Universal Connection Pool)
 - JDBC Sharding driver
 
## Tuesday

### Event Driven Tutorial
- book: Migrating to Microservices DB
- https://bit.ly/eda-tutorial
- **Code is easy, state is hard**,
    - Latency
    - Scalability
    - Performance
    - Cache & polling?
    - Eventual Consistency
 - History: ORM -> Pojos as (anemic) Domain Model
 - Event Sourcing <-> Domain Driven Design
    - Model transactions as events
        - Good for writes
        - Challenge for reads
        - Free audit log!
 - Event Storming: Event-first design approach
    - Think about events that happen in the system (_before the structure_)
 - CQS: Command Query Separation, Bertrand Meyer
 - CQRS: Command Query Responsibility Segregation
    - Distribution
    - Availability
    - Integration
    - Analytics
 - Events -> Facts that happened in the system, immutable!
 - Design trade offs: Low level vs Domain events
 - Domain Events (eg. AccountUpdated)
    - coupling is type and semantic of events
    - burden is on the producer
    - easy for consumer
    - need to understand domain well
    - domain can't change too quickly
 - Low level events (eg. Update:Account):
    - coupling is in the schema of the events
    - burden is on the consumer
    - easy for producer
    - more flexible
  - Hybrid approach: use low level events but extract domain events as we learn more
  - Tech to explore:
    - Debezium (implements Outbox pattern to replicate DB writes as events, doesn't work with Oracle though..)
    - Microprofile
    - RabbitMQ
    - Compare Quarkus and Micronout for event processing
- checkout Reactive Stream spec https://reactivestream.io

### Effective Typescript

User Typescript Playground online for quick and easy explorations!

### Completable Future: The Promise of Java
by Venkat Subramanian

- Like Promise in JavaScript
- 3 stages:
    - Pending
    - Rejected
    - Completed
        - Success
        - Error
 - failure/error as data
 - composability!
 - smart execution policy, may use main thread if non-blocking
 - always use a timeout if you have to block!
 - TO EXPLORE!


### Continuous Delivery with Docker and Java: The good, the bad and the ugly
by Daniel Bryant

- containers may amplify issues in application
- Books: Containerization and Continuous Delivery in Java
- velocity + stability is key
- CI will help provide it if done well
- The good:
    - dev experience
    - repeatable builds
    - seals libs and app server
- The bad:
    - size constraint
    - feedback loop w microservices can take a hit
    - app can get slow or freeze (watch for randomness issue, not enough entropy!)
 
 - Practices:
    - Build Binaries Only Once BBOO
    - Make dev as close as possible to prod:
        - spin up container in dev
        - same base image
    - Dockerfile content
        - use alpine with JRE, not JDK
    - checkout jlink
    - watch image size
        - mvn dependency:tree and dependency:analyze
        - wagoodman/dive, tool for analyzing layers content
    - building in containers, multi-stage FTW
    - BuildKit, caching for maven artifact
    - Telepresence to proxy into Kube (debug ports)
    - Metadata is valuable
        - Signing
        - build details and label
        - checkout Grafeas and Kritis
    - checkout blog.hasura.io
    - perf and load tests:
        - Gatling, JMeter, Flood.io
    - Security:
        - FindSecBugs
        - DockerBench
        - Bold-Security
        - Gauntlt ServerSpec?
        - owasp mvn plugin
        - Clair for scanning images
    - Speed, use latest Java
        - AOT
        - ACDS Application Class Data Sharing
    - Entropy issues with /dev/random, use /dev/.urandom instead (app freezes)


##Wednesday

### Drinking from the Stream: Messaging Platforms for scalability and performance

by Mark Heckler @mheck

- Showed Spring Cloud Stream (SCS)
    - Source -> Processor -> Sink model
    - Abstracts away messaging platform

(*) Interesting but I prefer exploring reflection-less alternatives based on Eclipse MicroProfile.
    
### JDBC Scalability and Async
- JDBC driver (v20C) from Oracle will work well with project Loom's Fibers
- threads are expensive to create and maintain
- Fibers are lightweight
- IO bound thread can do more work in parallel with Fibers
- Oracle killed ADBA spec, queuing requirements were too difficult to implement
- **TODO** Look up Flow in Java 9
- JDBC supports async and non-blocking and is compatible with Reactive Streams spec:
    - concurrent calls to same Connection object will block and not get queued
 
### The value of reactive systems
by @smaldini from Spring Reactor

- harder to debug: stacktraces!
- mutliple terminal routes
- blocking is hard to avoid
- extra-overrides in allocations (Flux, Mono wrappers...) and indirections (pipeline of lambdas)
    - queues
    - signals
    - volatile
- Priorities?
    - Time to market
    - Observability
    - Running costs, efficiency -> Reactive?
    
- Orders of magnitude more efficient!!
    - threads
    - memory pressure
    - hardware spec
 - Instead of Spring MVC, try Webflux/Netty, Reactive is...
    - less than 10x number threads
    - less memory
    - more in younger Gen for heap so faster and less GC
    - better tail latency (99th percentile)
    - **Designed for connection volume scalability**
    - lower impact for long connections (HTTP2, WebSockets...)
    - resilient under stress and lower maintenance (OPS :-))
 - HTTP status as back pressure (405?)
- look up Netflix patterns:
    - Remote calls parallelization
    - Hedge Clients
    - Spring Cloud Gateway
    - RSocket protocol

###Decompose your monolith

Strategies for migrating to microservices

**checkout** https://microservices.io

-> Chris Richardson @crichardson (Eventuate)

Incremental approach to migrating to microservices

Code for book: "ctworacleone19" for Microservices Pattern book


#####Why Microservices?

Rapid, frequent & reliable delivery and innovation.

As size goes up, good attributes of maintenance and sustainability go down...

**Architectural Style**
- App is  made up of services
- maintainable, testable
- loosely coupled
- ...

------------------

- Testability and Deployability
- Modularity -> Sustainability

**Caution**
* Avoid if possible!
* Try to use monolith until you've exhausted options!
    * process
    * CI/CD...
    
* No big bang rewrite
* Strangler pattern: monolith shrinks over time
     * stop making the monolith larger
     * new features as services
     * Can use App gateway to "Strangle"
* Data is the hardest part
    * replication
    * change data capture (Debezium!)
* Module -> Service
    * untangle dependency
    * separate data
    * duplicate data to reduce scope of refactoring at first
    * bounded context?
* Metrics
    * Lead time: commit -> prod
    * Frequency of deployments:  number_of_devs / day!
* What to extract? Have a target architecture in mind.  Revise continuously!
    * rank modules by ROI
    
* Approach

1. Separate code in modules
2. Separate data
3. Deliver Service
4. User service (could use messaging if appropriate...)
5. Delete old code
        
(*) Remember, do monolith -> micro if you've outgrown it and have tried other fixes first like process and CI/CD.  It's a last resort.  Monolith is not inherently bad.  It's often best! :-O
    
### Living without REST...

Was mostly a rehash of the same event driven talk I attended earlier...

Can checkout book on microservices database migration: https://bit.ly/mono2microdb

## Thursday

### Getting rid of dirty code

*   @MarcusBiel
    * Ebook: https://cleancodeacademy.com


* A good exercise is to find re-factor the code online for a brute force Sudoku solver.
    * Consider trying it in Kotlin as well for an extra challenge.

* Revisit: https://www.sandimetz.com/99bottles (from QCON NewYork last 2018)

* Compare this with Refactoring and Clean Code.
* Need to understand the business in order to refactor business logic!
* Legacy code defined as:
    * useful
    * working
    * not enough tests
    
 **Methodology**
 * Need to do some simple safe refactors simply to develop the deeper understanding at first...
 * Baby steps; slow but steady
 * code experiments explore by testing snippets of legacy code

### Reactive Microservice in action

* Look up Microprofile spec: https://microprofile.io/
    * async calls
    * resilient patterns w annotations
    * context propagation
        * capture/restore
 * Threadpools
    * blocking calls swept under the rug
    * complex
    * prefer non-blocking IO
* Containers don'tlike too many threads (resource quotas)
    * memory pressure
    * CPU context switch
    * defeats purpose to have 200+ threads a la Tomcat
* Async + Non-Blocking -> Efficiency :-)
* **TODO** review https://www.reactivemanifesto.org/ and https://www.reactive-streams.org/
* Libs that implement the RS spec manage subscription & back pressure
    * RxJava
    * Reactor
    * Akka...
* Reactive Systems
    * Responsive
    * Resilient
    * Elastic
    * Message Driven (async msg passing)
* Reactive properties
    * Temporal Decoupling
    * Need Broker to persist
    * MicroProfile can help
    * see demos: https://github.com/Emily-Jiang/
    
### How to share busyness logic in a polyglot app?

* Introduced https://github.com/doov-io (Java & TS versions)
* DSL for validation and and transformation of "beans"

### Clean architecture
This was a AMA with a few "experts"...

Just review the book "Clean Architecture" by Bob Martin

### How do I test this mess?
* Presented by @scott_wierschem
    * KeepCalmAndRefactor.com
* Largely based on the book "Working Effectively with Legacy Code" by Michael Feathers.
    * Seams
    * Peel & Slice
* Check out Approval Tests: https://approvaltests.com/platforms/
    * suposedly good at generating test combos
* Checkout https://refactoring.guru
* Checkout git effort script (https://github.com/tj/git-extras/blob/master/bin/git-effort) to find out files that are most changed.

### Quarkus: Fast, small, innovative and native
* by @rafabene
* checkout developer.oracle resources
* Quarkus is based on MicroProfile spec
    * is Micronoat as well?
*  Has a Spring layer
* https://code.quarkus.io is a good place to generate starters...
* Read: https://developers.redhat.com/blog/2017/03/14/java-inside-docker/
* mvn plugin to bootstrap
    * lib folder separates deps from your app
        * better docker layer
 * ~ 1.4s start up time on recent OpenJDK
 * **Does every dependency need to be Quarkus-ized?** No unless they need reflection or such wizardry, then you need to write an extension for them.
