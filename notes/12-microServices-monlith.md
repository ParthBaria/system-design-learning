# Monolith

in this architecture the entire program is constructed in single indivisble unit
- changes in program will neeed to restart entire server
- they can become complex and difficult to maintain as the size and complexity of the application grow.

# Microservices
A microservices architecture results in an application designed as a set of small, independent services. Each one represents a business capability in itself. The services loosely couple with one another and communicate over the network, typically making use of lightweight protocols such as HTTP or messaging queues.

# comparision

| Aspect          | Monolithic Architecture                             | Microservice Architecture                                                        |
| --------------- | --------------------------------------------------- | -------------------------------------------------------------------------------- |
| Architecture    | Single-tier architecture                            | Multi-tier architecture                                                          |
| Size            | Large, all components tightly coupled               | Small, loosely coupled components                                                |
| Deployment      | Deployed as a single unit                           | Individual services can be deployed independently                                |
| Scalability     | Horizontal scaling can be challenging               | Easier to scale horizontally                                                     |
| Development     | Development is simpler initially                    | Complex due to managing multiple services                                        |
| Technology      | Limited technology choices                          | Freedom to choose the best technology for each service                           |
| Fault Tolerance | Entire application may fail if a part fails         | Individual services can fail without affecting others                            |
| Maintenance     | Easier to maintain due to its simplicity            | Requires more effort to manage multiple services                                 |
| Flexibility     | Less flexible as all components are tightly coupled | More flexible as components can be developed, deployed, and scaled independently |
| Communication   | Communication between components is faster          | Communication may be slower due to network calls                                 |
