System design aims to build systems that are reliable, effective, and maintainable, among other characteristics.

## INTRODUCTION

### System Design 
    
    - Computer Netwroking 
    - Distributed System
        - Robust / Reliable
        - Scalable
        - Available
        - Resilency
        - Performance
    - Parallel Computing

### There’s no single correct approach or solution to a design problem.

### The sequence of steps to build large-scale distributed systems.
    
    1. Determine system requirements and constraints:
        Initially, clarify the functional and non-functional requirements of the system. Understanding the constraints is crucial as they influence the overall system design, including budgetary limitations and hardware specifications.
    2. Recognize components:
         Based on the requirements, select appropriate components, technologies, or APIs for the system design. This could involve choosing storage solutions, databases, or networking protocols that fit the system’s needs.
    3. Generate design:
         With the components identified, create a design that outlines how these components interact and work together to meet the system’s requirements. This includes the architecture and data flow diagrams.  
    4. Identify shortcomings in the initial design: 
        Review the initial design to spot any limitations or issues. This step involves critical analysis to find areas for improvement or alternative solutions that could enhance the system’s efficiency or performance. 
    5. Discuss trade-offs and improve iteratively:
         Finally, refine the design by discussing potential trade-offs. This involves balancing different aspects such as cost, performance, and scalability to optimize the design. Iterative improvements are made based on feedback and further analysis.


## Consistency

The data remains as close as to the data from source of truth will be more consistence data.


#### Consistency Types (in order of consistency)
    
    - Eventual:
        Least consistense spectrum doesn't gurantees the read will provide the data in sync.It ensures highly available DS EX- DNS resolver config , Cassandra
    - Causal
        Ensures the consistency only for the dependant operation or causally linked operations. EX- Commenting system
    - Sequential
        Ensures the consistency of a single process.IF multiple processes are running each process will have its own cache replica from the actual DB to have the cosnistency once the update operation is performed DB spawns the callout to each process cache. EX- News feed system. 
    - Strictly
        This model ensures that a read request from any replicas will get the latest write value EX - account password update 


## System Failures

    Failures are obvious in the world of distributed systems and can appear in various ways. They might come and go, or persist for a long period.

#### Failure Types (in order of severity)

    - Fail Stop:
        In this type of failure, a node in the distributed system halts permanently. However, the other nodes can still detect that node by communicating with it.
    - Crash
        In this type of failure, a node in the distributed system halts silently, and the other nodes can’t detect that the node has stopped working.
    - Omission
        In omission failures, the node fails to send or receive messages.
    - Temporal
        In temporal failures, the node generates correct results, but is too late to be useful. 
    - Byzantine
        In Byzantine failures, the node exhibits random behavior like transmitting arbitrary messages at arbitrary times, producing wrong results, or stopping midway.

## Non Functional Chracteristics

### Availability
    Availability is the percentage of time that some service or infrastructure is accessible to clients and is operated upon under normal conditions.
### Reliability
    Measures how well a system performs its intended operations (functional requirements). We use averages for that (Mean Time to Failure, Mean Time to Repair, etc.)
### Scalability
    Scalability is the ability of a system to handle an increasing amount of workload without compromising performance.

    -Vertical / Up : Increasing the capacity of machine
    -Horizontal / Out: Increasing the  number of machines
### Maintainability
    Besides building a system, one of the main tasks afterward is keeping the system up and running by finding and fixing bugs, adding new functionalities, keeping the system’s platform updated, and ensuring smooth system operations.

    - Operability -  keep the system operational under diff situations
    - Lucidity - Simple system to maintain
    - Modificability -  Easy to modify to meet future requirements
### Fault Tolerance
    Fault tolerance refers to a system’s ability to execute persistently even if one or more of its components fail.

    Techniques:

        - Replication
        - Checkpointing: Checkpointing is a technique that saves the system’s state in stable storage for later retrieval in case of failures due to errors or service disruptions.
        
## Back of the envelope Cacl.
    Involve swift, approximate, and simplified estimations or computations typically done on paper or, figuratively, on the back of an envelope. While these calculations are not intended to yield precise results, they function as a quick and preliminary evaluation of crucial parameters and the feasibility of a system.

## Building Blocks
    The purpose of separating the building blocks is to thoroughly discuss their design just once. This means that later we can use them anywhere without having to go over them in detail again. We can think about building blocks as bricks to construct more effective, capable systems.

#### Domain Name System(DNS):
    
    - Its a phone book of an internet to have the mapping b/w the website domain to IP address as to remember the IPs of multiple website is not feasible.
    - User -> Browser -> ISP -> DNS Arch. -> ISP -> Browser -> Webserver with IPs
    - Resource Record: single row in DNS server to get the details is called RR.
    - DNS server maintains some caching to make the retreival fast.
    - DNS names are processed right to left EX vipulpopli.com first it will resolve the .com and direct to the name server and then vipulpopli and so on.
    - It ensured the AP from CAP as this arch. is read intensive as compare to write.
    - If we need DNS to tell us which IP to reach a website or service, how will we know the DNS resolver’s IP address? (It seems like a chicken-and-egg problem!)

#### Load Balancers:
    - The job of the load balancer is to fairly divide all clients’ requests among the pool of available servers. Load balancers perform this job to avoid overloading or crashing servers.
    - LB provides us the availability , performance benefits and Scalability 
    - User (Client) -> LB -> Server's
    - Other Benefits: Health checking of the server.
    - What if load balancers fail? Are they not a single point of failure (SPOF)?
    - Global server load balancing (GSLB): GSLB involves the distribution of traffic load across multiple geographical regions. GSLB basically check up on the geigraphical server health and based on that takes the decision to route the traffic to the corresponding locations.
    - Local load balancing: This refers to load balancing achieved within a data center.
    - Algos for LB
      
        - Round robin: Sequential rotation
        - Weighted round robin : In case some server have higher capability of handling the requests that server is assigned with weight based on that load is being balanced.
        - Least Connection: Load is balanced to the server with the least no of active connections.
        - Least response time: Load is balanced to the server with the least response time.
        - IP hash: Load is balanced based on the user IP hashed.
        - URL hash:  Load is balanced based on the URL hashed.
    - Static Algo's: Do not consider the changing state of the servers.
    - Dynamic Alg's: Consider the changing state of the servers.
    - In practice, dynamic algorithms provide far better results because they maintain a state of serving hosts and are, therefore, worth the effort and complexity.
    - Stateful LBs: As the name indicates, stateful load balancing involves maintaining a state of the sessions established between clients and hosting servers. The stateful LB incorporates state information in its algorithm to perform load balancing.
    - Stateless LBs: Stateless LBs use consistent hashing to make forwarding decisions.However, if infrastructure changes stateless LBs may not be as resilient as stateful LBs because consistent hashing alone isn’t enough to route a request to the correct application server. Therefore, a local state may still be required along with consistent hashing.Therefore, a state maintained across different load balancers is considered as stateful load balancing. Whereas, a state maintained within a load balancer for internal use is assumed as stateless load balancing.
    - Tier 1 balances the load among the load balancers themselves. Tier 2 enables a smooth transition from tier 1 to tier 3 in case of failures, whereas tier 3 does the actual load balancing between back-end servers. Each tier performs other tasks to reduce the burden on end-servers.

#### Databases:

    - To design the system we need to store the permanent data available to maintain the customer state.
    - Files as a data base have some disadvantages

        - No Concurrency support
        - Access management
        - Not scalable for high throughput
    - Database: Store the organised form of data to have easy retrival and manipulation is DB's
    - Basic Types
        - SQL (Relational)
        - NoSQL (non-Relational)
    - Relational database has a well defined structure such as attributes (columns of the table). NoSQL databases such as document databases often have application-defined structure of data.

##### Relational databases:
    - Relational databases adhere to particular schemas before storing the data. The data stored in relational databases has prior structure. Mostly, this model organizes data into one or more relations (also called tables), with a unique key for each tuple (instance). Each entity of the data consists of instances and attributes, where instances are stored in rows, and the attributes of each instance are stored in columns. Since each tuple has a unique key, a tuple in one table can be linked to a tuple in other tables by storing the primary keys in other tables, generally known as foreign keys.
    - A Structure Query Language (SQL) is used for manipulating the database.
    - Relational databases provide the atomicity, consistency, isolation, and durability (ACID) properties to maintain the integrity of the database.
    - Poplular DBMS
        - MySQL
        - Oracle Database
        - Microsoft SQL Server
        - IBM DB2
        - Postgres
        - SQLite
    - Flexible SQL provides flexiblility to modify the data , tables
    - Helps a lot in removing the data redundancy with the help of normalisation.
    - Concurrency is an important factor while designing an enterprise database. In such a case, the data is read and written by many users at the same time
    - Relational databases guarantee the state of data is consistent at any time. The export and import operations make backup and restoration easier.
    - Drawback: Impedance mismatch is the difference between the relational model and the in-memory data structures. The relational model organizes data into a tabular structure with relations and tuples. SQL operation on this structured data yields relations aligned with relational algebra. However, it has some limitations. In particular, the values in a table take simple values that can’t be a structure or a list. The case is different for in-memory, where a complex data structure can be stored. To make the complex structures compatible with the relations, we would need a translation of the data in light of relational algebra.

###### ACID:
    - Atomicity: A transaction is considered an atomic unit. Therefore, either all the statements within a transaction will successfully execute, or none of them will execute. If a statement fails within a transaction, it should be aborted and rolled back.
    - Consistency: At any given time, the database should be in a consistent state, and it should remain in a consistent state after every transaction. For example, if multiple users want to view a record from the database, it should return a similar result each time.
    - Isolation: In the case of multiple transactions running concurrently, they shouldn’t be affected by each other. The final state of the database should be the same as the transactions that were executed sequentially.
    - Durability: The system should guarantee that completed transactions will survive permanently in the database even in system failure events.

##### Non Relational databases:
    - These databases are used in applications that require a large volume of semi-structured and unstructured data, low latency, and flexible data models.
    - Benefits
        - Simple design
        - Horizontal scaling: NoSQL makes it easier to scale out since the data related to a specific employee is stored in one document instead of multiple tables over nodes.
        - Cost: Licenses for many RDBMSs are pretty expensive, while many NoSQL databases are open source and freely available.

    - Types 
        - Key Value (DynamoDB)
        - Document (MongoDB)
        - Column Oritented / Columnar (Cassandra)
        - Graph (Neo4j)
    
    - Drawbacks
        - Porting from one NoSQL to another will be challenging as no structure is maintained.
        - NoSQL databases provide different products based on the specific trade-offs between consistency and availability when failures can happen. 
        -  

######   Key Value: 
    - Key-value databases use key-value methods like hash tables to store data in key-value pairs

######   Document
    - A document database is designed to store and retrieve documents in formats like XML, JSON, BSON, and so on. These documents are composed of a hierarchical tree data structure that can include maps, collections, and scalar values. Documents in this type of database may have varying structures and data. MongoDB and Google Cloud Firestore are examples of document databases.


######   Column Oritented / Columnar
    - It store the data in column instead of the rows.Popular columnar databases include HBase, Hypertable, and Amazon Redshift.

######   Graph
    - Graph databases use the graph data structure to store data, where nodes represent entities, and edges show relationships between entities.
   
##### Data Replication:
    - Replication refers to keeping multiple copies of the data at various nodes (preferably geographically distributed) to achieve availability, scalability, and performance.
    
    - Complexities that could arise due to replication are as follows:
        - How do we keep multiple copies of data consistent with each other?
        - How do we deal with failed replica nodes?
        - Should we replicate synchronously or asynchronously?
        - How do we deal with replication lag in case of asynchronous replication?
        - How do we handle concurrent writes?
        - What consistency model needs to be exposed to the end programmers?
    
    - Types
        - Sync:
            In synchronous replication, the primary node waits for acknowledgments from secondary nodes about updating the data. After receiving acknowledgment from all secondary nodes, the primary node reports success to the client.
            Latency from primary to the client.
        - Async
            Whereas in asynchronous replication, the primary node doesn’t wait for the acknowledgment from the secondary nodes and reports success to the client after updating itself.
            Data loss if fails to write the data eventually
    
    - Models
        - Single leader or primary-secondary replication
            - Primary node is responsible to write the the replicated data in the secondary node. We can distribute the read load to primary and secondary hence will be helpfurl for read intensive apps. Async Replication is done from primary to secondary.
            - Statment based replication: is an approach used in MySQL databases. In this approach, the primary node executes the SQL statements such as INSERT, UPDATE, DELETE, etc., and then the statements are written into a log file. In the next step, the log file is sent to the secondary nodes for execution.
            - Write-ahead log (WAL) shipping is a data replication technique used in both PostgreSQL and Oracle. In this technique, when a transaction occurs, it’s initially recorded in a transactional log file, and the log file is written to disk. Subsequently, the recorded operations are executed on the primary database before being transmitted to secondary nodes for execution. 
            - Logical (row-based) replication is utilized in various relational databases, including PostgreSQL and MySQL. In this approach, changes made to the database are captured at the level of individual rows and then replicated to the secondary nodes. Instead of replicating the actual physical changes made to the database, this approach captures the operations in a logical format and then executes them on secondary nodes.


        - Multi-leader replication
            -  There are multiple primary nodes that process the writes and send them to all other primary and secondary nodes to replicate.
            - Being 2 primary node there occurs a condition of the conflicts if the same data is being updated from two diff primary node.
            - Divide the records among the primary nodes.It will be an issue in case of diff regions we have to still access it via the far primary node.
            - Topology Circular, Star and all to all

        - Peer-to-peer or leaderless replication
            - All the nodes have equal weightage and can accept read and write requests. This replication scheme can be found in the Cassandra database.
            - Quorums: If we have 𝑛 n nodes, then every write must be updated in at least 𝑤 w nodes to be considered a success, and we must read from 𝑟 r nodes. We’ll get an updated value from reading as long as 𝑤 + 𝑟 > 𝑛 w+r>n because at least one of the nodes must have an updated write from which we can read. Quorum reads and writes adhere to these 𝑟 r and 𝑤 w values. These 𝑛 n , 𝑤 w , and 𝑟 r are configurable in Dynamo-style databases. 



#### Key-Value Store:
#### Content Delivery Network:
#### Sequencer:
#### Service Monitoring:
#### Distributed Caching:
#### Distributed Messaging Queue:
#### Publish-Subscribe System:
#### Rate Limiter:
#### Blob Store:
#### Distributed Search:
#### Distributed Logging:
#### Distributed Task Scheduling:
#### Sharded Counters: 

