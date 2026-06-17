# Phase 1: Basic Gateway & Rust Engine Ping

## Domain
Establish the foundational split-service architecture (Go control plane + Rust data plane) and ensure they can communicate via gRPC.

## Locked Requirements (from PROJECT.md / REQUIREMENTS.md)
- Define a stable gRPC contract between Go and Rust.
- Build a Go API gateway handling HTTP and metadata persistence.
- Build a Rust RAG engine with a gRPC server and async runtime.
- Provide local development path (Docker Compose, `go run`, `cargo run`).

## Implementation Decisions

### Service Boundaries & Communication
- **Decision:** Use **gRPC** over a structured Protobuf schema for all inter-service communication.
- **Rationale:** Ensures strict typing and performance for the data-plane connection.

### Core Frameworks
- **Decision:** Go for the API Gateway (standard library `net/http` or lightweight router like `chi`). Rust for the RAG Engine (using `tonic` for gRPC, `tokio` for async).
- **Rationale:** Maximizes systems engineering signaling. Avoids black-box frameworks.

### Build & Dev Environment
- **Decision:** Hybrid local-first (`go run` / `cargo run`) backed by Docker Compose for PostgreSQL (metadata) and Jaeger (traces).
- **Rationale:** Fast development loop while maintaining a production-like dependencies setup.

## Code Context
- **Reusable Assets:** Standard Protobuf definitions for the Ping/Health service.
- **Patterns:** Go dependency injection for DB access; Rust `tokio` runtime initialization and `tonic` server setup.

## Canonical References
- `.discussion/final_implementation_decision_document.md`
- `.discussion/lightweight_state_machine_plan.md`

## Deferred Ideas
- Document ingestion, chunking, and LanceDB storage (Deferred to Phase 2).
- Hybrid Retrieval and RAG logic (Deferred to Phase 3).
- OpenTelemetry tracing (Deferred to Phase 6, though basic scaffolding can start now if convenient).
