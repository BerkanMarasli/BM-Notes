<h1> 20 System Design Concepts </h1>

- Vertical Scaling - adding more resources (e.g. RAM, CPU) to a server 

- Horiztonal Scaling - duplicate replicas of the server. Can scale infinitely and dont need good machines. Adds redundancy and fault-tolerence (i.e. if a server goes down, other servers handle request, eliminating single point of failure in vertical scaling). More complicated approach.

- Load Balancer - a server that does reverse proxying. It directs an incoming request to a server. There are different algorithms used to direct the traffic of requests (Round Robin, Hashing) 

- Content Delivery Networks (CDN) - used when serving static content like (images, videos, HTML, CSS, JavaScript). Works by copying files from origin server to CDN servers across the globe. This can be done on a 'push' or 'pull' basis.

- Caching - create copies of data so that it can be refetched faster in the future. Networks requests, HHD fetching, RAN memory fetching can be expensive so data is copied to L1, L2, L3 CPU cache.



- IP Address - every computer is assigned a Internet Protocol Address (IP Address) which unqiuely identifies a device on a network.

- TCP/IP - internet protocol suite (IP, TCP, UDP). Set of rules that decide how we send data over the internet. For example, TCP sends data broken down into numbered packets over the internet to be reassembled later; if a packet is lost, TCP ensures the packet number missing is resent.

- Domain Name System (DNS) - centralised service that maps domain (URL) to IP Address.

- HTTP - TCP is too low level so we have a Application-Layer Protocol like HTTP/HTTPS that follows a client server model. Client initiates a request (request header, request body) and recevies a response (response header, response body).



<u>API Paradigms:</u>

- REST 

- GraphQL - can fetch multiple resources with a single request and you dont end up fetching data thats not actually needed.

- gRPC - considered a framework. Mainly used for server to server communication (gRPC Web looks for browser server communication). The performance boost comes from Protocol Buffers (vs JSON). Data is serialised into a binary format which is usually more storage efficient. But JSON is a lot more human readable.

- WebSockets - used in chat apps. If we wanted to implement a chat app using HTTP we would have to use Polling (where we periodically send a request) to check if there is a new message on the server. WebSockets support bidirectional communication so messages are pushed to devices instantely.



- Structured Query Language (SQL) - relational database management. They comply with ACID (Atomicity, Consistency, Isolation, Durability)

- NoSQL - consistency makes databases harder to scale so NoSQL has no consistency (primary/foreign key enforcement) and no relationships. Key value stores (DynamoDB), document stores (MongoDB), graph databases (neo4j).

- Sharding - because there is no consistency in NoSQL databases, we can break up our data and horizontally scale our databases which is known as sharding. This involves have a shard key to identify parts of the database.

- Replication

- CAP Theorem

- Message Queues - like databases as they have duarable storage and they can be replicated for redundancy or sharded for scalability. Used if the server is receiving data at a faster rate than it can process. Data is added to a queue and processed when resource is available. Different parts of the application become decoupled this way.



