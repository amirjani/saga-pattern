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