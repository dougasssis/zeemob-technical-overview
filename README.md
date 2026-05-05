# Zeemob Real Estate Backend

## Note on Source Code Access:
"The source code for this project is proprietary and belongs to Zeemob. As a lead developer, I maintain strict adherence to Non-Disclosure Agreements (NDAs) and professional ethics, meaning the private codebase cannot be shared publicly.

This repository serves as a Technical Architectural Overview. It documents the design patterns, infrastructure choices, and AI integration strategies I implemented to build a resilient, scalable, and auditable distributed system using .NET 10, Marten (Event Sourcing), and .NET Aspire."

## Project Overview
Zeemob is a comprehensive Real Estate Backend designed to streamline sales and operational workflows for real estate agencies. It serves as the central hub for managing properties, leads, communications, and marketing integrations, enabling agents to operate more efficiently through automated processes and centralized data management.

## Architecture & Patterns
The solution is built on modern architectural principles to ensure maintainability, scalability, and domain clarity:
- **Clean Architecture:** The codebase is structured to separate concerns, with inner layers defining the domain and outer layers handling infrastructure, persistence, and external APIs.
- **Domain-Driven Design (DDD):** Complex business rules are encapsulated within aggregates, entities, and value objects, ensuring the code closely maps to the real estate business domain.
- **CQRS (Command-Query Responsibility Segregation):** Read and write operations are separated, allowing for optimized data models for complex queries and distinct validation logic for commands.
- **Event Sourcing:** Critical state changes and domain events are persisted to build an immutable history of aggregate states, enabling robust auditing and complex workflow orchestrations.

## Infrastructure Orchestration (.NET Aspire)
The project leverages **.NET Aspire** to orchestrate distributed cloud-native services:
- **AppHost Configuration:** Centralized definition of all microservices, workers, and infrastructure dependencies.
- **Service Discovery & Interconnection:** Aspire manages endpoint bindings and service discovery seamlessly across the distributed architecture.
- **Resource Management:** Dependencies such as PostgreSQL for relational data/vector storage, RabbitMQ for message brokering, and Redis for distributed caching are seamlessly provisioned and injected as resources into the development environment.

## Asynchronous Messaging
To achieve high decoupling and responsiveness, the system heavily relies on asynchronous messaging:
- **MassTransit:** Serves as the primary enterprise service bus, orchestrating message delivery across bounded contexts.
- **Saga State Machines:** Complex distributed transactions and long-running workflows (e.g., lead onboarding, property publication across platforms) are managed using MassTransit Sagas, ensuring eventual consistency.
- **Exchanges & Consumers:** Strategically configured fan-out and topic exchanges ensure domain events are properly routed to specialized consumer strategies without tightly coupling services.

## AI Integration & RAG
The platform incorporates advanced AI capabilities to assist agents and automate interactions:
- **RAG Pipeline Retrieval-Augmented Generation):** Enhances LLM prompts with semantic search results from the internal knowledge base and property listings.
- **pgvector:** Used within PostgreSQL to store and query highly dimensional vector embeddings for rapid similarity searches.
- **Strategy & Factory Patterns:** AI integrations are decoupled from specific LLM providers. Factory and Strategy patterns dynamically resolve the appropriate provider (e.g., OpenAI, local models) based on configuration and context, ensuring flexibility and preventing vendor lock-in.

## Resilience & Reliability
Building a robust distributed system requires proactive fault tolerance:
- **Exponential Backoffs & Circuit Breakers:** Implemented to handle transient failures when interacting with third-party APIs (e.g., Idealista, Meta Ads, PipeDrive).
- **Kill Switches:** Mechanism to gracefully disable specific integrations or features during downstream outages.
- **Options Pattern:** Strongly typed configuration management ensures safe binding of environment variables and application settings, combined with validation to fail fast during startup if configurations are missing.

## Scalability
The architecture is designed to handle high concurrency and ensure data consistency:
- Microservices can scale independently based on load (e.g., scaling the Meta Ads worker independently from the main API).
- Asynchronous processing offloads heavy tasks (file uploads, AI processing) to background workers.
- Eventual consistency is maintained across bounded contexts via outbox patterns and message retries, ensuring no data loss even under high load.

