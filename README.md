# Spartacus -- a Domain-Specific API for Event-Driven "Fast Data" Architectures
> Spartacus seeks to lead the revolution to free developers and runtime functions enslaved by distributed synchronous call stacks; and achieve independent, evolutionary, parallel processing via highly available containerized services choreographed through existing local or distributed Key/Value Stores (aka KV Stores), and their ensuing SLAs.
> 
> Spartacus offers a "Domain Specific" API for EDA application portability across clouds and technologies.
> 
> Date: March 15, 2021
> Author: George Willis

### Gratitude
Thanks to Duke Energy, in particular Steve Neglia, Russ McIntire, Kevin Fullington, Erick Enslein, Charlie Ward, Shawn Olds, Rose Glenn, and the whole of the Modern Architure team.  Your friendships are eternal.  To Rose in particular I answer -- "This is what's next."

Thanks to UPS, in particular Andy Dotterwiech and Ed Nedrotiwicz for championing quality in all our endeavors.  I loved every moment at the office because of the comradery of the teams.  It remains a career highlight to have been selected for the SWAT team (a forerunner of Site Reliability Engineering.)

Thanks to Federated Systems Group, and the opportunities in organic workflow blended with EAI, BPM and LDAP architectures.  A time cut short by a massive budget error, and ensuing "downsizing".  Still, no regrets -- the experience was excellent.

To countless others at Albertsons, Logic Trends, Catalina Marketing, IBM, and Cap Gemini who have given me voice and support -- my gratitude.

### Motivation
Their is a "Copernican Shift" on the horizon.

As DOS command lines gave way to Windows Graphical User Interfaces (that will yet give way to AR/VR).

As networks transformed "Personal" Computers into "Server Farms", and Mesos-like Kubernetes technologies tranform clusters into federated "Data Center Operating Systems"...

As inter-host VM isolation/segmention models are giving way to Container multitenancy models at namespece fidelity or greater...

The next megatrend will be Public and Private Hybrid Cloud FaaS.

It is beyond the intent of this paper to explain why this megatrend will come to pass.  I have produced other media to detail the virtues and rationale, as have others such as the Gartner Group (search "event Thinking"), Congruent, creaters of Kafka, and AWS (ala "lambda" web services).  

**Kubernetes has a roadmap to Serverless Streaming and DLT applications.**  Don't believe me, go check out the [CNCF](https://landscape.cncf.io/serverless).  This is the same group the governs Kubernetes and K-native, and if you don't understand why a log and a ledger are both order stores of events with different encryption standards then you haven't been keeping up with FinTech.  (Now lattice blockchains and PACT -- that is some interesting DLT.)

IBM has OpenWhisk and Hyperledger.  Google and Azure have "Functions".  There's Kafka clouds, AWS lambda and several other offering all centered around **the Hollywood Principle** -- "Don't call us, we'll call you."  Like an "Inversion of Control (IoC)" pattern for invocation, the core capability to publish "events" to topics and have multiple, choreographed "hook" events in these stream enable not only "Fast Data" application (aka Streaming applications), but also general purpose data processing at at both velocity (both development and operational) and scale.  The formal name for this pattern in "Implicit Invocation", and it stands in contrast to "Explicit Invocation".

You may encounter pundents who claim that such Event Driven Architecture (EDAs) are only appropriate for a subset of computing.  That it works in the public cloud, but not private "On Premise".  You may hear that long running transations are a problem.  They are merely a class of processing -- somewhat "organic".  You may hear it will only work with stateless processing, but a study of Martin Freemann's work will point the way to stateful processing (on another roadmap). 

You may also hear somebody proclaim that a pub/sub "implicit" invocation with a single subscriber will always be slower than explicit direct invocation.  But nothing prevents this or countless other optimizations to be both statically and dynamically linked into a system where invocations are now laid bear as business processes and workflows.  These pundents should have learned from the legacy of the Apple Dylan language that could statically link the actual concrete class methods in an otherwise dynamically-linked language.

The advantages in availability, density, security, velocity, evolutionary development, quality and simplicity bring sanity and support to sprawling complexity and technical debt.  **Embarrassingly Parellelized Computing!**  This is now my life's work in IT -- my "Opus".

Gartner identified "Event Thinking" as the #1 obstacle to EDA adoption.  I believe #2 is a standard development interface for Event Driven applications -- a "Domain-specific" language for EDA, kinda like SQL for databases.  This achieve simplicity of development, and portability of code.  

The goal is to create service adaptors for existing distributed or local key/value stores to handle the heavy lifting of consensus, event storage, and event publication.  This leaves Spartacus in oversight of event filtering, log cursors, and parallelized polyglot invocation of event consumers.

#### The scope is small, the impact immense.

Develop to the API, host on any local or distributed Key/Value store and reap the underlying SLAs.  Or develop your own censensus-driven KV store using Kafka, RabbitMQ, Raft, Paxos, etcd, RocketDB -- whatever you want, but it is simpler to use existing KV stores like FoundationDB, Amazon S3, Cassandra, Ceph, -- the list is endless!  Spartacus sits on top of a KV store and achieves an Event-Driven Cluster Operating System.

A major goal is simplicity.  Even for EDA disciples, the hardest part is getting started in a landscape of DLT and log technology where hard and enduring choices seem to be made up front.

Spartacus seeks to lead the revolution.

### Roadmap
* [X] Create Draft of API
* [ ] Format API into Swagger (OpenAPI)
* [ ] Build Rust implementations (No GC interrupts!!!)
* [ ] Build KV Adapters (KV CRUD interface)
  * [ ] localhost KV -> RocketDB
  * [ ] FoundationDB
  * [ ] S3

## Model

### Topics
> **Hierarchical topology** where branches aggregate the stream.  I believe Apache Pulsar has this built in.
##### Actions
* CRUD
    * **C**reate
    * **R**equest (Metadata)
    * **U**pdate (Metadata)
    * **D**elete
##### Attributes
* Name
* ParentTopicName
* Tags

### Events
> Standardized format for header -- open format for payload
##### Actions
* Validate(schema)
* Publish(topic)   //persist then broadcast
* Request(Criteria, format)
##### Attributes
> Polymorphic to HTML Head/Body
* Header
    * AuthorPID (PII Vault)
    * EventID
    * TopicName
    * Workflow
        * ID
        * Seq (counter)
        * Context (aka "State Transfer")
    * Tags
        * *Binary TagIDs that leverage bitmap* comparison for fast filtering
    * Timestamps (value ordered KVs)
        > User-defined KPIs  
        >
        Raft Example: 
        1. Queued (EventID assigned)
        2. Assigned (Event Data assigned)
        3. Published (Majority Acknowledgement)
        4. Committed (Released)

* Body (Payload)
    > **Use whatever Event Typing and Schema Version Control standards you choose -- that's the value of the open format**
    > 
    > Many use Avro...

### Agents
> Standardized Agents/Hooks that rapidly filter topical event stream and invoke worker process/thread/coroutine
##### Actions
* Create/Request/Delete (CR_D)
* Pause/Continue      *(Invocations)*
##### Attributes
* TopicName
* Criteria
* InvocationNames     *(many)*
* EventID             *(cursor)*

### Criteria (Conditional Filter Algebra)
>Boolean Expression for Filter comparisons

### Filters
>Tag-based fast comparison (inclusion/exclusion) via bitmask matching
##### Actions
* match(Event.Header.Tag)
##### Attributes
* Name (unique)
* ID (unique integer)
    *  2 to the power of ID = bitmask

### Invocations
>Place reference/value of an event as the input to a (staged worker pool, where worker is a) thread/process/coroutine -- an **event consumer** (ala reentrant/idompotent service).
>
>This supports a strategy pattern with a high degree of reuse.
##### Actions
* Run
##### Attributes
* Name
* Type
* WorkerCount


