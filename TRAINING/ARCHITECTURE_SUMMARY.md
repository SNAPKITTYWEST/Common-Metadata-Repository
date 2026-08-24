# OpenMetadata — Architecture Summary

## Domain
Open data context platform — unified metadata catalog, AI governance, data lineage, quality, and semantic search for humans and AI agents

## Architecture Type
Multi-module Maven monorepo: Java REST backend (Dropwizard/JAX-RS) + React SPA + Python ingestion framework + schema-first JSON Schema source of truth + MCP server for AI agents

## Primary Language
TypeScript (UI) / Java (backend) / Python (ingestion)

## Major Components
- openmetadata-spec (904 JSON Schemas + generated POJOs)
- openmetadata-service (Dropwizard REST API, JDBI3 repositories, migration runner, ES/OS search, governance workflows)
- openmetadata-ui (React/TypeScript SPA, 4725 TS/TSX files)
- ingestion/ (Python framework, 130+ connectors, Source→Sink pipeline)
- openmetadata-mcp (MCP server for AI agents)
- openmetadata-k8s-operator (Kubernetes operator)
- openmetadata-sdk (Java client SDK)
- openmetadata-integration-tests (API ITs)
- openmetadata-ui-core-components (React component library)
- common/ (shared Java utilities)
- openmetadata-shaded-deps (shaded ES+OS clients)

## Data Stores
- MySQL (default relational store)
- PostgreSQL (alternative relational store)
- Elasticsearch 7.17+ (search index)
- OpenSearch 2.6+ (alternative search index)

## External Interfaces
- REST API (JAX-RS, Dropwizard, port 8585)
- MCP server (Model Context Protocol for AI agents: Claude Desktop, Cursor, VS Code, Windsurf)
- Python ingestion CLI (metadata CLI, __main__.py)
- Apache Airflow (ingestion orchestration)
- WebSocket (real-time notifications)
- OpenLineage events (lineage ingestion)
- Webhooks (change event publishing)

## Evidence Files Read
- README.md
- ARCHITECTURE.md
- CLAUDE.md
- pom.xml
- .devcontainer/full-stack/docker-compose.yml
- openmetadata-service/src/main/java/org/openmetadata/service/OpenMetadataApplication.java
- ingestion/src/metadata/__main__.py
- ingestion/src/metadata/ingestion/sink/metadata_rest.py
- openmetadata-spec/src/main/resources/json/schema/entity/data/table.json
- openmetadata-spec/src/main/resources/json/schema/entity/data/dataContract.json
- openmetadata-spec/src/main/resources/json/schema/entity/ai/aiApplication.json
- openmetadata-mcp/src/main/java/org/openmetadata/mcp/AuthEnrichedMcpContextExtractor.java
- openmetadata-service/src/main/java/org/openmetadata/service/jdbi3/EntityRepository.java
- openmetadata-service/src/main/java/org/openmetadata/service/resources/databases/TableResource.java
- openmetadata-service/src/main/java/org/openmetadata/service/resources/search/SearchResource.java
- openmetadata-service/src/main/java/org/openmetadata/service/resources/lineage/LineageResource.java
- openmetadata-service/src/main/java/org/openmetadata/service/search/SearchRepository.java
- openmetadata-service/src/main/java/org/openmetadata/service/events/ChangeEventHandler.java
- openmetadata-service/src/main/java/org/openmetadata/service/governance/workflows/Workflow.java
- openmetadata-service/src/main/java/org/openmetadata/service/apps/bundles/searchIndex/SearchIndexApp.java
- openmetadata-service/src/main/java/org/openmetadata/service/jdbi3/CollectionDAO.java
- openmetadata-service/src/main/java/org/openmetadata/service/security/DefaultAuthorizer.java
- ingestion/src/metadata/ingestion/api/steps.py
- ingestion/src/metadata/ingestion/source/database/common_db_source.py
- ingestion/src/metadata/ingestion/source/database/snowflake/metadata.py
- ingestion/src/metadata/ingestion/ometa/ometa_api.py
- openmetadata-ui/src/main/resources/ui/src/App.tsx
- openmetadata-ui/src/main/resources/ui/src/rest/tableAPI.ts
- openmetadata-integration-tests/src/test/java/org/openmetadata/it/tests/TableResourceIT.java
