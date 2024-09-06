# Transaction Saga Pattern

## Introduction

The concept of a saga in architecture predates microservices, originally concerned with limiting the scope of database locks in early distributed architectures.

the saga pattern for microservices as a sequence of local transactions where each update publishes an event, thus triggering the next update in the sequence. If a failure occurs, the saga executes compensating transactions that counteract the preceding updates. - Chris Richardson

There are 8 different types of sagas:

<!-- a table with 9 row and 4 columns  -->
| Pattern Name | Communication | Consistency | Coordination |
| --- | --- | --- | --- |
| Epic Saga | Synchronous | Atomic | Orchestrated |
| Phone Tag Saga | Synchronous | Atomic | Choreographed |
| Fairy Tale Saga | Synchronous | Eventual | Orchestrated |
| Time Travel Saga | Synchronous | Eventual | Choreographed |
| Fantasy Fiction Saga | Asynchronous | Atomic | Orchestrated |
| Horror Story Saga | Asynchronous | Atomic | Choreographed |
| Parallel Saga | Asynchronous | Eventual | Orchestrated |
| Anthology Saga | Asynchronous | Eventual | Choreographed |

## Epic Saga Pattern

This type of communication is the “traditional” saga pattern as many architects understand it, also called an Orchestrated Saga because of its coordination type. This pattern utilizes synchronous communication, atomic consistency, and orchestra‐ ted coordination.

![Epic Saga Pattern](./images/epic-saga.png)

an orchestrator service orchestrates a workflow that includes updates for three services, expected to occur in a transactional manner where either all three calls succeed or none do.
such transactions limit the choice of databases and have legendary failure modes.

as with many things in architecture, the error conditions cause the difficulties. In a compensating transaction framework, the mediator monitors the success of calls, and issues compensating calls to other services if one or more of the requests fail.

The clear advantage of the Epic Saga(sao) is the transactional coordination that mimics monolithic systems, coupled with the clear workflow owner represented via an orchestrator However, the disadvantages are varied. First, orchestration plus transactionality may have an impact on operational architecture characteristics such as performance, scale, elasticity, and so on—the orchestrator must make sure that all participants in the transaction have succeeded or failed, creating timing bottlenecks.

### Rating

| Epic Saga Pattern | Rating |
| --- | --- |
| Communication | Synchronous |
| Consistency | Atomic |
| Coordination | Orchestrated |
| Coupling | High |
| Complexity | High |
| Responsiveness/Availability | Low |
| Scale/elasticity | Very Low |

## Phone Tag Saga Pattern

The Phone Tag Saga(sac) pattern changes one of the dimensions of the Epic Saga(sao), changing coordination from orchestrated to choreographed.

![Phone Tag Saga Pattern](./images/phone-tag-saga.png)

The pattern name is Phone Tag because it resembles a well-known children’s game known as Telephone in North America: children form a circle, and one person whispers a secret to the next person, who passes it along to the next, until the final version is spoken by the last person.

How does choreography versus orchestration improve operational architecture characteristics like scale? Using choreography even with synchronous communication cuts down on bottlenecks—in non-error conditions, the services can operate independently, and the overall system can scale more easily.

A nice feature of non-orchestrated architectures is the lack of a coupling singularity, a single place the workflow couples to.

### Rating - Phone Tag Saga Pattern

| Phone Tag Saga Pattern | Rating |
| --- | --- |
| Communication | Synchronous |
| Consistency | Atomic |
| Coordination | Choreographed |
| Coupling | High |
| Complexity | High |
| Responsiveness/Availability | Low |
| Scale/elasticity | High |

## Fairy Tale Saga Pattern

Typical fairy tales provide happy stories with easy-to-follow plots, thus the name Fairy Tale Saga(seo), which utilizes synchronous communication, eventual consistency, and orchestration.

![Fairy Tale Saga Pattern](./images/fairy-tale-saga.png)

This communication pattern relaxes the difficult atomic requirement, providing many more options for architects to design systems. For example, if a service is down temporarily, eventual consistency allows for caching a change until the service restores.

In this pattern, an orchestrator exists to coordinate request, response, and error handling. However, the orchestrator isn’t responsible for managing transactions, which each domain service retains responsibility for. Thus the orchestrator can manage compensating calls, but without the requirement of occurring within an active transaction.

The biggest appealing advantage of the Fairy Tale Saga(seo) is the lack of holistic trans‐ actions. Each domain service manages its own transactional behavior, relying on eventual consistency for the overall workflow.

### Rating - Fairy Tale Saga Pattern

| Fairy Tale Saga Pattern | Rating |
| --- | --- |
| Communication | Synchronous |
| Consistency | Eventual |
| Coordination | Orchestrated |
| Coupling | High |
| Complexity | Very Low |
| Responsiveness/Availability | Medium |
| Scale/elasticity | High |

## Time Travel Saga Pattern

The Time Travel Saga(sec) pattern features synchronous communication, and eventual consistency, but choreographed workflow. In other words, this pattern avoids a central mediator, placing the workflow responsibilities entirely on the participating domain services.

![Time Travel Saga Pattern](./images/time-travel-saga.png)

In this workflow, each service accepts a request, performs an action, and then for‐ wards the request on to another service. This architecture can implement the Chain of Responsibility design pattern or the Pipes and Filters architecture style.

Each service in this pattern “owns” its own transactional operations, so architects must design workflow error conditions into the domain design. this pattern is best suited for simple workflows with few services, as the complexity of error handling increases with each additional service.

For solutions that benefit from high throughput, this pattern works extremely well for “fire and forget” style workflows, such as electronic data ingestion, bulk transactions, and so on.

Because this pattern lacks holistic transactional coordination, architects must take extra effort to synchronize data.

### Rating - Time Travel Saga Pattern

| Time Travel Saga Pattern | Rating |
| --- | --- |
| Communication | Synchronous |
| Consistency | Eventual |
| Coordination | Choreographed |
| Coupling | Medium |
| Complexity | Low |
| Responsiveness/Availability | Medium |
| Scale/elasticity | High |

## Fantasy Fiction Saga Pattern

Traditionally, one way that architects increase the responsiveness of distributed systems is by using asynchronicity, allowing operations to occur in parallel rather than serially. This may seem like a good way to increase the perceived performance over an Epic Saga.

![Fantasy Fiction Saga Pattern](./images/fantasy-fiction-story-saga.png)

Adding asynchronicity to orchestrated workflows adds asynchronous transactional state to the equation, removing serial assumptions about ordering and adding the possibilities of deadlocks, race conditions, and a host of other parallel system challenges.

### Rating - Fantasy Fiction Saga Pattern

| Fantasy Fiction Saga Pattern | Rating |
| --- | --- |
| Communication | Asynchronous |
| Consistency | Atomic |
| Coordination | Orchestrated |
| Coupling | High |
| Complexity | High |
| Responsiveness/Availability | Low |
| Scale/elasticity | Low |

