# Architecture Decision Records 

This section provides a record of the architectural decisions that we have made.  It allows us to build up a knowledge repository of how the architecture evolved to its current state. It is based on Michael Nygard's ADR template (see https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) 

 
## Decision 1: Design a cloud-native architecture 
**Status:** Accepted

**Context:** We are a startup and we have a strong emphasis on scalability, resilience, and global reach. This means that 1.) we do not want to spend all our VC money on buying servers, 2.) we hope that our userbase will continue to grow over time, and 3.) that we require low latency across the globe. 

**Decision:** We have decided to design our solution to be deployed into a cloud environment right off the bat. This will keep our infrastructure costs in check while we scale up, allow us to scale components with relative ease, and give us access to edge locations around the world to reduce latency when users interact with Road Warrior. 

**Consequences:** We may incur some vendor lock-in. 

## Decision 2: Design different subsystems to meet different needs 
**Status:** Accepted 

**Context:** The Road Warrior solution is inherently complex, with multiple moving parts. We cannot get away from complexity, but if we can keep complexity in check by using purpose-specific subsystems that fulfil specific, well-defined goals we can abstract complexity away from other components that have no reason to care about that complexity.   

**Decision:** We design our solution with multiple subsystems that serve different purposes.  This will also allow us to scale components independently, and to apply different architectural styles to different components. 

**Consequences:** We increase the complexity of our hosting environment, compared to a single monolithic solution. This can be managed, however, via infrastructure as code and solid development process that focus on CI/CD. 


## Decision 3: Develop native mobile applications 

**Status:** Accepted 

**Context:** We require the richest possible interfaces to streamline our users' experience. This includes integration with device functionality such as GPS. While progressive web applications (PWAs) have advanced greatly, we believe that the UX is still not quite on par with native applications. 

**Decision:** We will implement native mobile applications for popular mobile platforms, such as iOS, Android, and Huawei. 

**Consequences:** This increases development time and cost; we can mitigate this by developing for the most popular platforms first so that our product launch isn't delayed. 

## Decision 4: Pipes-and-filters architecture for data ingestion 
**Status:** Accepted 

**Context:** We expect vast amounts of data flowing into the system from user emails and API integration points. We believe this can introduce a performance bottleneck in the solution if it is not managed well.  As such, we want to ingest emails as quickly as possible. 

**Decision:** We will use a pipes-and-filters architecture that allows us to break this process down into discrete steps that can scale independently. The first, most lightweight step will simply receive data from an external provider and immediately move that to a queue for processing, which prevents blocking between the data source and the data processor while data is being processed. The queue can have multiple consumers that will process data and the number of consumers can be increased based on queue size. 

**Consequences:** This allows us to ingest large volumes of data while reducing the likelihood of performance issues. 

## Decision 5: Email scanning plugins / add-ons for specific providers 
**Status:** Accepted 

**Context:** Due to privacy and data storage (volume) concerns, we do not want to plug into user email accounts in their entirety and pull all of their email data into Road Warrior. We have evaluated the prospect of building email plugins that users can install directly into the most widely-used email providers like Google's Gmail or Microsoft's Outlook. We have not conducted a detailed analysis, but based on tools like SaneBox (https://www.sanebox.com/) we believe this to be possible. 

**Decision:** We intend to create plugins for the most popular mail providers to simplify the process of email-based integration into Road Warrior. 

**Consequences:** This streamlines the experience for the majority of users, but not all. For users using other email clients, there must be a generic email polling component. 

## Decision 6: Off the shelf authentication and identity provider
**Status:** Accepted

**Context:** We need to have authorization and authentication

**Decision:** We intend to use a Authentication-as-a-service (AaaS) provider for the authentication and authorization of Road Warrior

 
**Consequences:**
* Abstracts away nuances of authentication flows (traveller login, system authentication, password recovery etc)
* Standards such as Oauth, OIDC and SAML are easily available, this reduces vendor lock-in
* Extensible: because newer, features like WebAuthn are easiliy available (this can potentially increase vendor lock-in)
* Quick way to implement user management dashboards
* Access log can also be incorporated


## Decision 7: The use of a Data Lake
**Status:** Accepted

**Context:** The system needs to collate data from different sources

**Decision:** We make the assumption that alot of the data are event based and needs to be cleansed or transformed to make sense. The Data Lake approach accomodates this and can be geographically dispersed when leveraging cloud.

**Consequences:**
* The data lake will and can grow very large - however with different levels of storage we could reduce cost.
* The system can generate different views of data.
* The data can be mined for insights.