+++
title = "Microservices: Benefits and Challenges"
date = 2018-05-30
author = "Carlos R.F."
+++

# What is the Microservice Architecture Style?

Microservices are small applications with a single responsibility that can be deployed, scaled, and tested independently [^1]. They are responsible for one business capability (a.k.a., bounded context) [^2].

Microservices as an architecture style of building systems has gained popularity in recent years because of the benefits you can get from adopting it [^3][^1]. The abundance of case studies and the maturity of the supporting infrastructure make them a safer bet at this point in time (examples: [^8], [^7] and [^9]).

# Benefits of Microservices

Adopting microservices offers the following primary benefits:

* **Facilitates Continuous Delivery and Continuous Deployment** [^4]: By reducing the number of artifacts in the system that need to be redeployed as a result of a change. For example, a minor business logic change would only require the build and restart of the corresponding service. This allows for quicker and less risky deployments. The adoption of advanced technologies like Kubernetes and Docker would make this second nature.
* **Reduces the risk of system-driven or business-driven Continuous Experimentation** [^5], [^4]: and, as a result, creates a better environment that encourages so. For example, in a monolith, upgrading a core library or experimenting with a new language would require a full app migration. A microservice is not only an independent deployment unit, but also an independent module in the source code base. It would be possible to run two separate versions of the service that are, source-code-wise, totally incompatible, or you could quickly rollback only one service without having to rollback other changes or do `git` magic. This enables business and system experimentation in more flexible ways with less risk.
* **Facilitates tailored stack deployments for acceptance test/sign off** [^1]: By only deploying the minimum amount of microservices to enable a business capability that is under test. This means fewer moving pieces and fewer resource requirements to deploy the desired capability in a feature branch environment.
* **Infrastructure usage optimization**: By automating the elasticity of the system and scaling out only the services that are used the most at a given point in time [^4]. This allows for a direct savings on cloud service costs in the public cloud. On-premise clouds, the cost savings are more subtle: it is reflected in the less need to buy more hardware infrastructure.
* **Isolation of Source Code**: Isolating source code can be beneficial for two reasons: It isolates experiments with new approaches or libraries, and it isolates bad code designs [^6][^4]. The isolation would help in the future to safely replace the service with a better version since all dependencies on that service would be on its REST API and not on the code directly.
* **Reducing impact of Faults**: Basically, one microservice going down because of some memory leak or excessive CPU usage would not affect the availability of other features in the system that do not have a strong dependency on it.

# Challenges Ahead

Because of the abundance of case studies and the maturity of the microservice approach, there are well-documented challenges when adopting it. In this section, I address a few of these challenges.

* **Increase complexity of the deployment model**: If the system is seen as a graph where vertices are autonomous services and edges are dependencies, the microservice architecture style does have more vertices and edges; therefore, it increases the deployment model complexity. New generation tools like Kubernetes and all the ecosystems around them greatly enhance the operational effort of a microservices system [^3].
* **Performance**: Generally speaking, microservices performance is not better than traditional monoliths. It depends on factors like network latency, virtualization, and developer skills. The developers will need to adopt best practices when coding microservices and leverage caching to increase performance [^3].
* **Organization**: Usually, microservices bring a cultural change to the organization [^3].
* **Design**: Designing microservices requires that the developer become familiar with microservice design techniques in order to scope the microservice responsibility correctly and to design the communication channels efficiently. This task is one of the most important to get right at the beginning of their design [^10]. Mistakes on this aspect have bad consequences down the road [^2]. Fortunately, there are very well-known best practices and techniques to successfully assign responsibilities to microservices based on how the business operates and its different capabilities. The most well-known and proven one is Domain Driven Design (DDD) [^1][^6]. The more the microservices separation of concerns is in alignment with the actual business, the less likely it will need major responsibility changes in the future [^6]. The developer would need to understand the domain and split the system accordingly instead of using the traditional split by layers.

# Conclusion

Splitting a monolith system into microservices can definitely help a company reduce long-term costs and improve the quality of its services by improving availability, flexibility, and the risk of experimentation.

Adopting Kubernetes and its ecosystem is a **must** to make the operational effort for microservices manageable and cost-effective. It also enables the team to reap some of the benefits of microservices, like optimized auto-scaling and continuous experimentation and delivery, without having to build such a platform from scratch.

However, making it happen requires a significant refactoring effort and a cross-team commitment to this approach, which can take a few years to complete.

# References

[^1]: "Microservices"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2018.2141030
[^2]: "On the Definition of Microservice Bad Smells"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2018.2141031
[^3]: "Microservices: The Journey So Far and Challenges Ahead"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2018.2141039
[^4]: "The Hidden Dividends of Microservices"; [URL] https://queue.acm.org/detail.cfm?id=2956643
[^5]: "Continuous Experimentation: Challenges, Implementation Techniques, and Current Research"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2018.111094748
[^6]: E. Evans, "Domain-Driven Design: Tackling Complexity in the Heart of Software,"  Addison-Wesley, 2003.
[^7]: "From Monolithic to Microservices: An Experience Report from the Banking Domain"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2018.2141026
[^8]: "Using Microservices for Legacy Software Modernization"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2018.2141035
[^9]: "A Scalable, Reactive Architecture for Cloud Applications"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2017.265095722
[^10]: "Migrating Enterprise Legacy Source Code to Microservices: On Multitenancy, Statefulness, and Data Consistency"; [URL] https://doi.ieeecomputersociety.org/10.1109/MS.2017.440134612