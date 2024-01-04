# TuringPi - Rust AI Agent Platform - RFC


## AutoRust RFC

### Author:
Laurence Avent

### Date:
Wednesday 3 Jan 2024


## Abstract
A Rust-Based AI Agent orchestration platform is proposed. The core system will comprise a configurable set of agents, a registry, and a manager. 
The system will be deployed on Kubernetes and be able to scale up and down according to the user's requirements.
The MVP will be able to orchestrate a set of agents to perform a simple task, such as reading a sketch of a proposed system and turning it into specs/pseudocode, then into code, which can be checked and run by other agents.
The full implementation should unlock RAG, fine-tuning, and other advanced features of the OpenAI API.
We anticipate that individual developers (e.g. those already in the TuringPi Community) and businesses will find great utility in locally hosted AI agents so that they can keep their data private and avoid the costs of cloud-based AI agents. 
This system is therefore designed such that it can be relatively trivially adopted in an on-prem/edge environment.

### Purpose

The purpose of this proposal is to outline the design of this system and to provide a basis for discussion and feedback.

### Scope

The scope of this proposal is limited to the design of the system. 

It is not intended to form a detailed technical specification and implementation details may change, especially those relating to the frontend and user experience.

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
11. [Scalability and Reliability](#scalability-and-reliability)
12. [Cost Analysis](#cost-analysis)
13. [Risks and Mitigations](#risks-and-mitigations)
14. [Alternatives Considered](#alternatives-considered)
15. [Future Work](#future-work)
16. [Appendices](#appendices)
17. [References](#references)
18. [Approval and Feedback](#approval-and-feedback)


## Introduction

Microsoft have open-sourced their AI Agent framework, which is written in Python. 
See:[Autogen](https://github.com/microsoft/autogen)
This allows for orchestration, but is limited to Python and the OpenAI API.
It is also not designed for on-prem/edge deployment. 

We therefore see the need for a Rust-based AI Agent Platform that can be deployed on-prem/edge and can be used to orchestrate a variety of agents, including those that are not based on the OpenAI API.


## Problem Statement



## Stakeholders
- Laurence Avent
- TuringPi
- TuringPi Community
- TuringPi Customers
- TuringPi Investors
- Rust Community
- Raspberry Pi Community
- LLM Enjoyers the World Over

# Goals and Non-Goals

## Goals
- Create a Rust-based AI Agent Platform that can be used to create AI agents for a variety of purposes.
- Enable users to define custom workflows and hierarchies for AI agents
- Expand the TuringPi ecosystem
- We want to remain reactive to the rapidly shifting AI landscape. 
- We will use the OpenAI API and other existing AI agents for now but will be able to swap these out as the landscape changes.

## Non-Goals
- We are not trying to reinvent the wheel. We will use existing libraries and frameworks where possible.
- We do not wish to lock ourselves into a specific ecosystem or vendor. We will use open-source technologies and frameworks where possible.

## Architectural Overview

The proposed system consists of the following components:
- Agent Manager: Manages the agents and the agent registry.
- Agent Registry: Maintains a list of agents, their IDs, and functions.
- Agents for specific roles: e.g. GPT Vision, Coding Agent, Code Interpreter, Code Checker (see example).
- Message Handling: JSON and image data communication, in line with the OpenAI API.
- OpenAI API Communication: External system interaction using the OpenAI API and authentication token.
- Error Handling and Regeneration: Manages errors from the OpenAI API and coding errors.
- Logging and Monitoring: Implements logging and monitoring with common Rust libraries.

The following diagram illustrates the proposed generic high-level architecture:

[![Architecture Diagram](async.png)](../images/async.png)

The following diagram illustrates a more comprehensive example configuration:

[![Architecture Diagram - Detailed](async-advanced.png)](../images/async-advanced.png)

In this example, there are 4 agents arranged such that:
- One can read a sketch of a proposed system and turn it into specs/pseudocode (GPT Vision Agent)
- One can turn the specs/pseudocode into code (Coding Agent)
- One can run the code and return the results (Code Interpreter Agent)
- One can check the output for errors (Code Checker Agent)

The user should be able to specify the number of agents for each role, the hierarchy and the order of operations, and the system should be able to scale up and down accordingly.

This system is designed such that it can be relatively trivially adopted in an on-prem/edge environment and will run on K8's or K3's. 

Taking advantage of Rust's fearless concurrency, we will be able to scale up and down according to the user's requirements and system resources.

## Detailed Design

### MVP/Proof of Concept
An MVP framework for the system has been developed and is available here: [TuringPi-Rust-AI-Agent-Platform](https://github.com/postrv/autorust)

### Learning Points
The MVP exercise has highlighted the following learning points:
- Rust is a great language for this kind of project. It is fast, safe, and has great concurrency features.
- Due consideration must be given to the handling of multimodal (text and image) data in messages.
- The system must be able to handle errors from the OpenAI API and coding errors.
- An asynchronous approach is required to handle and benefit from the concurrent nature of the system.
- 

## Technology Stack

- Rust
    - Reqwest
    - Serde
    - Tokio
  
- Kubernetes
  - Kube-rs
  - Kubelet
  - Kubectl
  - Kube-proxy
  - Kube-scheduler
  - Kube-controller-manager
  - Etcd
  - Kube-apiserver
  - Kubelet
- OpenAI API
 - GPT-4
 - GPT-Vision
- CLI and Web UI
- Private data, on-prem/edge deployment
- Sand-boxed Docker Container or, for evaluation/MVP purposes Google Cloud Functions.


## Security Considerations



## Performance Considerations



## Scalability and Reliability



## Cost Analysis



## Risks and Mitigations



## Alternatives Considered



## Future Work



## Appendices



## References



## Approval and Feedback



