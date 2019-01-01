In most scenarios, the service relies on some state when serving client's request. We either design the service/system as stateless or stateful.

## Conventional Stateless Solutions
We need to reply other technologies to hold the state info if we use stateless approach. There are several approaches:
- Solution 1: relies on the backend database as a whole (Data Shipping Paradigm)
    - Requirements:
        - Database can handle all data store read/write requests within reasonable latency
        - Each request might require to do a read from data to fetch the state
    - Pros:
        - Service implementation is simple, since the state store and data consistence offload to the backend database
        - Scalability (in terms of the service nodes)
        - Availability (in terms of service node), one bad/dead node could be transparently replaced
    - Cons:
        - Extra state data traffic, it needs to fetch the state info for each request
        - Higher latency
        - Lower performance
        - Backend storage might be a bottleneck, or single point of failure if a single conventional database
        - Not practicable for certain transactional requirement/scenarios
        - Lower utilization (Waste of resources)
- Solution 2: sharding/partitioning database
    
    As the number of client increase, the state storage might hit the limit of (single) storage in terms of latency and throughput. To resolve the problem, the trade-off solution would be either leverage high capability storage solution like NOSQL, or leveraging the sharding technologies.

    The NOSQL solution will requires more computing and storage resources. What is more, it will require extra complicated DevOps.

    Sharding/Partitioning solution also has several disadvantages. Firstly, it brings more maintenance works. Secondly, the application service might need to be aware of the partitioning/sharding, and the service would be like *stateless within partition and stateful cross paritions*. There is another optoin, you could wrap the sharding and partitioning into the data storage layer which run and exposed as a single service, then the application service does not need to know the partitioning. However, the data storage service will need to take care about it. That ends up just offload the work to data storage service only.

To sum up, stateless solutions always have the disadvantage of wasting resource and have higher latency in the scenario of having state info. If the business/application requires transactional logic, the stateless solution might not be able to meet the requirement.

## Stateful Solutions
Different from stateless solution, stateful solution will work with the local cache of the state and only take the database as the ground truth source. 

There are several obvious advantages:
- Lower latency, the local cache (Data Locality) could reduce the latency, especially for the data intensive application
- Higher availability, the service could keep serving while backend databasee down,since data is cached
- Better consistency (via sticky session)

Basically, there are several characteristics for stateful services:
- Sticky session
- Cached state

The differences of variants of stateful service solutions are mostly on the ways of building the sticky session. Basically there are two categories: persistent connection (*directly*) and routing logic (*indirectly*).
1. Persistent Connection (HTTP/TCP): the client connects to certain server directly.
    - Characteristics:
        - The client connets *'directly'* to the target serving node.
    - Cons/Problems:
        - Load balancing problems
            - Traffic/work load skew causes hot spots server or outage
            - Solution: implement the back pressure logic as the service protection mechanism
        - No stickness once connection break
    - Pros
        - Easy to implement
2. Routing logic: leverage *load balancing* to do the traffic routing. 
    - Characteristics:
        - The client doesn't connet directly to the target serving node.
    - Challenges
        - **Work Distribution**: How to know/determine which server should be routed to? (Determine how to move work throughout the cluster)
            1. Random Placement
                - What: Write anywhere and read from everywhere
                    - Not really sticky connection, but a stateful service
                - Scenario
                    - In-memory indexes and caches
                    - Usually applied in stateless scenarios
            2. Consistent Hashing
                - What: Deterministic placement
                - Scenario
                    - > Consistent Hashing & Random Trees: distributed caching protocols for relieving hot spots on the WWW
                    - Amazon DynamoDB
                    - Twitter's Manhattan database
                    - Cassandra
                - Problems
                    - Hot spots, requests are not evenly distributed, or evenly distributed but hash might fall into certain one and have hot node, or certain node downs and bunch of requests fall into the next one
                    - Work load not allow/easy to move
                        - Allocate enough/extra resources -- wasting resources
                        - Virtual nodes proposed in DynamoDB
            3. Distributed Hash Table
                - What: Try to resolve the problems of consistent hashing
                - Non-deterministic placement
                - How: allow to remap the traffic/request to another node
                    - Why
                        - Previous node overwhelmed
                        - Node dead/unavailable
                        - Rescale system
                        - Reshape cluster
                    - *How internal works??? (TODO)*
        - **Cluster Membership**: who should be the candidates to route traffic to?
            1. Static Cluster Membership
                - Problems
                    - Not fault-tolerant
                    - Need manually replace bad machine, operation is painful
                    - Expand/maintain cluster will be very painful and might need to take down the cluster services (impact the SLA)
            2. Dynamic Cluster Membership: add/remove the nodes on the fly
                - Pros
                    - Flexible and good for DevOps
                - How
                    1. Gossip Protocols -- Availability
                        - Application needs to tolerate the partition and temporary inconsistent view of cluster, or the work might be routed to different nodes
                        - Pros
                            - Fast
                    2. Consensus Systems (central place of the truth) -- Consistency
                        - Algorithms
                            - Paxos
                            - Raft
                        - Pros
                            - Strict strong consistent
                        - Cons
                            - Single point of failure, if consensus system not available temporary, work still be possible routed to different nodes
                            - Slow

Stateful design approach are widely applied in the distrbuted database or file system. 
> TODO Summary the tech and remapping different systems' categories

## Stateful Systems/Projects
1. [Scuba, Facebook](https://research.fb.com/wp-content/uploads/2016/11/scuba-diving-into-data-at-facebook.pdf): fast, scalable, distributed, in-memory databse
- What: random placement method
- Characteristics
    - Write
        - Random Fan-out write to any machine
    - Read
        - Random Fan-out request to all machines in the cluster
        - Compose result
        - Return result and completeness (how many percentage of the completenesss for the request)

2. [PingPop, Uber](https://eng.uber.com/intro-to-ringpop/), using swim gossip protocol (for cluster membership) + consistent hashing.

3. [Orlens, Microsoft](https://www.microsoft.com/en-us/research/project/orleans-virtual-actors/?from=http%3A%2F%2Fresearch.microsoft.com%2Fen-us%2Fprojects%2Forleans%2F), gossip protocol + consistent hashing + distributed hash table and actor model based distrbuted programming. Each actor would behavior as one/or all of following once recieved request:
    - Send new messages
    - Update its journals in its internal states
    - Create a new actors

## Lessons
* **Unbounded Data Structure**, implicit assumptions are the killer of distributed systems
* **Memory Management**, like making use of garbage collector profiler
    * Persist data for the lifetime, make objects in longer life and might take longer time/more expensive to GC
* **Reloading State**
    * Scenarios
        * First connection
        * Recovering from crashes
        * Deploying new code
    * Projects/Experience
        * Fast restart at facebook
            * (Planned restart) Migrate the memory state to new machine and switch to the new machine once it is ready, instead of fully reload state from storage and recovery/restore from there.

## Extension
> (TODO) 
> 1. Summarize more solutions for the dynamic routing, especially for the distributed database systems
> 2. Add more experiences and lessons

**References:**
* ["Building Scalable Stateful" by Caitie MaCaffrey](https://www.youtube.com/watch?v=H0i_bXKwujQ)
* [Making the Case for Building Scalable Stateful Services in the Modern Era](http://highscalability.com/blog/2015/10/12/making-the-case-for-building-scalable-stateful-services-in-t.html)
* [PingPop (from Uber)](https://eng.uber.com/intro-to-ringpop/)
* [Scuba (from Facebook)](https://research.fb.com/wp-content/uploads/2016/11/scuba-diving-into-data-at-facebook.pdf)
