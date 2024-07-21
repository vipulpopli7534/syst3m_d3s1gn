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


### Consistency

The data remains as close as to the data from source of truth will be more consistence data.


#### Consistency Types (in order of consistency)
    
    - Eventual:
        Least consistense spectrum doesn't gurantees the read will provide the data in sync.It ensures highly available DS EX- DNS resolver config , Cassandra
    - Causal
        Ensures the consistency only for the dependant operation or causally linked operations. EX- Commenting system
    - Sequential
        Ensures the consistency of a single process.IF multiple processes are running each process will have its own cache replica from the actual DB to have the cosnistency once the update operation is performed DB spawns the callout to each process cache. EX- News feed system. 
    - Strictly
        This model ensures that a read request from any replicas will get the latest write value EX - account password upadte 
