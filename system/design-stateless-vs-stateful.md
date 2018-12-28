When designing the distributed system/service with large scale, the design of stateless and stateful for the service is always a big discussion.

**State** usually refers to the information about the user/client, and when processing the client/user's request, the performed behaviors in server side depends on the state info.

## Stateless Design
**Stateless** means there is no 'sticky' knowledge/state about a request or transaction and the service always performs every request as it is first time to request.

UDP protocol is an example designed as the stateless, each 'transation' does not depends on the knowledge of previous sent package. While TCP considered as an example for the stateful design, in each transaction the protocol needs to be aware of what packages already received.

### Cons & Pros of Stateless Design
**Pros**:
* Scalability: The service instances could scale horizontally (scale-out)
* Load balance: Arbitrarily dispatch work load
* Debug-ability: Easy to debug with simply request replay
* DevOps: Much easy to do the trouble-shooting (might need to combine with the request 'pin' feature)
* Resource utilization: Usually more effecient
* Simplicity: Dramatically reduce complexity
* High concurrency

**Cons**:

For scenarios has (transaction) state info:
* It might heavily depends on the backend storage (might hit the limitation)
* High pressure on the storage service
* Larger latency
* Large traffic package size since it might needs to carry the state info during the request

### Scenarios
* CDN services
* Static web pages (some dynamic pages as well)
* Function as a service
* Short life-time request scenarios

## Stateful Design
**Stateful** means that what the previous transactions/requests done may affect the behavior for current transaction/request.

Usually, stateful design highly depends on a good DevOps system to make sure the whole cluster/system works well.

### Cons & Pros of Statefull Design
**Pros**:
* Data locality & High performance: the same client requests will hit the same server, and could reuse the cached data

**Cons**:
* Work load (load balance): Might be unbalanced, if too many clients' reqeust fall into the same server, that server might be overwhelmed
* Complexity: 
    * More complicated than stateless design
    * Service (re)placement
    * Routing logic
* Reliability: Less reliable
* DevOps: Hihger challenging for managing the full lifecycle

### Scenarios
* Transactions involved area, like online shopping
* Databases

## Stateless and Stateful Design
Whether the service designed as stateless or stateful is totally a trade-off, each design has its own advantages and disadvantages and good fit in different scenarios. However, they are not really exclusive to each other, in some scenarios we need to combine both. Take HTTP and TCP as an example, TCP is transaction-oriented 'stateful' design while HTTP is more like a stateless design based on the TCP.

> TODO details for stateful design and related trade-off solutions

References:
* ["Building Scalable Stateful" by Caitie MaCaffrey](https://www.youtube.com/watch?v=H0i_bXKwujQ)