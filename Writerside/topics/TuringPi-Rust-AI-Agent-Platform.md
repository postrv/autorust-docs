# TuringPi - Rust AI Agent Platform - RFC

## AutoRust RFC

### Author:
Laurence Avent

### Date:
Thursday 4 Jan 2024

## Abstract
A Rust-Based AI Agent orchestration platform is proposed. The core system will comprise a configurable set of Agents, a registry, and a manager. 

Rust offers fearless concurrency, memory safety, and speed, making it an ideal language for this kind of project.

The system will be deployed on Kubernetes and be able to scale up and down according to the user's requirements.

The MVP will be able to orchestrate a set of Agents to perform a simple task, such as reading a sketch of a proposed system and turning it into specs/pseudocode, then into code, which can be checked and run by other Agents.

The full implementation should unlock RAG, fine-tuning, and other advanced features of the OpenAI API.

We anticipate that individual developers (e.g. those already in the TuringPi Community) and businesses will find great utility in locally hosted AI Agents so that they can keep their data private and avoid the costs and privacy concerns of cloud-based AI Agents. 

This system is therefore designed such that it can be relatively trivially adopted in an on-prem/edge environment.

### Purpose

The purpose of this proposal is to outline the design of this system and to provide a basis for discussion and feedback.

### Scope

The scope of this proposal is limited to the design of the system. 

It is not intended to form a detailed technical specification and implementation details may change, especially those relating to the frontend and user experience.

Cost calculations have been deferred to the implementation phase.

### Considerations

The following considerations have been taken into account in the design of this system:
- Language: Rust
- Deployment: Kubernetes
- API: OpenAI API
- User Experience: CLI and Web UI
- Security: Private data, on-prem/edge deployment
- Code Execution Environment: Sand-boxed Docker Container or, for evaluation/MVP purposes Google Cloud Functions.

## Table of Contents

1. [AutoRust RFC](#autorust-rfc)
2. [Abstract](#abstract)
3. [Introduction](#introduction)
4. [Stakeholders](#stakeholders)
5. [Goals and Non-Goals](#goals-and-non-goals)
6. [Architectural Overview](#architectural-overview)
7. [Detailed Design](#detailed-design)
8. [Technology Stack](#technology-stack)
9. [Security Considerations](#security-considerations)
10. [Performance Considerations](#performance-considerations)
11. [Scalability, Reliability and Testing](#scalability-reliability-and-testing)
12. [Risks and Mitigations](#risks-and-mitigations)
13. [Future Work](#future-work)
14. [Appendices](#appendices)
15. [Approval and Feedback](#approval-and-feedback)

## Introduction

Microsoft have open-sourced their AI Agent framework, which is written in Python. 

See: [Autogen](https://github.com/microsoft/autogen)

This allows for orchestration, but is limited to Python and the OpenAI API. It is also not designed for on-prem/edge deployment. 

We therefore see the need for a Rust-based AI Agent Platform that can be deployed on-prem/at the edge and can be used to orchestrate a variety of Agents, including those that are not based on the OpenAI API.

## Stakeholders
- Laurence Avent
- TuringPi
- TuringPi Customers/Community
- TuringPi Investors
- Rust Community
- Raspberry Pi Community
- LLM Enjoyers the World Over...

## Goals and Non-Goals

### Goals
- Create a Rust-based AI Agent Platform that can be used to create AI Agents for a variety of purposes.
- Enable users to define custom workflows and hierarchies for AI Agents
- Expand the TuringPi ecosystem
- We want to remain reactive to the rapidly shifting AI landscape. 
- We will use the OpenAI API and other existing AI Agents for now but will be able to swap these out as the landscape changes.

### Non-Goals
- We are not trying to reinvent the wheel. We will use existing libraries and frameworks where possible.
- We do not wish to lock ourselves into a specific ecosystem or vendor. We will use open-source technologies and frameworks where possible.

## Architectural Overview

The proposed system consists of the following components:
- Agent Manager: Manages the Agents and the Agent registry. Has the ability to start and stop Agents and to append messages to their message queues.
- Agent Registry: Maintains a list of Agents, their IDs, and functions.
- Agents for specific roles: e.g. GPT Vision, Coding Agent, Code Interpreter, Code Checker (see example).
  - Each Agent has its own message queue and can be configured to run on a separate thread or process.
  - They can also be configured to run on a separate machine or cluster depending on their requirements.
- Message Handling: JSON and image data communication, in line with the OpenAI API.
- OpenAI API Communication: External system interaction using the OpenAI API and authentication token.
- Error Handling and Regeneration: Manages errors from the OpenAI API and coding errors.
- Logging and Monitoring: Implements logging and monitoring with common Rust libraries.

The following diagram illustrates the proposed generic high-level architecture:

![Architecture Diagram](../images/async.png "High Level Architecture Diagram")

The following diagram illustrates a more comprehensive example configuration:

![Architecture Diagram - Detailed](../images/async-advanced.png "Detailed Architecture Diagram with example agent configuration")

In this example, there are 4 x Agents arranged as follows:
1. GPT Vision Agent - can read a sketch of a proposed system and turn it into specs/pseudocode (GPT Vision Agent)
2. Coding Agent - can turn the specs/pseudocode into code
3. Code Interpreter Agent - can run the code and return the results
4. Code Checker Agent - can check the output for errors

We can therefore think of them as performing the following respective duties:
1. Product Manager
2. Developer
3. DevOps
4. QA

The user should be able to specify the number of Agents for each role, the hierarchy and the order of operations.

This system is designed such that it can be relatively trivially adopted in an on-prem/edge environment and will run on K8's or K3's. 

Taking advantage of Rust's fearless concurrency, we will get the best possible results out of a limited compute environment.

## Detailed Design

### User Journeys for inclusion in the UI

The following user journeys have been identified:
- User wants to: 
  - Create one or more Agents 
  - Define a workflow for one or more Agents 
  - Run an Agent or workflow 
  - Stop an Agent or workflow

- User wants to view the status of:
  - One or more Agent 
  - The system 
  - The Agent registry 
  - The message queue 
  - The OpenAI API (or other API if integrating with other AI providers)
- User wants to view the logs

The UI should allow the user to perform the above actions unsupervised and in an intuitive manner.

Though not in the scope of this proposal, we should consider how best to implement a natural language orchestration/setup interface for onboarding customers and for customizing workflows.

### Components

#### Agent Manager

The Agent Manager will be responsible for managing the Agents and the Agent registry.

The Agent Manager will be responsible for:
- Starting and stopping Agents
- Managing the Agent registry
- Managing the message queue
- Managing the OpenAI API (or other API if integrating with other AI providers)

#### Agent Registry

The Agent Registry will be responsible for maintaining a list of Agents, their IDs, and functions.

#### Agents for specific roles

As discussed in [#Architectural Overview](#architectural-overview) above, Agents will be responsible for performing specific roles, such as:
- GPT Vision
- Coding Agent
- Code Interpreter
- Code Checker

Others might include: 
- Assistants API/GPT's for pre-specialised Agents
- [Future] RAG
- [Future] Fine-tuning
- [Future] Other AI providers

#### Message Handling

The system will use JSON for message handling, in line with the OpenAI API. It will be able to handle multimodal (text and image) data in messages.

#### OpenAI API Communication

The system will communicate with the OpenAI API (or other API if integrating with other AI providers) using an authentication token.

#### Error Handling and Regeneration

The system will be able to handle errors from the OpenAI API and coding errors.

#### Logging and Monitoring

The system will implement logging and monitoring with common Rust libraries.

### MVP/Proof of Concept
An MVP framework for the system has been developed and is available here: [TuringPi-Rust-AI-Agent-Platform](https://github.com/postrv/autorust)

#### Learning Points
The MVP exercise has highlighted the following learning points:
- Rust is a great language for this kind of project. It is fast, safe, and has great concurrency features.
- The system must be able to handle errors from the OpenAI API and coding errors.
- An asynchronous approach is required to handle and benefit from the concurrent nature of the system.
- Consideration must be given to the handling of multimodal (text and image) data in messages. See below excerpt from the MVP code:
[GPTVisionAgent - lines 26 - 56](https://github.com/postrv/autorust/blob/master/src/gpt_vision_agent.rs#L26-L56)

```C
impl GPTVisionAgent {
    fn process_message(&mut self, message: &Message) {
        match message.content["type"].as_str() {
            Some("image_analysis_request") => {
                if let Some(image_base64) = message.content["data"].as_str() {
                    match base64::decode(image_base64) {
                        Ok(image_data) => {
                            let self_id = self.id.clone();
                            let _sender_id = message.sender_id.clone();
                            // Use tokio::spawn to handle the async call
                            tokio::spawn(async move {
                                match GPTVisionAgent::analyze_image(&self_id, &image_data).await {
                                    Ok(_specs) => {
                                        // Send the message using a new instance or a shared state
                                    },
                                    Err(e) => eprintln!("Error analyzing image: {}", e),
                                }
                            });
                        },
                        Err(e) => eprintln!("Error decoding base64 image: {}", e),
                    }
                }
            },
            Some("text") => {
                // Handle text data
            },
            _ => {
                // Handle other message types or ignore
            }
        }
    }
   
    // Rest of implementation block omitted for brevity
```

## Technology Stack

- Rust - to include, but not be limited to the following libraries:
    - `Reqwest` for HTTP requests
    - `Serde` and `Serde_json` for JSON handling
    - `Tokio` and `async-trait` for async
    - `lazy_static` for compute and memory efficient state management
    - `log` and `env_logger` for logging
    - `clap` for CLI
    - [Optional]`actix-web` or similar for Web UI. Alternatively, we could use a different language for the Web UI, such as Python or JavaScript.
- Kubernetes
  - `K8's` for Cloud
  - `K3's` for on-prem/edge
- OpenAI API
  - `GPT-4`
  - `GPT-Vision`
  - `Code Interpreter`
  - `Assistants API/GPT's` for pre-specialised Agents

- Web UI [TBC] options include:
  - JavaScript/TypeScript React App or similar
  - Rust using `actix-web` or similar popular Rust web framework
  - Python using `Flask` or similar popular Python web framework
  - [Proposal] - A hybrid approach with Rust backend and JavaScript/TypeScript React App frontend could work well in our case.
- Sand-boxed Docker Container akin to that which Shuttle.rs use. (For evaluation/MVP purposes we can use Google Cloud Functions).

- [Optional] Storage:
  - PostGres DB hosted in AWS RDS or similar could be used for our own Cloud-based storage solution if we deem it necessary.
  - A PostGres DB hosted in a Docker container or similar could work well for a user's edge/local compute purposes.
  - A lightweight storage solution could be used for the Agent registry, such as Redis or similar.
  - SQLite could be used for the Agent registry in an on-prem/edge environment.

## Security Considerations

The primary considerations on the security front are:
- Private data - we want to enable users to keep their data private.
- Arbitrary code execution - we need to isolate ourselves and the customer from the effects of potentially malicious arbitrary code execution both locally and in the cloud.
- Authentication - we need to ensure that only authorized users can access the system.
- Authorization - we need to ensure that only authorized users can access the data and functions that they are authorized to access.
- Encryption - we need to ensure that data is encrypted in transit and at rest.
- Logging - we need to ensure that logs are secure and only accessible to authorized users. They should also be appropriately stored for audit purposes.

### Proposed Solutions

#### Private Data
We should have an onboarding workflow that enables users to set up their own private cloud or on-prem/edge deployment.

Private data should not be stored in our application or on our servers; in the case of images, these should be temporarily rendered from the user's storage solution via ephemeral signed urls.

#### Arbitrary Code Execution
We should use a sand-boxed Docker container to isolate ourselves and the customer from the effects of potentially malicious arbitrary code execution. 
[Shuttle.rs](https://shuttle.rs) solve this problem in a similar way for their hosting solution, also built in Rust.

#### Authentication, Authorization, and Encryption
We should use a standard authentication and authorization solution, such as OAuth2.0, to ensure that only authorized users can access the system and the data and functions that they are authorized to access.

We should use standard encryption solutions:
- HTTPS over TLS v1.3, to ensure that data is encrypted in transit. Assuming we are using a WAF for our cloud platform, termination should take place at the load balancer level.
- AES-256 for storage at rest (taking care not to store anything unless it is strictly necessary). Key management can be handled by the user's existing storage solution, or AWS KMS, or similar if we implement a cloud storage solution ourselves.

#### Logging
We should use standard logging solutions, such as `log` and `env_logger`, to ensure that logs are secure and only accessible to authorized users. They should also be appropriately stored for audit purposes. (This is out of the current scope of this proposal).

## Performance Considerations

Bruce Lee once said: "It is daily decrease, not daily increase, that makes the martial artist".

So too with code, we should seek to keep our code as lightweight and performant as possible, erring on the side of edge-first computing.

To that end, we propose the following performance considerations. We should:
- Take an edge-first approach, meaning the core system should be lightweight and performant enough to run on a TuringPi 7 x Raspberry Pi cluster board.
- Use Rust, which is the **most powerful programming language for the task** (drawing from Richard W. Hamming's "You and Your Research" talk).
- Use asynchronous programming, which is fast and memory efficient and in particular make use of `lazy_static` for efficient state management.
- Use K3's, which is a lightweight Kubernetes distribution designed for edge computing.
- Have a configurable concurrency level that is appropriate for the user's requirements and system resources but can be overridden by power users operating in their own environments.

## Scalability, Reliability and Testing

- We should use Kubernetes, which is designed for scalability and reliability.
- Autoscaling should be enabled.
- Appropriate performance and health checks should be implemented.
- Unit and integration tests should be implemented and run as part of the CI/CD pipeline to ensure that the system is reliable. Tests should include a representative spread of data and contexts. 
- N.B. When testing, we should always test the functionality not the implementation, such that a later refactoring does not significantly mutate the expected behaviour of the existing tests.

## Risks and Mitigations

The main risks are:
- Breaking changes to the OpenAI API
- Disruption in the AI LLM space
- Malicious code execution as discussed in the Security Considerations section above

Dealing with each in turn we propose:
- Ensuring the API integration itself is as lightweight as possible and that it is logically separated from the internal application logic.
- Ensuring we can swap out the API with relative ease both for local and cloud deployments.
- Ensuring that we use a sand-boxed Docker container to isolate ourselves and the customer from the effects of potentially malicious arbitrary code execution both locally and in the cloud.
- Utilising our strong Developer Community to ensure we stay abreast of the latest developments in the AI LLM space and can react accordingly.

## Future Work

- We should consider how best to implement the Web UI. We could use Rust, Python, or JavaScript.
- We should build out a more detailed drill down into the user journeys that will inform the UI development.
- Adding support for RAG and fine-tuning would be a great next step.
- Adding an LLM-powered natural language orchestration/setup interface would ensure a seamless user experience.

## Appendices

The PUML code used to generate the diagrams contained in this repo can be found here: [PUML](https://github.com/postrv/autorust/blob/master/README.md#puml-diagrams)

The MVP code can be found here [TuringPi-Rust-AI-Agent-Platform](https://github.com/postrv/autorust/tree/master)

## Approval and Feedback

### Outstanding Questions:
- How best to implement the Web UI? Rust, Python, or JavaScript?
- How best to handle multimodal (text and image) data in messages?
- Should we implement a storage solution or rely on the user's existing storage solution?

### Feedback:
- Please provide feedback and comments on this proposal by creating an issue in this repo by Friday 12 January 2024.

Thanks for reading!